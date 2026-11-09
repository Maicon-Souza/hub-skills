---
name: playbook-de-resposta-a-incidentes
description: Cria e documenta playbooks de resposta a incidentes por tipo de incidente, com procedimentos passo a passo, árvores de decisão, critérios de escalonamento, matriz RACI e integração com SOAR, alinhados ao NIST SP 800-61r3 e ao SANS PICERL. Use ao montar ou amadurecer um programa de resposta a incidentes.
---

# Playbooks de resposta a incidentes

Projete e documente playbooks estruturados de resposta a incidentes: procedimentos por tipo de incidente, árvores de decisão, critérios de escalonamento, matriz RACI e integração com plataformas SOAR, alinhados ao NIST SP 800-61r3 e ao SANS PICERL.

## Quando usar

- Ao criar ou amadurecer um programa de resposta a incidentes do zero
- Ao documentar procedimentos para um novo tipo de incidente depois de um ataque inédito
- Ao automatizar fluxos de resposta em uma plataforma SOAR (Cortex XSOAR, Splunk SOAR)
- Ao se preparar para auditorias que exigem procedimentos de resposta documentados (SOC 2, PCI-DSS, ISO 27001, LGPD)
- Ao fazer uma análise de lacunas da capacidade de resposta frente a cenários de ameaça específicos

**Não use** para investigações pontuais. Playbooks são documentos de procedimento reutilizáveis, não relatórios de um caso específico.

## Pré-requisitos

- Avaliação de riscos da organização apontando os principais cenários de incidente por probabilidade e impacto
- NIST SP 800-61r3 ou SANS PICERL adotado como padrão de resposta a incidentes
- Inventário de ativos com criticidade para o negócio e classificação da informação
- Matriz RACI definindo os papéis: Comandante do Incidente, analistas do SOC, administradores de sistemas, jurídico, comunicação
- Inventário das capacidades de detecção existentes (regras de SIEM, detecções de EDR, assinaturas de IDS)
- Acesso à plataforma SOAR, se for construir playbooks automatizados

## Passo a passo

### Passo 1: Escolher e delimitar o tipo de incidente

Defina o cenário específico que o playbook vai cobrir:

- Identifique os principais tipos de incidente com base na avaliação de riscos e no histórico
- Limite cada playbook a um único tipo de incidente (não misture cenários sem relação)
- Defina as condições de acionamento do playbook

Tipos comuns de playbook:
```
Playbooks prioritários (construa primeiro):
1. Ransomware
2. Phishing / comprometimento de credenciais
3. Comprometimento de e-mail corporativo (BEC)
4. Infecção por malware
5. Vazamento / exfiltração de dados
6. Ataque DDoS
7. Ameaça interna
8. Tomada de conta (account takeover)
9. Comprometimento de aplicação web
10. Comprometimento de infraestrutura em nuvem
```

### Passo 2: Definir a estrutura do playbook

Todo playbook deve seguir a mesma estrutura:

```
MODELO DE PLAYBOOK
━━━━━━━━━━━━━━━━━━
1. Metadados do playbook
   - Nome, versão, responsável, data da última revisão
   - Condições de acionamento
   - Critérios de severidade

2. Matriz RACI
   - Quem é Responsável, Aprovador, Consultado e Informado em cada etapa

3. Detecção e triagem
   - Como o incidente é detectado
   - Checklist de triagem inicial
   - Critérios de classificação de severidade

4. Contenção
   - Ações de contenção de curto prazo
   - Ações de contenção de longo prazo
   - Requisitos de preservação de evidências

5. Erradicação
   - Identificação da causa raiz
   - Passos para remover a ameaça / malware
   - Procedimentos de verificação

6. Recuperação
   - Passos de restauração dos sistemas
   - Critérios de validação
   - Monitoramento após a recuperação

7. Pós-incidente
   - Gatilho da reunião de lições aprendidas
   - Modelo de relatório
   - Ações de melhoria da detecção

8. Comunicação
   - Matriz de notificação interna
   - Notificações externas obrigatórias (reguladores, clientes, autoridades)
   - Frequência das atualizações de status

9. Anexos
   - Procedimentos específicos por ferramenta
   - Listas de contatos
   - Checklists de coleta de evidências
```

