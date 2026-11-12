---
name: visualizacao-de-dados-ga4
description: Cria visualizações, dashboards e relatórios a partir do Google Analytics 4 (GA4) e outras fontes, no Looker Studio e no BigQuery, com foco em storytelling e decisões: funis de conversão, comparação de atribuição, conteúdo, engajamento e alertas. Use ao montar um dashboard de GA4, analisar um funil ou escolher o gráfico certo para uma métrica.
---

# Visualização de dados com GA4

Você projeta e monta visualizações de dados que transformam métricas do Google Analytics 4 (GA4) em dashboards e relatórios úteis para decidir, principalmente no Looker Studio e, quando preciso, com o BigQuery. Responda em português do Brasil.

Palavras-chave: Google Analytics 4, GA4, Looker Studio, BigQuery, atribuição, funil de conversão, dashboards interativos, engajamento, dimensões personalizadas, explorações do GA4, storytelling com dados, séries temporais, coortes.

## Antes de começar

Pergunte o que não foi informado, de uma vez só:

- **Objetivo e público:** que decisão o dashboard deve apoiar e quem vai usar (gestão, marketing, produto)?
- **Tipo de negócio:** e-commerce, geração de leads, conteúdo, aplicativo.
- **Eventos coletados:** quais eventos e eventos-chave (antigas "conversões") existem na propriedade. Os nomes reais importam.
- **Acessos:** propriedade do GA4, Looker Studio, exportação para o BigQuery.
- **Consentimento:** o site usa banner de cookies e modo de consentimento? Isso muda a leitura dos números.

Use as métricas e eventos que existem na propriedade. Não invente nomes de eventos nem números de resultado.

## Limitações do GA4 que mudam a análise

- **Consentimento (LGPD):** quem recusa cookies não é medido, ou é medido de forma modelada. Os números são estimativas, não contagem completa.
- **Limites e amostragem:** o conector do GA4 no Looker Studio tem cota de uso; relatórios muito pesados dão erro de cota. Em períodos longos ou dados com muitos valores, prefira o BigQuery.
- **Limiares de privacidade:** com poucos usuários ou com indicadores demográficos, o GA4 pode esconder linhas.
- **Diferenças entre fontes:** GA4, BigQuery, plataformas de anúncio e o sistema de vendas raramente batem exatamente. Use o sistema de vendas como verdade para receita e o GA4 para comportamento e tendência.

## Primeiros passos rápidos

1. **Conecte o GA4 ao Looker Studio:** Criar > Fonte de dados > Google Analytics, autorize e escolha a propriedade.
2. **Desempenho no tempo:** gráfico de série temporal com a dimensão `Data` e as métricas `Sessões` e `Usuários ativos`.
3. **Engajamento:** um cartão (scorecard) com `Tempo médio de engajamento` e um gráfico de barras com `Contagem de eventos` por `Nome do evento`.
4. **Filtro de período** aplicado ao relatório todo.
5. **Compartilhe** o relatório com as pessoas certas (de preferência por e-mail, não por link aberto).

---

## Fluxo 1: funil de conversão de e-commerce

1. **Confira os dados.** Veja no DebugView do GA4 se os eventos de e-commerce (`view_item`, `add_to_cart`, `begin_checkout`, `purchase`) chegam com os parâmetros certos. Para análises mais flexíveis, ative a exportação para o BigQuery (Administrador > Links de produtos > BigQuery); ela só exporta dados a partir da ativação, sem histórico.

2. **Defina as etapas:** visualizou produto → adicionou ao carrinho → iniciou checkout → comprou.

3. **Monte o funil no Looker Studio:**
   - Gráfico do tipo Funil.
   - Dimensão: `Nome do evento`, filtrada para os quatro eventos, ou um campo calculado com o nome das etapas:
     ```
     CASE
       WHEN Event name = "view_item" THEN "1. Visualizou produto"
       WHEN Event name = "add_to_cart" THEN "2. Adicionou ao carrinho"
       WHEN Event name = "begin_checkout" THEN "3. Iniciou checkout"
       WHEN Event name = "purchase" THEN "4. Comprou"
     END
     ```
   - Métrica: `Total de usuários` (usuários que dispararam cada evento). A `Contagem de eventos` infla o funil, porque o mesmo usuário vê vários produtos.
   - Ordene pelas etapas (o número no início do nome ajuda).

   Esse funil é **aberto**: conta quem fez cada etapa, não necessariamente na sequência. Para um funil sequencial de verdade, use a exploração de funil do GA4 ou o BigQuery.

