---
name: auditoria-de-buckets-s3
description: Audita as permissões de buckets AWS S3 para achar buckets públicos, ACLs permissivas demais, políticas mal configuradas e falta de criptografia, usando AWS CLI, Prowler e IAM Access Analyzer. Use em avaliações de segurança da AWS, alertas de exposição de dados no S3 ou revisões periódicas de acesso.
---

# Auditoria de permissões de buckets AWS S3

Audite de forma sistemática as permissões dos buckets S3 da AWS para identificar buckets acessíveis publicamente, ACLs permissivas demais, políticas de bucket mal configuradas e falta de criptografia, aplicando o princípio do menor privilégio no acesso aos dados.

## Quando usar

- Em avaliações de segurança de ambientes AWS, para identificar dados expostos publicamente
- Ao incluir uma nova conta AWS e definir a linha de base de segurança do armazenamento
- Ao responder a um alerta de possível exposição no S3 vindo do AWS Trusted Advisor ou do Security Hub
- Quando frameworks de conformidade (SOC 2, PCI DSS, ISO 27001, LGPD) exigem revisão periódica dos controles de acesso a dados
- Quando um vazamento ou comprometimento de credenciais exige revisão imediata de todos os recursos S3 acessíveis

**Não use** para auditar armazenamento de objetos fora da AWS (use as ferramentas do provedor), para monitoramento em tempo real (use S3 Event Notifications com Lambda) ou para analisar padrões de acesso (use o S3 Access Analyzer ou os eventos de dados do S3 no CloudTrail).

## Pré-requisitos

- AWS CLI v2 configurada com credenciais que tenham as permissões `s3:GetBucketPolicy`, `s3:GetBucketAcl`, `s3:GetBucketPublicAccessBlock`, `s3:GetEncryptionConfiguration` e `s3:ListAllMyBuckets`
- Prowler instalado (`pip install prowler`) para checagens automáticas do benchmark CIS
- S3audit ou ferramenta parecida para detectar rapidamente buckets públicos
- Acesso ao AWS Organizations, se a auditoria cobrir várias contas
- Python 3.8+ com boto3 para scripts de auditoria próprios

## Passo a passo

### Passo 1: Listar os buckets e o bloqueio de acesso público da conta

Verifique primeiro a configuração de S3 Block Public Access no nível da conta e depois liste todos os buckets com suas regiões.

```bash
# Bloqueio de acesso público no nível da conta
aws s3control get-public-access-block \
  --account-id $(aws sts get-caller-identity --query Account --output text) \
  --output json

# Lista todos os buckets com data de criação
aws s3api list-buckets \
  --query 'Buckets[*].[Name,CreationDate]' \
  --output table

# Região de cada bucket
for bucket in $(aws s3api list-buckets --query 'Buckets[*].Name' --output text); do
  region=$(aws s3api get-bucket-location --bucket "$bucket" --query 'LocationConstraint' --output text)
  echo "$bucket -> ${region:-us-east-1}"
done
```

### Passo 2: Verificar o bloqueio de acesso público e a ACL de cada bucket

Percorra todos os buckets avaliando o bloqueio de acesso público individual e as permissões concedidas por ACL.

```bash
# Bloqueio de acesso público por bucket
for bucket in $(aws s3api list-buckets --query 'Buckets[*].Name' --output text); do
  echo "=== $bucket ==="
  aws s3api get-public-access-block --bucket "$bucket" 2>/dev/null || echo "  Sem Block Public Access configurado"

  # ACL com permissões públicas
  aws s3api get-bucket-acl --bucket "$bucket" \
    --query 'Grants[?Grantee.URI==`http://acs.amazonaws.com/groups/global/AllUsers` || Grantee.URI==`http://acs.amazonaws.com/groups/global/AuthenticatedUsers`]' \
    --output json
done
```

### Passo 3: Analisar políticas de bucket permissivas demais

Revise as políticas de bucket em busca de principal curinga, falta de condições e declarações que liberam acesso amplo.

```bash
# Extrai e analisa as políticas de bucket
for bucket in $(aws s3api list-buckets --query 'Buckets[*].Name' --output text); do
  policy=$(aws s3api get-bucket-policy --bucket "$bucket" --query Policy --output text 2>/dev/null)
  if [ -n "$policy" ]; then
    echo "=== política de $bucket ==="
    echo "$policy" | python3 -c "