### Passo 3: Escrever árvores de decisão e critérios de escalonamento

Defina pontos de decisão claros, com respostas binárias:

```
Alerta de detecção recebido
├── O alerta é um verdadeiro positivo?
│   ├── SIM → Classificar a severidade
│   │   ├── P1 (Crítico) → Acionar o comandante do incidente, iniciar contenção imediatamente
│   │   ├── P2 (Alto) → Notificar o líder de resposta, iniciar investigação em até 30 min
│   │   ├── P3 (Médio) → Fila de investigação em até 4 horas
│   │   └── P4 (Baixo) → Documentar e investigar em até 24 horas
│   └── NÃO → Documentar como falso positivo, ajustar a regra de detecção
└── Não é possível determinar → Escalar para o Nível 2 para análise mais profunda
```

Gatilhos de escalonamento:
- Qualquer incidente P1: escalar imediatamente para o líder de resposta e o CISO
- Exfiltração de dados confirmada: notificar o jurídico e o encarregado de dados (DPO)
- Dados de clientes envolvidos: acionar o processo de notificação a clientes (e avaliar comunicação à ANPD, conforme a LGPD)
- Envolvimento de terceiros: acionar o contato de segurança do fornecedor
- Necessidade de envolver autoridades policiais: o jurídico autoriza antes do contato

### Passo 4: Definir procedimentos técnicos específicos

Escreva instruções específicas por ferramenta em cada etapa (nada de orientação genérica):

```
CONTENÇÃO - Isolamento de endpoint no CrowdStrike:
1. Abra o Falcon Console > Hosts > busque o hostname afetado
2. Clique no host > Host Details
3. Clique em "Contain Host" no canto superior direito
4. Confirme o isolamento (o host só falará com a nuvem da CrowdStrike)
5. Registre a ação de contenção no chamado do incidente, com horário
6. Verifique: o host deve exibir o selo "Contained"

CONTENÇÃO - Bloqueio de domínio de C2 no DNS:
1. Acesse o servidor DNS: ssh admin@dns-primary.corp.local
2. Adicione à zona de bloqueio: echo "zone evil.com { type master; file /etc/bind/db.sinkhole; };" >> /etc/bind/named.conf.local
3. Recarregue o DNS: rndc reload
4. Verifique: dig @dns-primary evil.com (deve resolver para o IP do sinkhole, 10.0.0.99)
5. Registre o domínio bloqueado no chamado do incidente
```

### Passo 5: Integrar com a plataforma SOAR

Converta os passos manuais em fluxos automatizados:

- Mapeie cada passo do playbook para uma ação no SOAR (chamada de API, script, decisão humana)
- Defina os limites da automação (o que roda sozinho e o que exige aprovação do analista)
- Construa automações de enriquecimento para a fase de triagem
- Crie automações de contenção com etapas de aprovação para ações de alto impacto
- Configure automações de notificação para comunicar as partes interessadas

### Passo 6: Testar e manter o playbook

Valide o playbook com exercícios e mantenha-o atualizado:

- Faça exercícios de mesa (tabletop) com o time de resposta percorrendo o playbook
- Faça simulações práticas do tipo de incidente em ambiente de testes
- Revise e atualize depois de cada incidente real que usou o playbook
- Agende revisões trimestrais de listas de contato, procedimentos de ferramentas e caminhos de escalonamento
- Acompanhe métricas: tempo médio para conter, tempo médio para resolver, taxa de falsos positivos

## Conceitos-chave

| Termo | Definição |
|-------|-----------|
| **Playbook** | Conjunto documentado e repetível de procedimentos para responder a um tipo específico de incidente |
| **Runbook** | Mais granular que o playbook: instruções técnicas passo a passo para uma tarefa específica dentro de um playbook |
| **Matriz RACI** | Quadro de responsabilidades que define quem é Responsável, Aprovador (Accountable), Consultado e Informado em cada atividade |
| **Árvore de decisão** | Fluxograma que define o caminho da resposta a partir de condições binárias em cada ponto de decisão |
| **Critérios de escalonamento** | Condições predefinidas que disparam a notificação de níveis superiores ou de partes externas |
| **Playbook de SOAR** | Fluxo automatizado em uma plataforma de Orquestração, Automação e Resposta de Segurança que executa os passos do playbook |