4. **Taxas entre etapas.** O conector do GA4 não deixa misturar condições sobre `Nome do evento` dentro de uma métrica agregada. Duas saídas:
   - Um cartão por etapa, cada um com filtro no evento, e uma combinação de dados (blend) para calcular a taxa: `usuários add_to_cart / usuários view_item`.
   - Ou calcule no BigQuery e leve o resultado pronto ao Looker Studio.

5. **Segmentação:** controles de `Grupo de canais padrão da sessão` e `Origem / mídia da sessão`, além do filtro de período e de dispositivo.

6. **Leia o resultado:** ache a etapa com a maior queda. Se for do carrinho para o checkout, investigue a página do carrinho, o frete e a exigência de cadastro.

### Funil sequencial no BigQuery

```sql
-- Usuários que chegaram a cada etapa, na ordem, nos últimos 30 dias
WITH eventos AS (
  SELECT user_pseudo_id, event_name, event_timestamp
  FROM `seu-projeto.analytics_XXXXXXX.events_*`
  WHERE _TABLE_SUFFIX BETWEEN FORMAT_DATE('%Y%m%d', DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY))
                          AND FORMAT_DATE('%Y%m%d', DATE_SUB(CURRENT_DATE(), INTERVAL 1 DAY))
    AND event_name IN ('view_item', 'add_to_cart', 'begin_checkout', 'purchase')
),
por_usuario AS (
  SELECT
    user_pseudo_id,
    MIN(IF(event_name = 'view_item', event_timestamp, NULL)) AS t1,
    MIN(IF(event_name = 'add_to_cart', event_timestamp, NULL)) AS t2,
    MIN(IF(event_name = 'begin_checkout', event_timestamp, NULL)) AS t3,
    MIN(IF(event_name = 'purchase', event_timestamp, NULL)) AS t4
  FROM eventos
  GROUP BY user_pseudo_id
)
SELECT
  COUNTIF(t1 IS NOT NULL) AS visualizou,
  COUNTIF(t2 > t1) AS adicionou,
  COUNTIF(t3 > t2 AND t2 > t1) AS iniciou_checkout,
  COUNTIF(t4 > t3 AND t3 > t2 AND t2 > t1) AS comprou
FROM por_usuario;
```

## Fluxo 2: comparação de atribuição

1. **No GA4:** Publicidade > Atribuição > Comparação de modelos. Desde 2023, o GA4 tem só **baseado em dados** e **último clique** (canais pagos e orgânicos, ou só canais pagos do Google). Primeiro clique, linear, decadência temporal e baseado na posição foram removidos.

2. **No Looker Studio:** o conector do GA4 traz as métricas de receita e eventos-chave segundo o modelo de atribuição configurado na propriedade, nas dimensões de "sessão" (último clique, dentro das regras do GA4) e de "primeiro usuário" (aquisição). Uma comparação útil e simples:
   - Tabela com `Grupo de canais padrão do primeiro usuário` e `Grupo de canais padrão da sessão`, com receita e eventos-chave.
   - Barras agrupadas mostrando, por canal, a receita como canal de aquisição (primeiro contato) e como canal da sessão de compra (último contato).

   Confira na lista de campos do conector quais métricas de atribuição estão disponíveis; os nomes mudam com o tempo.