import json, sys
policy = json.load(sys.stdin)
for stmt in policy.get('Statement', []):
    principal = stmt.get('Principal', {})
    effect = stmt.get('Effect', '')
    if principal == '*' or principal == {'AWS': '*'}:
        print(f'  ALERTA: {effect} com principal curinga')
        print(f'  Ações: {stmt.get(\"Action\", \"\")}')
        print(f'  Condição: {stmt.get(\"Condition\", \"NENHUMA\")}')
"
  fi
done
```

### Passo 4: Verificar criptografia e versionamento

Confirme que todos os buckets têm criptografia no servidor e versionamento configurados.

```bash
# Criptografia, versionamento e logs de cada bucket
for bucket in $(aws s3api list-buckets --query 'Buckets[*].Name' --output text); do
  echo "=== $bucket ==="

  # Configuração de criptografia
  aws s3api get-bucket-encryption --bucket "$bucket" 2>/dev/null \
    && echo "  Criptografia: ATIVA" \
    || echo "  Criptografia: INATIVA"

  # Status do versionamento
  aws s3api get-bucket-versioning --bucket "$bucket" \
    --query 'Status' --output text

  # Status dos logs de acesso
  aws s3api get-bucket-logging --bucket "$bucket" \
    --query 'LoggingEnabled' --output text 2>/dev/null
done
```

Observação: desde janeiro de 2023 a AWS aplica criptografia SSE-S3 por padrão em objetos novos. Por isso, o mais relevante é verificar se buckets com dados sensíveis usam SSE-KMS com chave gerenciada pela organização.

### Passo 5: Rodar as checagens de S3 do Prowler

Execute as checagens de S3 do Prowler alinhadas ao CIS AWS Foundations Benchmark.

```bash
# Checagens específicas de S3 no Prowler
prowler aws \
  --checks s3_bucket_public_access \
           s3_bucket_default_encryption \
           s3_bucket_policy_public_write_access \
           s3_bucket_server_access_logging_enabled \
           s3_bucket_versioning_enabled \
           s3_bucket_acl_prohibited \
  -M json-ocsf \
  -o ./prowler-s3-audit/

# Resumo de todo o serviço S3
prowler aws --service s3 -M csv -o ./prowler-s3-audit/
```

### Passo 6: Usar o IAM Access Analyzer para acessos públicos e entre contas

Use o IAM Access Analyzer para identificar buckets compartilhados externamente ou publicamente.

```bash
# Cria um analisador, se ainda não existir
aws accessanalyzer create-analyzer \
  --analyzer-name s3-access-audit \
  --type ACCOUNT

# Lista os achados do Access Analyzer para S3
aws accessanalyzer list-findings \
  --analyzer-arn $(aws accessanalyzer list-analyzers --query 'analyzers[0].arn' --output text) \
  --filter '{"resourceType": {"eq": ["AWS::S3::Bucket"]}}' \
  --query 'findings[*].[resource,status,condition,principal]' \
  --output table
```

### Passo 7: Gerar o relatório e remediar

Compile os achados em um relatório acionável e aplique a remediação dos problemas críticos.

```bash
# Ativa o Block Public Access em um bucket
aws s3api put-public-access-block \
  --bucket BUCKET_ALVO \
  --public-access-block-configuration \
  'BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true'

# Ativa a criptografia padrão com SSE-KMS
aws s3api put-bucket-encryption \
  --bucket BUCKET_ALVO \
  --server-side-encryption-configuration \
  '{"Rules":[{"ApplyServerSideEncryptionByDefault":{"SSEAlgorithm":"aws:kms","KMSMasterKeyID":"alias/aws/s3"},"BucketKeyEnabled":true}]}'

# Ativa o versionamento
aws s3api put-bucket-versioning \
  --bucket BUCKET_ALVO \
  --versioning-configuration Status=Enabled
