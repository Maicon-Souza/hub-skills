---
name: investigacao-de-phishing
description: Investiga incidentes de phishing do relato do usuário até a contenção (análise de cabeçalhos, URLs e anexos, quem recebeu, quem clicou, expurgo e bloqueio). Use quando um e-mail suspeito reportado precisa de investigação completa de escopo e impacto.
---

# Investigação de incidente de phishing

Conduza a investigação de um e-mail de phishing do relato inicial até a contenção e o relatório final, usando ferramentas de SOC como Splunk (ou outro SIEM), Microsoft Defender for Office 365 e sandboxes de análise.

## Quando usar

- Um usuário reportou um e-mail suspeito pelo botão de "reportar phishing" ou por chamado no helpdesk
- O gateway de e-mail sinalizou uma mensagem que passou pelos filtros iniciais
- Uma detecção automática identificou URL de captura de credenciais ou anexo malicioso
- Uma campanha de phishing contra a organização precisa ter o escopo avaliado

**Não use** para spam ou e-mail marketing sem intenção maliciosa. Encaminhe esses casos para a administração de e-mail ajustar os filtros.

## Pré-requisitos

- Acesso aos logs do gateway de e-mail (Proofpoint, Mimecast ou Microsoft Defender for Office 365)
- Splunk ou outro SIEM recebendo logs de e-mail (Message Trace do O365, logs de rastreamento do Exchange)
- Acesso a sandbox (Any.Run, Joe Sandbox ou Hybrid Analysis) para detonar URLs e anexos
- Microsoft Graph API ou Exchange Admin Center para busca e expurgo de e-mails
- Chaves de API do URLScan.io e do VirusTotal

## Passo a passo

### Passo 1: Extrair e analisar os cabeçalhos

Obtenha os cabeçalhos completos (arquivo `.eml`) da mensagem reportada:

```python
import email
from email import policy

with open("phishing_sample.eml", "rb") as f:
    msg = email.message_from_binary_file(f, policy=policy.default)

# Cabeçalhos principais
print(f"From: {msg['From']}")
print(f"Return-Path: {msg['Return-Path']}")
print(f"Reply-To: {msg['Reply-To']}")
print(f"Subject: {msg['Subject']}")
print(f"Message-ID: {msg['Message-ID']}")
print(f"X-Originating-IP: {msg['X-Originating-IP']}")

# Cabeçalhos Received (de baixo para cima mostram a origem real)
for header in reversed(msg.get_all('Received', [])):
    print(f"Received: {header[:120]}")

# Resultados de autenticação
print(f"Authentication-Results: {msg['Authentication-Results']}")
print(f"DKIM-Signature: {msg.get('DKIM-Signature', 'NONE')[:80]}")
```

O que verificar:
- **SPF**: o domínio do `Return-Path` bate com o IP de envio? Procure `spf=pass` ou `spf=fail`
- **DKIM**: a assinatura é válida? `dkim=pass` confirma que o e-mail não foi alterado no caminho
- **DMARC**: o domínio do `From` está alinhado com os domínios de SPF/DKIM? `dmarc=fail` indica falsificação (spoofing)

### Passo 2: Analisar URLs e anexos

**Análise de URL:**

```python
import requests

# Envia a URL para o URLScan.io
url_to_scan = "https://evil-login.example.com/office365"
response = requests.post(
    "https://urlscan.io/api/v1/scan/",
    headers={"API-Key": "SUA_CHAVE", "Content-Type": "application/json"},
    json={"url": url_to_scan, "visibility": "unlisted"}
)
scan_id = response.json()["uuid"]
print(f"Resultado: https://urlscan.io/result/{scan_id}/")

# Reputação da URL no VirusTotal
import vt
client = vt.Client("SUA_CHAVE_VT")
url_id = vt.url_id(url_to_scan)
url_obj = client.get_object(f"/urls/{url_id}")
print(f"Pontuação VT: {url_obj.last_analysis_stats}")
client.close()
```

**Análise de anexo:**

```python
import hashlib

# Calcula os hashes do arquivo
with open("attachment.docx", "rb") as f:
    content = f.read()
    md5 = hashlib.md5(content).hexdigest()
    sha256 = hashlib.sha256(content).hexdigest()

print(f"MD5: {md5}")
print(f"SHA256: {sha256}")

# Consulta no MalwareBazaar
response = requests.post(
    "https://mb-api.abuse.ch/api/v1/",
    data={"query": "get_info", "hash": sha256}
)
print(response.json()["query_status"])
```

