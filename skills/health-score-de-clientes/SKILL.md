---
name: health-score-de-clientes
description: Cria, calcula e acompanha o health score (índice de saúde) de clientes de consultorias, agências e serviços B2B, com pesos, faixas de risco, alertas, planos de recuperação e relatórios. Use ao montar um health score, avaliar o risco de um cliente, preparar uma reunião com cliente em risco ou buscar oportunidades de renovação e upsell.
---

# Health score de clientes

Você ajuda consultorias, agências e empresas de serviços B2B a montar e usar um health score: uma nota de 0 a 100 que resume a saúde de cada cliente, antecipa riscos de cancelamento e aponta oportunidades de renovação e expansão. Responda em português do Brasil.

Palavras-chave: health score, saúde do cliente, retenção, churn preditivo, sucesso do cliente, engajamento, valor percebido, onboarding, relatório de desempenho, consultoria B2B, upsell, satisfação, plano de ação.

## Antes de começar

Pergunte o que não foi informado, de uma vez só:

- **Tipo de serviço:** consultoria, agência, serviço recorrente, projeto fechado? Há uma plataforma que o cliente usa?
- **Objetivo do score:** reduzir cancelamento, aumentar renovação, achar oportunidades de upsell?
- **Dados disponíveis:** reuniões, e-mails, KPIs do projeto, NPS/CSAT, uso de plataforma, contratos. Onde ficam (CRM, planilha, BI)?
- **Carteira:** quantos clientes, quantos por consultor, se há clientes de portes muito diferentes.

Se o usuário mandar dados reais, calcule com eles. Nunca invente notas, resultados ou histórico de clientes.

## Primeiros passos rápidos

1. **Mapeie os pontos de contato:** onboarding, reuniões de revisão, workshops, relatórios, suporte.
2. **Escolha os indicadores:** de 5 a 10 métricas concretas (presença em reuniões, tempo de resposta, metas atingidas, uso da plataforma, satisfação).
3. **Dê pesos que somem 100%** e uma nota de 0 a 10 para cada indicador.
4. **Calcule para 5 a 10 clientes** com os dados dos últimos 30 dias, para testar o modelo.
5. **Classifique por faixa:** Crítico (0 a 40), Atenção (41 a 70), Saudável (71 a 85), Promotor (86 a 100).

---

## Fluxo 1: montar e manter o health score

1. **Defina o propósito.** Exemplo: "Reduzir em 15% o cancelamento dos clientes de marketing digital em 12 meses, identificando cedo os insatisfeitos e as oportunidades de expansão."

2. **Escolha e pondere os indicadores.** Exemplo:

   | Indicador | Peso | Como medir |
   |---|---|---|
   | Uso da solução ou plataforma (se houver) | 20% | Frequência de acesso, funcionalidades usadas |
   | Engajamento com a consultoria | 25% | Reuniões realizadas ÷ agendadas, tempo de resposta a e-mails |
   | Metas do projeto atingidas | 35% | % dos KPIs combinados atingidos (tráfego, leads, ROI) |
   | Satisfação | 10% | Último NPS ou CSAT |
   | Sinais de renovação ou expansão | 10% | Conversas sobre renovação, pedidos de novos projetos |

   Se não houver plataforma, redistribua os 20% entre os outros indicadores.

3. **Calcule.** Cada indicador recebe uma nota de 0 a 10 com critério escrito (exemplo: metas: 10 = 100% ou mais atingido, 7 = 70%, 0 = nada). Depois:

   ```
   health score (0 a 100) = soma de (nota de 0 a 10 × peso em %) ÷ 10
   ```

   Exemplo: uso 8 × 20 = 160; engajamento 9 × 25 = 225; metas 7 × 35 = 245; satisfação 8 × 10 = 80; renovação 7 × 10 = 70. Soma 780 ÷ 10 = **78**.

4. **Integre os dados e automatize.** Puxe interações do CRM (HubSpot, Pipedrive, Salesforce), KPIs das planilhas do projeto e dados de uso, e monte um painel (Power BI, Looker Studio ou uma planilha bem feita). Configure alertas para o consultor responsável e para o gestor quando:
   - o cliente entrar na faixa Atenção ou Crítico; ou
   - o score cair 15 pontos ou mais em 30 dias, mesmo dentro de uma faixa boa.

5. **Ligue cada faixa a uma ação.**

   | Faixa | Ação |
   |---|---|
   | 0 a 40 (Crítico) | Reunião com o cliente em até 48h, plano de recuperação de 7 dias, envolver a diretoria |
   | 41 a 70 (Atenção) | Reunião de alinhamento, revisão de escopo, treinamento extra, relatório focado no valor entregue |
   | 71 a 85 (Saudável) | Acompanhamento normal, conversa sobre oportunidades de expansão |
   | 86 a 100 (Promotor) | Pedir depoimento, estudo de caso, indicação; reconhecer a parceria |

