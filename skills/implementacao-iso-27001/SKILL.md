---
name: implementacao-iso-27001
description: Guia a implementação de um Sistema de Gestão de Segurança da Informação (SGSI) pela ISO/IEC 27001:2022, do escopo e análise de lacunas à avaliação de riscos, seleção de controles do Anexo A, Declaração de Aplicabilidade e melhoria contínua. Use ao definir um SGSI, se preparar para certificação ou auditoria ISO 27001, ou documentar controles do Anexo A.
---

# Implementação da ISO 27001 (gestão de segurança da informação)

## Visão geral

A ISO/IEC 27001:2022 é a norma internacional para estabelecer, implementar, manter e melhorar continuamente um Sistema de Gestão de Segurança da Informação (SGSI, ou ISMS em inglês). Esta skill cobre o ciclo completo, do escopo à certificação: seleção de controles do Anexo A, metodologia de avaliação de riscos, criação da Declaração de Aplicabilidade (SoA) e processos de melhoria contínua.

## Quando usar

- Ao definir o escopo de um novo SGSI
- Ao se preparar para a certificação ou para uma auditoria ISO 27001
- Ao selecionar e documentar controles do Anexo A em um programa de conformidade
- Ao fazer uma avaliação de segurança que precisa estar alinhada à ISO 27001

## Pré-requisitos

- Entendimento de princípios de segurança da informação e de gestão de riscos
- Conhecimento da estrutura de governança e dos processos de negócio da organização
- Conhecimento da infraestrutura de TI, da arquitetura de rede e dos fluxos de dados
- Acesso às normas ISO/IEC 27001:2022 e ISO/IEC 27002:2022

## Conceitos centrais

### Cláusulas do SGSI (4 a 10)

Os requisitos do sistema de gestão definem **o que** precisa ser feito:
- **Cláusula 4 - Contexto da organização**: escopo, partes interessadas e questões internas e externas
- **Cláusula 5 - Liderança**: comprometimento da alta direção, política de segurança da informação, papéis e responsabilidades
- **Cláusula 6 - Planejamento**: processo de avaliação de riscos, plano de tratamento de riscos, objetivos de segurança da informação
- **Cláusula 7 - Apoio**: recursos, competência, conscientização, comunicação, informação documentada
- **Cláusula 8 - Operação**: planejamento operacional, execução da avaliação de riscos, implementação do tratamento de riscos
- **Cláusula 9 - Avaliação de desempenho**: monitoramento, medição, auditoria interna, análise crítica pela direção
- **Cláusula 10 - Melhoria**: não conformidades, ações corretivas, melhoria contínua

### Controles do Anexo A (edição 2022)

A revisão de 2022 reorganizou os controles em 93 itens, divididos em quatro temas:

| Tema | Controles | Exemplos |
|------|-----------|----------|
| Organizacionais (A.5) | 37 controles | Políticas, papéis, inteligência de ameaças, segurança em nuvem |
| Pessoas (A.6) | 8 controles | Verificação de antecedentes, conscientização, trabalho remoto, notificação de eventos |
| Físicos (A.7) | 14 controles | Perímetros, controle de entrada, segurança de equipamentos |
| Tecnológicos (A.8) | 34 controles | Controle de acesso, criptografia, registro de logs, desenvolvimento seguro |

### Controles novos na edição 2022

Foram adicionados 11 controles:
1. A.5.7 - Inteligência de ameaças
2. A.5.23 - Segurança da informação no uso de serviços em nuvem
3. A.5.30 - Prontidão de TIC para a continuidade de negócios
4. A.7.4 - Monitoramento da segurança física
5. A.8.9 - Gestão de configuração
6. A.8.10 - Exclusão de informações
7. A.8.11 - Mascaramento de dados
8. A.8.12 - Prevenção de vazamento de dados
9. A.8.16 - Atividades de monitoramento
10. A.8.23 - Filtragem da web
11. A.8.28 - Codificação segura

## Passo a passo

### Fase 1: Análise de lacunas e escopo (semanas 1 a 4)
1. Defina os limites do escopo do SGSI (locais, unidades de negócio, sistemas)
2. Identifique as partes interessadas e seus requisitos (inclusive legais, como a LGPD)
3. Faça a análise de lacunas frente aos requisitos da ISO 27001:2022
4. Documente o contexto interno e externo (PESTEL, SWOT)
5. Obtenha o comprometimento da alta direção e o orçamento

### Fase 2: Avaliação de riscos (semanas 5 a 10)
1. Defina a metodologia de avaliação de riscos (baseada em ativos, em cenários ou híbrida)
2. Monte o inventário de ativos: informação, pessoas, processos e tecnologia
3. Identifique ameaças e vulnerabilidades de cada ativo
4. Avalie probabilidade e impacto com critérios definidos
5. Calcule o nível de risco e defina o tratamento (mitigar, aceitar, transferir, evitar)
6. Elabore o Plano de Tratamento de Riscos (PTR)