Envie o anexo para uma sandbox (Any.Run ou Joe Sandbox) para análise dinâmica de macros, execução de PowerShell e conexões com servidores de comando e controle (C2).

### Passo 3: Determinar o escopo da campanha

Busque no Splunk todos os destinatários do mesmo e-mail:

```spl
index=email sourcetype="o365:messageTrace"
(SenderAddress="attacker@evil-domain.com" OR Subject="Urgent: Password Reset Required"
 OR MessageId="<phishing-message-id@evil.com>")
earliest=-7d
| stats count by RecipientAddress, DeliveryStatus, MessageTraceId
| sort - count
```

Ou use a Microsoft Graph API:

```python
import requests

headers = {"Authorization": f"Bearer {access_token}"}
params = {
    "$filter": f"subject eq 'Urgent: Password Reset Required' and "
               f"receivedDateTime ge 2024-03-14T00:00:00Z",
    "$select": "sender,toRecipients,subject,receivedDateTime",
    "$top": 100
}
response = requests.get(
    "https://graph.microsoft.com/v1.0/users/admin@empresa.com.br/messages",
    headers=headers, params=params
)
messages = response.json()["value"]
print(f"{len(messages)} mensagens encontradas")
```

### Passo 4: Identificar os usuários impactados (quem clicou)

Veja nos logs de proxy/web quem acessou a URL de phishing:

```spl
index=proxy dest="evil-login.example.com" earliest=-7d
| stats count, values(action) AS actions, latest(_time) AS last_access
  by src_ip, user
| lookup asset_lookup_by_cidr ip AS src_ip OUTPUT owner, category
| sort - count
| table user, src_ip, owner, actions, count, last_access
```

Verifique se houve envio de credenciais (requisições POST para o domínio de phishing):

```spl
index=proxy dest="evil-login.example.com" http_method=POST earliest=-7d
| stats count by src_ip, user, url, status
```

### Passo 5: Ações de contenção

**Expurgar os e-mails de todas as caixas:**

```powershell
# Busca de conformidade e expurgo no Microsoft 365
New-ComplianceSearch -Name "Phishing_Purge_2024_0315" `
    -ExchangeLocation All `
    -ContentMatchQuery '(From:attacker@evil-domain.com) AND (Subject:"Urgent: Password Reset Required")'

Start-ComplianceSearch -Identity "Phishing_Purge_2024_0315"

# Depois que a busca terminar, execute o expurgo
New-ComplianceSearchAction -SearchName "Phishing_Purge_2024_0315" -Purge -PurgeType SoftDelete
```

**Bloquear os indicadores:**
- Adicione o domínio remetente à lista de bloqueio do gateway de e-mail
- Adicione o domínio da URL de phishing à lista de bloqueio do proxy web
- Adicione o hash do anexo à lista de bloqueio do EDR
- Crie uma entrada de DNS sinkhole para o domínio de phishing

**Redefinir as credenciais comprometidas:**

```powershell
# Força troca de senha e revoga sessões dos usuários impactados (Microsoft Graph PowerShell)
Connect-MgGraph -Scopes "User.ReadWrite.All"
$impactedUsers = @("usuario1@empresa.com.br", "usuario2@empresa.com.br")
foreach ($user in $impactedUsers) {
    Update-MgUser -UserId $user -PasswordProfile @{ ForceChangePasswordNextSignIn = $true }
    Revoke-MgUserSignInSession -UserId $user
}
```

Se houve envio de credenciais, troque a senha de fato (não só force a troca no próximo login) e confirme que o MFA está ativo para a conta.

### Passo 6: Documentar e reportar

Monte o relatório do incidente com linha do tempo completa, IOCs, usuários impactados e ações de remediação.

```spl
| makeresults
| eval incident_id="PHI-2024-0315",
       reported_time="2024-03-15 09:12:00",
       sender="attacker@evil-domain[.]com",
       subject="Urgent: Password Reset Required",
       url="hxxps://evil-login[.]example[.]com/office365",
       recipients_count=47,
       clicked_count=5,
       credentials_submitted=2,
       emails_purged=47,
       passwords_reset=2,
       domains_blocked=1,
       disposition="Verdadeiro positivo - campanha de phishing de credenciais"
| table incident_id, reported_time, sender, subject, url, recipients_count,
        clicked_count, credentials_submitted, emails_purged, passwords_reset, disposition
```