```

## Conceitos-chave

| Termo | Definição |
|-------|-----------|
| S3 Block Public Access | Configuração no nível da conta e do bucket que se sobrepõe a ACLs e políticas, impedindo acesso público independentemente da configuração de cada recurso |
| Política de bucket | Política em JSON associada ao bucket que define quem pode acessá-lo e quais ações pode executar |
| ACL (lista de controle de acesso) | Mecanismo legado do S3 que concede permissões a contas AWS ou a grupos predefinidos, como AllUsers e AuthenticatedUsers |
| IAM Access Analyzer | Serviço da AWS que analisa políticas de recursos para identificar o que está compartilhado com entidades externas ou com o público |
| Criptografia no servidor | Criptografia aplicada pelo S3 a cada objeto, com SSE-S3, SSE-KMS ou SSE-C, antes de gravar os dados em disco |
| CIS AWS Foundations Benchmark | Padrão de boas práticas do Center for Internet Security com controles específicos para configuração de buckets S3 |

## Ferramentas

- **AWS CLI**: interface principal para consultar configurações, políticas, ACLs e criptografia dos buckets
- **Prowler**: ferramenta open source com mais de 50 checagens de S3 alinhadas a CIS, PCI DSS e HIPAA
- **IAM Access Analyzer**: serviço nativo da AWS que monitora continuamente políticas que concedem acesso externo
- **S3audit**: ferramenta leve para listar rapidamente buckets públicos de uma conta
- **ScoutSuite**: auditoria multinuvem que coleta a configuração do S3 e gera relatórios HTML com pontuação de risco

## Cenário comum

### Cenário: bucket com dados de clientes legível publicamente

**Contexto**: um engenheiro de segurança recebe um alerta do Trusted Advisor sobre um bucket S3 acessível publicamente. O bucket foi criado por um time de desenvolvimento para uma demonstração e nunca foi fechado.

**Abordagem**:
1. Rode `aws s3api get-bucket-acl` e encontre uma permissão para `AllUsers` com `READ`
2. Consulte `get-bucket-policy` e encontre uma política com `Principal: "*"` e `s3:GetObject`
3. Confirme que o Block Public Access não está ativo nem no bucket nem na conta
4. Liste o conteúdo do bucket para avaliar a sensibilidade dos dados
5. Ative imediatamente o Block Public Access no bucket
6. Revise os eventos de dados do S3 no CloudTrail para saber se houve acesso não autorizado
7. Reporte o achado com linha do tempo, inventário dos dados e confirmação da remediação. Se houver dados pessoais expostos, acione o encarregado de dados (DPO) para avaliar a comunicação prevista na LGPD

**Armadilhas**: ativar o Block Public Access pode quebrar aplicações que servem conteúdo público de propósito (sites estáticos). Sempre confirme o uso pretendido do bucket antes de aplicar restrições e verifique se há distribuições do CloudFront ou outros serviços dependendo do acesso público.

## Formato de saída

```
Relatório de auditoria de permissões de buckets S3
====================================================
Conta: 123456789012 (Produção)
Data: 2026-02-23
Auditor: Time de Engenharia de Segurança
Total de buckets: 47

CONFIGURAÇÃO DA CONTA:
  Block Public Access: ATIVO (as quatro opções)

ACHADOS CRÍTICOS:
[S3-001] Leitura pública via ACL
  Bucket: marketing-assets-prod
  Problema: grupo AllUsers com permissão READ via ACL
  Risco: qualquer pessoa na internet pode listar e baixar o conteúdo do bucket
  Sensibilidade: material de marketing voltado ao cliente, não sensível
  Remediação: remover a permissão AllUsers da ACL e ativar o Block Public Access

[S3-002] Principal curinga na política do bucket
  Bucket: data-exchange-partner
  Problema: política libera s3:GetObject com Principal "*" sem condição de VPC/IP
  Risco: feito para acesso de parceiro, mas acessível a qualquer um que saiba o nome do bucket
  Remediação: adicionar condição aws:SourceVpce ou aws:SourceIp para restringir o acesso

RESUMO:
  Buckets com acesso público:            3 / 47
  Buckets sem criptografia KMS:          5 / 47
  Buckets sem versionamento:            12 / 47
  Buckets sem log de acesso:            18 / 47
  Buckets com políticas amplas demais:   7 / 47
```

---

## Créditos e licença

Skill adaptada e traduzida para o português a partir da skill `auditing-aws-s3-bucket-permissions` (versão 1.0), de autoria de **mahipal**, publicada em [mukul975/Anthropic-Cybersecurity-Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills) sob a licença **Apache 2.0**.

Alterações desta versão: tradução para o português do Brasil, remoção dos metadados de mapeamento (MITRE ATT&CK, NIST CSF), correções nos comandos (extração do campo `Policy` da política, comentário de criptografia SSE-KMS, `--service s3` no resumo do Prowler e criação do analisador antes da consulta), nota sobre a criptografia padrão do S3, inclusão de referências à LGPD, e não inclusão das pastas `scripts` e `references` do original.
