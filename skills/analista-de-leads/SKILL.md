---
name: analista-de-leads
description: Analisa uma planilha de leads no Google Drive/Sheets e entrega ranking com nota 0-100, os 3 contatos prioritários do dia, mensagens de WhatsApp prontas e insights do funil (valor em risco, concentração, urgências). Use sempre que o usuário pedir para priorizar leads, qualificar oportunidades, saber "quem procurar hoje", analisar pipeline/funil de vendas ou mencionar planilha de leads/prospects, mesmo sem dizer "skill".
---

# Analista de Leads

Transforma uma planilha de leads em decisão de venda: quem priorizar, o que dizer e o que o funil está mostrando.

## Fluxo

1. **Localizar** a planilha no Google Drive (`search_files`, filtrando por título e tipo planilha). Se houver mais de uma com nome parecido, perguntar qual. Se não achar, avisar e pedir o nome exato.
2. **Ler** com `read_file_content`. Mapear colunas pelo cabeçalho (nome, produto, valor, dias desde o último contato, observação). Se faltar uma coluna-chave, pontuar só com o que existe e declarar a limitação.
3. **Pontuar** cada lead de 0 a 100 usando a rubrica da seção [Rubrica de pontuação](#rubrica-de-pontuação-total-100) abaixo.
4. **Escolher o Top 3 de hoje:** maiores notas; desempate pelo maior valor.
5. **Escrever uma mensagem de WhatsApp** para cada um do Top 3 (regras abaixo).
6. **Gerar insights do funil** (seção abaixo).

## Formato de entrega

Sempre em tabelas, nesta ordem:

1. **Ranking** — `# | Lead | Produto | Nota | Motivo (1 linha)`, ordenado da maior para a menor nota.
2. **Top 3 de hoje** — `Lead | Por que hoje | Mensagem de WhatsApp`.
3. **Insights** — 3 a 5 bullets curtos, cada um com número calculado a partir da planilha.

Fechar com uma pergunta: quer ajustar os pesos da nota?

## Rubrica de pontuação (total 100)

Heurística inicial, ajustável pelo usuário. Aplicar sempre da mesma forma para manter o ranking comparável.

| Critério | Peso | Como pontuar |
|---|---|---|
| Sinal de compra | 0–40 | Decisão aprovada ou proposta pedida: 36–40. Escopo/necessidade clara: 24–35. Comparando fornecedor, aguardando orçamento futuro ou sensível a preço: 10–23. Esfriou ou sem retorno: 0–9. |
| Valor | 0–25 | Proporcional ao maior valor da planilha: (valor ÷ maior valor) × 25, arredondado. |
| Recência do contato | 0–20 | Até 7 dias: 20. 8–14: 14. 15–21: 8. 22 ou mais: 2. |
| Urgência/timing | 0–15 | Urgência, prazo ou evento explícito na observação: 15. Janela futura definida: 8. Nada informado: 0. |

### Definição de "lead frio"

Mais de 14 dias sem contato.

### Cuidados

- O sinal de compra e a urgência vêm só do texto da observação. Se a observação estiver vazia, pontuar 0 nesses critérios e dizer "não informado".
- Se faltar a coluna de valor ou de dias, redistribuir o peso proporcionalmente entre os critérios restantes e avisar.

## Mensagens de WhatsApp

- Até 3 linhas, tom profissional e próximo, começando pelo primeiro nome.
- Usar apenas o que está na planilha (produto e observação). Sem promessa, prazo, desconto ou preço que não conste lá.
- Um único próximo passo (ex.: confirmar proposta, marcar conversa de 15 min).
- Lead frio (muitos dias sem contato): retomar sem cobrar, abrindo uma porta fácil de responder.

## Insights que valem trazer

- Valor total do funil e quanto dele está em leads frios (usar a definição de lead frio da rubrica).
- Concentração: % do funil no maior lead.
- Leads com urgência ou prazo explícito na observação.
- Leads para reaquecer (frios com valor relevante).
- Tema recorrente entre os produtos, se houver.

## Regras

- Nunca inventar dado, número ou fato. O que não estiver na planilha vira "não informado".
- Somente leitura: não editar a planilha.
- Nota e motivo devem ser rastreáveis à rubrica e aos dados lidos.
- Responder em português, direto.