3. **No BigQuery:** os dados brutos não trazem a atribuição baseada em dados; trazem as origens para você montar a sua própria comparação.

   ```sql
   -- Receita por origem de aquisição do usuário e por origem da sessão (último clique)
   SELECT
     traffic_source.source AS origem_primeiro_contato,
     session_traffic_source_last_click.manual_campaign.source AS origem_sessao,
     SUM(ecommerce.purchase_revenue) AS receita
   FROM `seu-projeto.analytics_XXXXXXX.events_*`
   WHERE event_name = 'purchase'
     AND _TABLE_SUFFIX BETWEEN FORMAT_DATE('%Y%m%d', DATE_SUB(CURRENT_DATE(), INTERVAL 90 DAY))
                           AND FORMAT_DATE('%Y%m%d', DATE_SUB(CURRENT_DATE(), INTERVAL 1 DAY))
   GROUP BY 1, 2
   ORDER BY receita DESC;
   ```

   `traffic_source` é a origem do **primeiro acesso do usuário**, não da compra. O campo `session_traffic_source_last_click` existe nas exportações feitas a partir de julho de 2024; para períodos anteriores, use `collected_traffic_source` e os parâmetros dos eventos.

4. **Interpretação:** canais com mais receita como primeiro contato do que como último (comum em busca orgânica, redes sociais e conteúdo) ajudam a iniciar jornadas. Use isso para equilibrar o investimento entre canais de topo e de fundo de funil, sem tratar nenhum modelo como verdade absoluta.

---

## Modelos

### Especificação de dashboard de conteúdo

Use como roteiro para montar no Looker Studio; não é um arquivo importável.

```
Dashboard: Desempenho de conteúdo (GA4)
Objetivo: mostrar quais páginas engajam mais e de onde vem o tráfego.
Fonte: propriedade GA4 [ID]

Página 1: Visão geral
- Cartões: Sessões, Usuários ativos, Visualizações, Tempo médio de engajamento, Taxa de engajamento
- Tabela "Páginas mais acessadas": Caminho da página + Título da página
  Métricas: Visualizações, Usuários ativos, Tempo médio de engajamento, Taxa de engajamento
  Ordem: Visualizações, decrescente
- Série temporal "Visualizações e usuários por dia": Data × Visualizações, Usuários ativos
- Barras "Sessões por canal": Grupo de canais padrão da sessão × Sessões

Controles: período (padrão: últimos 28 dias, comparando com o período anterior), canal, dispositivo
```

### Especificação de alerta de queda de conversão (insight personalizado do GA4)

Configure em Página inicial > Insights > Criar (insights personalizados).

```
Nome: Queda na taxa de compra
Frequência: diária
Segmento: todos os usuários
Métrica: taxa de eventos-chave de purchase (ou purchases ÷ sessões)
Condição: variação percentual menor que -20% em relação ao período anterior
Notificar: [e-mails da equipe]
O que verificar ao receber: erro no carrinho ou checkout, falha no meio de pagamento, campanhas pausadas, queda de tráfego, problema na coleta (tag ou consentimento)
```

---

## Checklist

- [ ] Cores consistentes e com significado (ex.: verde para positivo, vermelho para negativo).
- [ ] Todos os gráficos com título descritivo e legenda clara.
- [ ] Filtros de período e segmentação funcionando e fáceis de usar.
- [ ] Tipo de gráfico adequado (linha para tempo, barras para comparar categorias).
- [ ] Sem excesso de informação em cada visualização.
- [ ] Métricas e dimensões relevantes para o objetivo e com a definição correta (usuários, sessões ou eventos).
- [ ] Cores acessíveis: contraste suficiente e combinações que funcionam para daltônicos.
- [ ] Testado em telas diferentes.
- [ ] Narrativa clara: o dashboard responde primeiro à pergunta principal.
- [ ] Fonte de dados conectada, com a data da última atualização visível.
- [ ] Limitações explicadas (consentimento, amostragem, diferenças com o sistema de vendas).

## Métricas de referência

Valores indicativos da skill original, sem fonte citada, e em dólar. Variam muito por segmento, ticket e país; compare principalmente com o histórico do próprio site.

| Métrica do GA4 | Referência (e-commerce) | Meta |
|---|---|---|
| Taxa de conversão de compra | 1,5 a 2,5% | acima de 3% |
| Taxa de engajamento | 60 a 75% | acima de 75% |
| Receita por usuário | US$ 15 a 30 | acima de US$ 35 |
| Receita por sessão | US$ 0,50 a 1,50 | acima de US$ 2 |
| Abandono de carrinho | 60 a 75% | abaixo de 60% |
| Tempo médio de engajamento | acima de 60 s | acima de 90 s |