## Conceitos-chave

| Termo | Definição |
|-------|-----------|
| **SPF (Sender Policy Framework)** | Registro DNS TXT que define quais servidores podem enviar e-mails em nome de um domínio |
| **DKIM** | DomainKeys Identified Mail: assinatura criptográfica que prova que o conteúdo do e-mail não foi alterado no caminho |
| **DMARC** | Domain-based Message Authentication, Reporting and Conformance: política que combina o alinhamento de SPF e DKIM |
| **Captura de credenciais** | Técnica de phishing com páginas de login falsas para roubar usuário e senha |
| **BEC (Business Email Compromise)** | Golpe de engenharia social com e-mail de executivo comprometido ou falsificado, geralmente para fraude financeira |
| **Message Trace** | Log do O365/Exchange com rota, status de entrega e filtros aplicados a cada e-mail, usado na análise forense |

## Ferramentas

- **Microsoft Defender for Office 365**: segurança de e-mail com Safe Links, Safe Attachments e Threat Explorer para investigação
- **URLScan.io**: análise gratuita de URLs com captura de tela, DOM, cookies e requisições de rede
- **Any.Run**: sandbox interativa para detonar arquivos e URLs maliciosos e ver o comportamento em tempo real
- **Proofpoint TAP**: painel que mostra URLs clicadas e ameaças entregues por usuário
- **PhishTool**: plataforma dedicada à análise de phishing, automatiza a leitura de cabeçalhos e a extração de IOCs

## Cenários comuns

- **Phishing de credenciais**: página falsa de login do O365. Procure POSTs no proxy e force a troca de senha de quem enviou dados
- **Documento com macro**: Word com macro VBA. A sandbox mostra download via PowerShell; verifique execução nos endpoints
- **Phishing por QR Code (quishing)**: e-mail com QR Code que leva a uma página de captura. Decodifique o QR e envie a URL para a sandbox
- **Sequestro de conversa (thread hijacking)**: o atacante responde em conversas existentes a partir de uma caixa comprometida. Procure "viagem impossível" e regras de caixa de entrada novas
- **Phishing de correio de voz**: falsa notificação de voicemail com anexo HTML. Analise o anexo em busca de cadeias de redirecionamento

## Formato de saída

```
RELATÓRIO DE INCIDENTE DE PHISHING — PHI-2024-0315
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Reportado:    2024-03-15 09:12 UTC por jsilva (Financeiro)
Remetente:    attacker@evil-domain[.]com (SPF: FAIL, DKIM: NONE, DMARC: FAIL)
Assunto:      Urgent: Password Reset Required
Carga:        URL de captura de credenciais

IOCs:
  URL:        hxxps://evil-login[.]example[.]com/office365
  Domínio:    evil-login[.]example[.]com (registrado em 2024-03-14, Namecheap)
  IP:         185.234.xx.xx (VT: 12/90 malicioso)

Escopo:
  Destinatários: 47 usuários do Financeiro e do RH
  Clicaram:      5 usuários acessaram a URL de phishing
  Enviaram:      2 usuários digitaram credenciais (POST confirmado no proxy)

Contenção:
  [FEITO] 47 e-mails expurgados via Compliance Search
  [FEITO] Domínio bloqueado no proxy e no DNS sinkhole
  [FEITO] Senha de 2 usuários redefinida e sessões revogadas
  [FEITO] MFA exigido para as 2 contas comprometidas
  [FEITO] Regras de caixa de entrada auditadas, sem regras de encaminhamento

Status:       RESOLVIDO — sem evidência de movimentação lateral após o comprometimento
```

---

## Créditos e licença

Skill adaptada e traduzida para o português a partir da skill `investigating-phishing-email-incident` (versão 1.0), de autoria de **mahipal**, publicada em [mukul975/Anthropic-Cybersecurity-Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills) sob a licença **Apache 2.0**.

Alterações desta versão: tradução para o português do Brasil, remoção dos metadados de mapeamento (MITRE ATT&CK, MITRE F3, NIST CSF), troca dos comandos dos módulos MSOnline/AzureAD (descontinuados) pelos equivalentes do Microsoft Graph PowerShell, e não inclusão das pastas `scripts` e `references` do original.