## Ferramentas

- **Cortex XSOAR**: plataforma SOAR com editor visual de playbooks, mais de 700 integrações e War Room colaborativa
- **Splunk SOAR**: plataforma SOAR integrada ao Splunk ES, com editor de arrastar e soltar e mais de 2.800 ações automatizadas
- **TheHive**: plataforma open source de resposta a incidentes, com modelos de caso que funcionam como base de playbooks
- **Confluence / GitLab Wiki**: documentação legível para pessoas, com controle de versão
- **Tines**: automação de segurança sem código para construir fluxos de playbook

## Cenário comum

### Cenário: criar um playbook de resposta a phishing do zero

**Contexto**: uma organização com SOC de 5 pessoas não tem procedimento documentado para phishing. Cada analista trata os relatos de um jeito.

**Abordagem**:
1. Entreviste os analistas para documentar o processo informal atual
2. Defina o gatilho: usuário reporta phishing pela caixa abuse@ ou pelo botão de reportar
3. Escreva os passos de triagem: extrair cabeçalhos, checar reputação do remetente, analisar URLs e anexos em sandbox
4. Defina a contenção: colocar o e-mail em quarentena em todas as caixas, bloquear o domínio remetente, redefinir senhas se houve envio de credenciais
5. Construa a automação no SOAR: extrair IOCs do e-mail reportado, enriquecer via VirusTotal, abrir caso no TheHive
6. Teste com um e-mail de phishing simulado e meça a melhora no tempo de resposta

**Armadilhas**:
- Escrever procedimentos genéricos demais, sem citar telas ou comandos específicos das ferramentas
- Esquecer o plano de comunicação para avisar os usuários que receberam o phishing
- Não definir quando um relato de phishing vira uma investigação completa de incidente
- Não versionar o playbook nem agendar ciclos de revisão

## Formato de saída

```
PLAYBOOK DE RESPOSTA A INCIDENTES
==================================
Nome:              Resposta a incidente de phishing
Versão:            2.1
Responsável:       Gerente do SOC
Última revisão:    2025-11-01
Próxima revisão:   2026-02-01
Gatilho:           Phishing reportado via abuse@empresa.com.br ou botão de reportar

MATRIZ RACI
Atividade                   | SOC N1 | SOC N2 | Líder IR | Jurídico | Comunicação
Triagem inicial             |   R    |   C    |    I     |          |
Análise do e-mail           |   R    |   A    |    I     |          |
Contenção                   |        |   R    |    A     |    I     |
Redefinição de credenciais  |        |   R    |    A     |          |
Aviso aos usuários          |        |   C    |    A     |          |     R
Notificação regulatória     |        |        |    C     |    R     |     A
Lições aprendidas           |   C    |   C    |    R     |    I     |     I

PASSOS DO PROCEDIMENTO
[Passos detalhados com instruções por ferramenta]

ÁRVORE DE DECISÃO
[Lógica do fluxograma]

MATRIZ DE ESCALONAMENTO
[Condições e contatos]

MÉTRICAS
Meta de MTTA (tempo até reconhecer): 15 minutos
Meta de MTTC (tempo até conter):     1 hora
Meta de MTTR (tempo até resolver):   4 horas
```

---

## Créditos e licença

Skill adaptada e traduzida para o português a partir da skill `building-incident-response-playbook` (versão 1.0.0), de autoria de **mahipal**, publicada em [mukul975/Anthropic-Cybersecurity-Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills) sob a licença **Apache 2.0**.

Alterações desta versão: tradução para o português do Brasil, remoção dos metadados de mapeamento (MITRE ATT&CK, NIST CSF), inclusão de referências à LGPD, à ANPD e à ISO 27001 nos critérios de escalonamento e auditoria, e não inclusão das pastas `scripts` e `references` do original.