## Erros comuns

1. **Pizza com mais de 5 fatias.** Fica ilegível. Use barras ou agrupe as menores em "Outros".
2. **Cores inconsistentes.** Defina uma paleta e use a mesma cor para a mesma coisa em todos os gráficos (ex.: orgânico sempre azul, pago sempre verde).
3. **Comparar totais de bases diferentes.** Use taxas e percentuais. Comparar a taxa de conversão de dois canais diz mais que comparar o número de conversões quando o tráfego é muito diferente.
4. **Excesso num só gráfico ou dashboard.** Cada gráfico, uma mensagem. Divida em páginas e use filtros para os detalhes. Em vez de um gráfico com 10 linhas, faça 2 ou 3 com 2 ou 3 linhas cada.
5. **Dados sem contexto.** Títulos que dizem o que olhar ("Sessões por mês: pico em março com a campanha X") e anotações nos gráficos importantes.
6. **Misturar usuários, sessões e eventos.** São bases diferentes; deixe claro qual cada gráfico usa.

## Dicas avançadas

1. **Pequenos múltiplos:** o mesmo gráfico repetido para cada região, produto ou canal, com escalas iguais, para comparar padrões.
2. **Relatórios pré-filtrados por link:** com parâmetros de relatório habilitados no Looker Studio, dá para abrir o dashboard já filtrado por cliente ou campanha, passando os valores na URL. Confira na documentação do Looker Studio o formato atual dos parâmetros.
3. **Métricas de negócio no BigQuery:** cálculos complexos, como receita por sessão engajada, ficam no BigQuery e chegam prontos ao Looker Studio. No BigQuery, sessão engajada é identificada pelo parâmetro `session_engaged = '1'`, e o ID da sessão é a combinação de `user_pseudo_id` com o parâmetro `ga_session_id`.
4. **Prototipe nas explorações do GA4:** teste hipóteses nas explorações de funil e de caminho antes de montar o dashboard.
5. **Dados de CRM e vendas:** junte ao GA4 os dados do CRM (Salesforce, HubSpot, RD Station) ou de vendas por conectores do Looker Studio, Google Sheets ou BigQuery. Ao cruzar dados de usuários, respeite a LGPD: use identificadores pseudonimizados e só os dados necessários.

## Formato da resposta

Entregue: (1) o objetivo do dashboard em uma frase; (2) a estrutura (páginas, gráficos, métricas, dimensões e filtros); (3) campos calculados e consultas prontos; (4) como ler os resultados; e (5) as limitações e o que o usuário precisa confirmar (eventos, acessos, consentimento).

---

## Créditos e licença

Skill adaptada a partir da skill `data-visualization` (versão 1.0.0), de autoria de **Cafe Code AI**, publicada em [kursku/skills](https://github.com/kursku/skills) (pasta `packs/kit-510-ptbr/15-analytics-dados/data-visualization`). A licença **MIT** está declarada nos metadados da skill original; o repositório não traz um arquivo de licença próprio.

Alterações desta versão: correção dos modelos de atribuição do GA4 (primeiro clique, linear, decadência temporal e baseado na posição foram removidos em 2023); consulta do BigQuery corrigida (vírgula antes do `FROM` impedia a execução e `traffic_source` era tratado como último clique, quando é a origem do primeiro acesso), com o campo de origem da sessão; fórmulas do Looker Studio que usavam SQL não aceito pelo conector do GA4 trocadas por alternativas que funcionam; funil com usuários em vez de contagem de eventos, com explicação de funil aberto e um funil sequencial em SQL; os "templates" JSON, que não são importáveis, viraram especificações; inclusão das seções "Antes de começar", "Limitações do GA4" (consentimento e LGPD, cotas, limiares, diferenças entre fontes) e "Formato da resposta"; aviso de que as métricas de referência não têm fonte e estão em dólar; checklist desmarcado para uso.

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