6. **Revise todo trimestre.** Compare o score com quem renovou e quem cancelou. Se clientes com 60 pontos estão cancelando tanto quanto os de 40, os pesos ou os indicadores estão errados: ajuste.

## Fluxo 2: intervir quando o score cai

1. **Detecte e investigue.** Exemplo: o painel mostra o "Cliente Indústria X" caindo de 75 para 55 em uma semana. Veja quais indicadores puxaram a queda (aqui, metas atingidas e engajamento).
2. **Converse antes de concluir.** Ligue para o contato principal e pergunte sobre desafios recentes, mudanças na equipe, percepção de valor e expectativas não atendidas. O número mostra que algo mudou; só o cliente diz o porquê.
3. **Monte um plano de recuperação** com objetivo, ações, datas e responsáveis. Exemplo:
   - Objetivo: subir a nota de "metas atingidas" de 4 para 7 em 30 dias.
   - Ação 1: reunião estratégica de otimização das campanhas (18/09), consultor sênior.
   - Ação 2: treinamento de 2 horas da equipe do cliente na ferramenta X (25/09), especialista de produto.
   - Ação 3: relatório semanal focado nos KPIs críticos (a partir de 22/09), consultor sênior.
4. **Execute e comunique.** A cada contato, mostre o que avançou e pergunte como o cliente está percebendo.
5. **Acompanhe e escale.** Exemplo: em 15 dias, metas subiu para 6 e engajamento para 8; pergunte ao cliente se o valor ficou mais claro. Se o score não melhorar no prazo do plano, leve à gestão para uma revisão completa da conta.

---

## Modelos

### Relatório mensal de health score

```markdown
# Relatório mensal de health score – TechSolutions S.A.

Período: agosto de 2026
Consultora responsável: Ana Paula Martins
Emissão: 2 de setembro de 2026

## Health score: 78/100 (Saudável)
Tendência: estável (mês anterior: 77)

## Indicadores

| Indicador | Nota (0-10) | Peso | Pontos | Observações |
|---|:-:|:-:|:-:|---|
| Uso da plataforma de automação | 8 | 20% | 16 | Acessos subiram 15%; começaram a usar a segmentação |
| Engajamento com a consultoria | 9 | 25% | 22,5 | Todas as reuniões realizadas; retornos rápidos |
| Metas do projeto | 7 | 35% | 24,5 | Leads qualificados: 92% da meta; conversão: 88%. Uma campanha atrasou |
| Satisfação | 8 | 10% | 8 | Último CSAT: 4,2/5 |
| Renovação e expansão | 7 | 10% | 7 | Primeiras conversas sobre automação de vendas |
| **Total** | | **100%** | **78** | |

## Destaques
- Equipe do cliente participa ativamente e aplica as recomendações.
- Uso maior da plataforma indica que o valor está sendo percebido.

## Pontos de atenção
- A conversão ficou um pouco abaixo da meta por atraso na aprovação das peças criativas.

## Próximos passos
1. Reunião de otimização das campanhas e do fluxo de aprovação (10/09).
2. Proposta de expansão para automação de vendas (15/09).
3. Acompanhamento diário das campanhas para fechar as metas no próximo mês.
```

### Roteiro de reunião com cliente em risco

```markdown
# Reunião de alinhamento – VarejoConecta Ltda.

Contexto: health score caiu de 65 para 48 em duas semanas, puxado por menor engajamento e resultados abaixo do esperado nas campanhas de e-mail.
Participantes: João Silva (consultor sênior), Camila Costa (sucesso do cliente) | Roberto Almeida (gerente de marketing), Patrícia Nunes (analista de e-mail marketing)
Duração: 45 minutos

1. Abertura (5 min)
   João: "Obrigado pelo tempo, Roberto e Patrícia. A ideia hoje é entender como vocês estão vendo o projeto e o que podemos ajustar para melhorar os resultados da VarejoConecta."

2. Situação atual (10 min)
   Camila: "Nas últimas semanas, as taxas de abertura e clique dos e-mails ficaram abaixo da média de vocês, e algumas reuniões de acompanhamento foram remarcadas. Queríamos ouvir a leitura de vocês sobre isso."
   [Pausa. Ouvir.]

3. Desafios e feedback (15 min)
   João: "Tem algo afetando a equipe ou as campanhas? Alguma expectativa que não estamos atendendo?"
   [Escuta ativa, perguntas abertas, anotar.]
   Respostas comuns: aprovação interna lenta, equipe sobrecarregada, retorno abaixo do esperado.

4. Plano de ação (10 min)
   Camila: "Com base no que vocês trouxeram, propomos:"
   - Workshop de 2h de otimização de e-mail (22/10): segmentação, personalização e texto, com meta de subir abertura e clique em 15%.
   - Reunião semanal de 30 min, às quartas, 14h, para acompanhar e destravar aprovações.
   - Relatório quinzenal ligando nossas ações aos resultados de vendas.
   João: "Faz sentido para vocês? Querem ajustar ou incluir algo?"
   [Obter acordo.]

5. Fechamento (5 min)
   Camila: "Resumindo: workshop dia 22/10, reuniões às quartas e relatório quinzenal. Nossa meta é voltar à faixa saudável em 30 dias."
   João: "Mandamos o convite do workshop e o primeiro relatório ainda hoje."
```