### Fase 3: Seleção de controles e Declaração de Aplicabilidade (semanas 11 a 14)
1. Mapeie os tratamentos de risco para os controles do Anexo A
2. Crie a Declaração de Aplicabilidade (SoA), documentando:
   - Quais controles se aplicam e a justificativa
   - Quais controles foram excluídos e a justificativa
   - O status de implementação de cada controle
3. Monte os planos de implementação dos controles, com responsáveis e prazos

### Fase 4: Implementação (semanas 15 a 30)
1. Elabore e aprove a política de segurança da informação
2. Implemente os controles do Anexo A selecionados
3. Crie a informação documentada obrigatória:
   - Política de segurança da informação (A.5.1)
   - Processo de avaliação de riscos (cláusula 6.1.2)
   - Processo de tratamento de riscos (cláusula 6.1.3)
   - Programa de auditoria interna (cláusula 9.2)
   - Processo de análise crítica pela direção (cláusula 9.3)
   - Procedimento de ação corretiva (cláusula 10.1)
4. Implante os controles técnicos e as ferramentas de segurança
5. Faça o treinamento de conscientização em segurança para todas as pessoas

### Fase 5: Auditoria interna e análise crítica pela direção (semanas 31 a 36)
1. Planeje e execute o programa de auditoria interna cobrindo todas as cláusulas e os controles aplicáveis
2. Documente os achados da auditoria e as não conformidades
3. Implemente as ações corretivas com análise de causa raiz
4. Faça a análise crítica pela direção, cobrindo:
   - Situação das ações anteriores
   - Mudanças nas questões internas e externas
   - Métricas de desempenho da segurança da informação
   - Resultados das auditorias e da avaliação de riscos
   - Oportunidades de melhoria

### Fase 6: Auditoria de certificação (semanas 37 a 42)
1. **Auditoria de Estágio 1**: revisão da documentação e avaliação de prontidão
2. Trate os achados do Estágio 1
3. **Auditoria de Estágio 2**: avaliação no local da eficácia do SGSI
4. Resolva as não conformidades (não conformidades maiores exigem nova auditoria)
5. Receba a certificação ISO 27001 (válida por 3 anos)

### Fase 7: Melhoria contínua (permanente)
1. Auditorias de manutenção anuais (anos 1 e 2)
2. Auditoria de recertificação (ano 3)
3. Reavaliação periódica de riscos e análise da eficácia dos controles
4. Incorporação de melhorias vindas de incidentes e lições aprendidas

## Documentos principais

- Documento de escopo do SGSI
- Política de segurança da informação
- Metodologia de avaliação de riscos
- Registro de riscos e Plano de Tratamento de Riscos
- Declaração de Aplicabilidade (SoA)
- Relatórios de auditoria interna
- Atas de análise crítica pela direção
- Registro de ações corretivas
- Painel de métricas e indicadores

## Armadilhas comuns

- Escopo amplo ou restrito demais, o que complica a auditoria
- Tratar a ISO 27001 como checklist em vez de incorporá-la aos processos de negócio
- Pouco envolvimento e comprometimento da alta direção
- Não manter evidências documentadas de que os controles estão operando
- Não reavaliar os riscos com regularidade conforme o cenário de ameaças muda
- Ignorar os 11 controles novos da edição 2022 na transição

## Pontos de integração

- **ISO 27002:2022**: orientação detalhada para implementar os controles do Anexo A
- **ISO 27005**: metodologia de gestão de riscos de segurança da informação
- **ISO 27017**: controles de segurança para nuvem
- **ISO 27018**: proteção de dados pessoais em serviços de nuvem
- **ISO 27701**: extensão para Sistema de Gestão da Privacidade da Informação (SGPI), útil para atender a LGPD
- **NIST CSF 2.0**: mapeamento cruzado para conformidade dupla
- **SOC 2**: critérios de serviços de confiança que se sobrepõem

## Referências

- ISO/IEC 27001:2022 Sistemas de gestão de segurança da informação (no Brasil, ABNT NBR ISO/IEC 27001:2022)
- ISO/IEC 27002:2022 Controles de segurança da informação
- ISO/IEC 27005:2022 Gestão de riscos de segurança da informação
- Guia do Anexo A da ISMS.online: https://www.isms.online/iso-27001/annex-a-2022/
- Guia de controles ISO 27001 da IT Governance: https://www.itgovernance.co.uk/blog/iso-27001-the-14-control-sets-of-annex-a-explained

---

## Créditos e licença

Skill adaptada e traduzida para o português a partir da skill `implementing-iso-27001-information-security-management` (versão 1.0), de autoria de **mahipal**, publicada em [mukul975/Anthropic-Cybersecurity-Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills) sob a licença **Apache 2.0**.

Alterações desta versão: tradução para o português do Brasil com a terminologia usada na versão ABNT da norma, remoção dos metadados de mapeamento (MITRE ATT&CK, NIST CSF), reescrita da seção "Quando usar", inclusão de referências à LGPD, e não inclusão das pastas `scripts`, `references` e `assets` do original.