---

## Checklist

- [ ] Listar todos os pontos de contato do cliente com a consultoria.
- [ ] Definir de 5 a 10 indicadores, com critério escrito para cada nota.
- [ ] Dar pesos que somem 100%.
- [ ] Definir as faixas e os gatilhos de alerta (faixa e queda brusca).
- [ ] Automatizar a coleta de dados via CRM, BI ou planilha.
- [ ] Criar um painel visível para a equipe e a gestão.
- [ ] Escrever um playbook de ação para cada faixa.
- [ ] Treinar os consultores para ler o score e executar os planos.
- [ ] Revisar o modelo todo trimestre contra renovações e cancelamentos reais.

## Métricas de referência

Valores indicativos da skill original, sem fonte citada. Use como ponto de partida e calibre com o seu histórico.

| Métrica | Referência (consultoria B2B) | Meta ambiciosa |
|---|---|---|
| Taxa de renovação de contrato | 80 a 85% | acima de 95% |
| NPS | 40 a 60 | acima de 70 |
| LTV ÷ CAC | 3 a 5 vezes | acima de 6 vezes |
| Taxa de upsell/cross-sell | 15 a 25% | acima de 35% |
| Tempo até o primeiro valor percebido | 60 a 90 dias | menos de 30 dias |

O original também citava churn anual de 5 a 8% como referência, o que não fecha com renovação de 80 a 85% (que implica perda de 15 a 20% ao ano). Calcule as duas taxas com seus próprios dados.

## Erros comuns

1. **Olhar só números.** O cliente pode usar muito a plataforma e estar insatisfeito com o atendimento.
   Como evitar: pesquisas curtas e frequentes, conversas sem pauta comercial e registro no CRM de todo feedback qualitativo, com peso no score. Exemplo: engajamento alto mas NPS 6 pede investigação, não comemoração.
2. **Modelo parado no tempo.** Serviço, cliente e mercado mudam.
   Como evitar: revisão trimestral ou semestral dos pesos e indicadores. Ao lançar um serviço novo, inclua um indicador de adoção dele.
3. **Score sem ação.** Saber que o cliente está em risco e não saber o que fazer leva à paralisia.
   Como evitar: playbook por faixa, com roteiros, modelos de e-mail, responsáveis e tarefas criadas automaticamente no CRM quando o cliente muda de faixa.

## Dicas avançadas

1. **Score por porte de cliente.** Grandes contas, médias e pequenas pedem pesos diferentes. Para grandes contas, "participação da diretoria em comitês" pode pesar mais; para pequenas, metas de curto prazo e uso dos materiais de apoio.
2. **Prever cancelamento pela queda.** Use o histórico para achar padrões de queda que antecederam cancelamentos. Exemplo: se 80% dos clientes que caíram 20 pontos em 30 dias cancelaram nos 60 dias seguintes, esse padrão vira alerta de alto risco. Com histórico suficiente, regressão logística ou outros modelos estimam a probabilidade.
3. **Cruzar com rentabilidade.** Compare o score com margem e horas consumidas. Clientes críticos que consomem 3 vezes mais horas pedem intervenção rápida ou uma revisão honesta da parceria.
4. **IA para sugerir a próxima ação.** Com o histórico do cliente e os indicadores em queda, o Claude pode sugerir as ações e os modelos mais adequados para o consultor revisar e aplicar.
5. **Reconhecimento interno.** Reconheça os consultores que mais recuperam clientes em risco. Cuidado com metas atreladas ao próprio score: elas incentivam a "maquiar" as notas. Prefira premiar renovações e recuperações reais.

## Formato da resposta

Entregue o material pronto (modelo de score com pesos e critérios, cálculo, plano de ação, relatório ou roteiro), com os dados que faltam marcados com `[ ]`, seguido do que o usuário precisa preencher ou validar.

---

## Créditos e licença

Skill adaptada a partir da skill `client-health-score` (versão 1.0.0), de autoria de **Cafe Code AI**, publicada em [kursku/skills](https://github.com/kursku/skills) (pasta `packs/kit-510-ptbr/10-clientes-consultoria/client-health-score`). A licença **MIT** está declarada nos metadados da skill original; o repositório não traz um arquivo de licença próprio.

Alterações desta versão: correção da fórmula do score (notas de 0 a 10 vezes pesos em % somavam até 10, não 100); faixas unificadas entre o início e o fluxo; alerta por queda brusca além da faixa; aviso sobre a inconsistência entre churn e renovação nas métricas de referência e de que elas não têm fonte; alerta sobre o risco de metas ligadas ao próprio score; inclusão das seções "Antes de começar" e "Formato da resposta"; checklist desmarcado para uso; textos dos modelos revisados.

Texto da licença MIT:

```
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
