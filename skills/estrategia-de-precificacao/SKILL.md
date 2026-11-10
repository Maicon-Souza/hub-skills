---
name: estrategia-de-precificacao
description: Analisa, define e testa preços de produtos e serviços (SaaS, infoprodutos, cursos, serviços), com custos, margem de contribuição, ponto de equilíbrio, concorrência, valor percebido, planos, testes de preço e respostas à objeção "está caro". Use ao precificar um produto novo, revisar preços, montar planos ou simular cenários de receita.
---

# Estratégia de precificação

Você ajuda a analisar, definir e otimizar preços de produtos e serviços para aumentar receita e lucro, do cálculo de custos ao teste de preços no funil de vendas. Responda em português do Brasil, com valores em reais.

Palavras-chave: precificação, valor percebido, CAC, LTV, elasticidade-preço, assinatura, freemium, preço psicológico, skimming, penetração, bundling (pacotes), descontos progressivos, ponto de equilíbrio, margem de contribuição, parcelamento.

## Antes de começar

Pergunte o que não foi informado, de uma vez só:

- **O que é vendido:** SaaS, curso, infoproduto, serviço, produto físico? Venda única ou recorrente?
- **Custos:** fixos mensais, custos variáveis por venda ou por cliente, taxas de pagamento, regime tributário.
- **Mercado:** público-alvo, principais concorrentes e seus preços, diferenciais reais.
- **Situação atual:** é lançamento ou revisão? Se já vende, quais são o preço, a conversão, o churn e o CAC atuais?
- **Objetivo:** ganhar mercado, maximizar margem, aumentar ticket, reduzir churn?

Use os números do usuário. Nunca invente custos, preços de concorrentes ou resultados de testes: o que faltar vira pergunta ou marcador `[ ]`, e todo número de exemplo deve ser apresentado como exemplo.

## Primeiros passos rápidos

1. **Calcule o custo total do produto.** Exemplo, "Curso de Marketing Digital Avançado": hospedagem, plataforma, licenças de ferramentas, horas de produção, taxas de pagamento e impostos.
2. **Levante 3 a 5 concorrentes diretos:** preço, formato, o que entregam, diferenciais.
3. **Simule três cenários de preço** (exemplo: R$ 497, R$ 697 e R$ 897), projetando receita e margem de contribuição para [50] vendas em cada um. Inclua também cenários com volumes diferentes, porque o preço muda a quantidade vendida.
4. **Monte uma matriz de valor percebido:** módulos, bônus e suporte, e o que cada um entrega a mais que os concorrentes.

---

## Fluxo 1: preço de um produto SaaS novo

Exemplo: "CRM Inteligente para Pequenas Empresas".

1. **Custos**
   - **Variáveis por cliente (conta):** taxa de pagamento de 2,5% sobre a receita; processamento de dados de R$ 0,50 por mês (estimando 1 GB por conta); suporte de R$ 0,50 por mês (R$ 5 por chamado × 0,1 chamado por conta).
   - **Impostos sobre a receita:** dependem do regime. No Simples Nacional, a alíquota efetiva varia conforme o anexo e o faturamento; confirme com o contador. No exemplo, 6%.
   - **Fixos:** servidores R$ 1.500, licenças R$ 800, equipe R$ 15.000, marketing R$ 3.000. **Total: R$ 20.300 por mês.**

2. **Concorrência**
   - Concorrente A: Básico R$ 79/mês (até 5 usuários, 1.000 contatos); Pro R$ 149/mês (ilimitado, automação).
   - Concorrente B: Starter R$ 59/mês (1 usuário, 500 contatos); Business R$ 119/mês (3 usuários, 5.000 contatos).
   - Nosso diferencial: IA que qualifica leads e automatiza follow-up, que os concorrentes nessa faixa não têm.

3. **Proposta de valor.** Exemplo: "A IA do CRM Inteligente qualifica leads em tempo real, escreve os e-mails de follow-up e sugere o próximo passo, economizando cerca de [10] horas por semana por vendedor." Use só ganhos que você consiga comprovar (testes, clientes piloto).

4. **Estratégia inicial.** Escolha a abordagem:
   - **Baseada em valor:** o preço acompanha o valor gerado para o cliente.
   - **Penetração:** preço baixo para ganhar mercado rápido.
   - **Skimming (desnatamento):** preço alto no início para quem mais valoriza, reduzindo depois.

   Exemplo, baseada em valor com plano premium:
   - **Essencial, R$ 89/mês:** até 3 usuários, 2.000 contatos, CRM básico. Competitivo.
   - **Inteligente, R$ 179/mês:** até 10 usuários, 10.000 contatos, CRM completo, IA de qualificação e automação avançada. Preço puxado pelo diferencial.

5. **Receita e ponto de equilíbrio**

   Supondo 70% dos clientes no Essencial e 30% no Inteligente:

   ```
   Receita média por cliente (ARPU) = 0,7 × 89 + 0,3 × 179 = 62,30 + 53,70 = R$ 116,00

   Custos variáveis por cliente:
     dados e suporte            R$ 1,00
     taxa de pagamento (2,5%)   R$ 2,90
     impostos (6%, exemplo)     R$ 6,96
     total                      R$ 10,86

   Margem de contribuição por cliente = 116,00 − 10,86 = R$ 105,14

   Ponto de equilíbrio = custos fixos ÷ margem de contribuição
                       = 20.300 ÷ 105,14 ≈ 193,1  →  194 clientes pagantes
   ```

   Sem considerar impostos, a conta daria 182 clientes. Arredonde sempre para cima.

   Depois do ponto de equilíbrio, confira se o **CAC** (custo para conquistar cada cliente) cabe no **LTV**: uma regra comum de mercado é LTV pelo menos 3 vezes o CAC e recuperar o CAC em até 12 meses.

## Fluxo 2: testar e otimizar o preço de um produto que já vende

Exemplo: curso "Lançamento de Produtos Digitais", R$ 997, conversão atual de 2,5%.

1. **Ache onde o funil perde gente.** Exemplo: só 25% de quem vê a página de preços inicia o checkout; 40% de quem inicia conclui. A queda grande antes do checkout sugere que o preço (ou a forma de apresentá-lo) trava a decisão.

2. **Formule hipóteses:**
   - **A, preço menor:** R$ 797.
   - **B, mais valor pelo mesmo preço:** R$ 997 com bônus (template de landing page + mentoria em grupo, valor de R$ 400).
   - **C, parcelamento maior:** R$ 997 em 12x sem juros (antes 6x). Lembre que o vendedor paga a antecipação das parcelas.

3. **Configure o teste.**
   - Ferramentas: VWO, Optimizely, AB Tasty ou o recurso de teste da sua plataforma de vendas. O Google Optimize foi descontinuado em 2023.
   - **Cuidado com preços diferentes ao mesmo tempo:** mostrar preços diferentes para o mesmo produto, no mesmo momento, pode ser visto como discriminação de preço pelo Código de Defesa do Consumidor e gera desconfiança se o cliente perceber. Alternativas mais seguras: testar bônus e condições de pagamento (B e C), testar preços em períodos alternados, ou consultar o jurídico antes.
   - **Tamanho da amostra:** com conversões em torno de 3%, diferenças pequenas (3,0% contra 3,2%) exigem dezenas de milhares de visitantes por variante. Calcule a amostra antes e não encerre o teste no primeiro resultado bom. Mínimo de 2 a 4 semanas, cobrindo ciclos completos de semana.

4. **Analise pela receita, não só pela conversão.** A métrica principal é a **receita por visitante** (conversão × preço), descontando os custos de cada variante.

   Exemplo de resultado depois de 3 semanas, a cada 100 visitantes:

   | Variante | Conversão | Preço | Receita por 100 visitantes | Observação |
   |---|---|---|---|---|
   | Atual | 2,5% | R$ 997 | R$ 2.492,50 | base |
   | A, R$ 797 | 3,8% | R$ 797 | R$ 3.028,60 (+21%) | Ticket 20% menor, mas a receita total subiu |
   | B, R$ 997 + bônus | 3,2% | R$ 997 | R$ 3.190,40 (+28%) | Descontar o custo da mentoria em grupo |
   | C, 12x sem juros | 3,0% | R$ 997 | R$ 2.991,00 (+20%) | Descontar o custo de antecipação |

   Nesse exemplo, B ganha: mais receita por visitante sem baixar o preço e sem treinar o público a esperar desconto. Confirme se as diferenças são estatisticamente significativas antes de decidir.

5. **Implemente e continue testando.** Adote a vencedora como padrão e planeje o próximo teste (exemplo: qual bônus gera mais valor percebido).

---

## Modelos

### Matriz de valor percebido

```
Matriz de valor percebido – GerenciadorPro, plano Premium (SaaS)

Público: pequenas e médias empresas com equipe de vendas
Concorrente principal: SalesFlow Business (R$ 129/mês)
Preço sugerido: R$ 149/mês

| Funcionalidade | Valor para o cliente | Nosso diferencial | Como o concorrente oferece |
|---|---|---|---|
| Automação de follow-up (e-mail, WhatsApp) | Menos tempo da equipe, mais respostas | IA sugere mensagens personalizadas | Automação sem IA |
| CRM integrado | Dados do cliente num só lugar | Relatórios montados arrastando e soltando | Relatórios fixos |
| Relatórios de desempenho | Decisões com dados, gargalos à vista | Previsão de vendas | Só histórico |
| Suporte por chat e telefone | Problema resolvido rápido | Chamado crítico atendido em até 30 min | Só e-mail |
| Integração com ERP | Menos erro manual | Conectores prontos para [ERPs] | Via API, precisa de desenvolvedor |

Justificativa dos R$ 149 (R$ 20 acima do concorrente):
IA na automação, previsão de vendas, suporte prioritário e integrações prontas, que o concorrente não tem ou exige desenvolvimento. Posiciona o plano como a opção premium para PMEs que querem ganhar tempo.
```

### Roteiro para a objeção "está caro"

```
Contexto: o cliente acha caro o plano Avançado (R$ 199/mês) e viu opções de R$ 120 a R$ 150.

1. Reconheça
"Faz sentido olhar com cuidado; é um investimento mensal."

2. Entenda a comparação
"Você está comparando com quais soluções? O que elas incluem?"

3. Mostre a diferença que importa para ele
"A diferença principal do Avançado é [funcionalidade exclusiva]. Nos nossos clientes, ela tem gerado em média [resultado real e comprovável, ex.: X% mais conversão de leads]."

4. Traga para os números dele
"Quantos leads vocês recebem por mês e quanto custa cada um? [...] Com esses números, [X%] a mais de conversão representa cerca de [N] vendas extras por mês, ou R$ [Y]. A diferença de preço para as outras opções é de R$ 49 a R$ 79."

5. Ofereça um caminho
"Se o orçamento estiver apertado agora, o Básico, de R$ 99, resolve [o essencial], mas não inclui [funcionalidade-chave]. Pelo que você me contou sobre [objetivo do cliente], eu iria de Avançado. Quer que eu monte essa simulação com os seus números e te mande?"
```

---

## Checklist

- [ ] Mapear todos os custos fixos e variáveis, incluindo impostos e taxas de pagamento.
- [ ] Calcular margem de contribuição e ponto de equilíbrio.
- [ ] Calcular o LTV médio por segmento e compará-lo ao CAC.
- [ ] Levantar os preços e planos de pelo menos 3 concorrentes diretos.
- [ ] Montar a matriz de valor percebido de cada plano ou produto.
- [ ] Definir uma proposta de valor clara para cada faixa de preço.
- [ ] Testar ofertas ou preços com amostra suficiente e de forma juridicamente segura.
- [ ] Analisar o impacto do preço no churn e no ARPU.
- [ ] Treinar o time de vendas para a objeção de preço.
- [ ] Revisar a precificação a cada trimestre ou semestre.

## Métricas e fórmulas

| Métrica | Fórmula |
|---|---|
| Margem de contribuição | Preço − custos variáveis (taxas, impostos, entrega) |
| Ponto de equilíbrio | Custos fixos ÷ margem de contribuição por venda ou cliente |
| ARPU | Receita recorrente ÷ número de clientes |
| LTV (recorrente) | ARPU × margem bruta % ÷ churn mensal |
| LTV ÷ CAC | Regra comum de mercado: 3 ou mais |
| Payback do CAC | CAC ÷ (ARPU × margem bruta %), em meses; regra comum: até 12 |
| Receita por visitante | Conversão × ticket médio |
| Elasticidade-preço | Variação % da quantidade vendida ÷ variação % do preço |

## Erros comuns

1. **Preço só pelo custo mais margem.** Ignora o valor gerado e a concorrência. Use o custo como piso, não como preço.
2. **Copiar o concorrente.** Sem saber os custos e o posicionamento dele, você copia também os erros.
3. **Esquecer impostos e taxas.** No Brasil, impostos sobre a receita, taxas de cartão e antecipação de parcelas podem comer boa parte da margem.
4. **Medir só conversão.** Preço menor quase sempre converte mais; a pergunta é se dá mais receita e margem.
5. **Desconto como hábito.** Promoções frequentes ensinam o cliente a esperar a próxima. Prefira bônus e condições de pagamento.
6. **Nunca revisar.** Custos, concorrência e valor do produto mudam; o preço também deve ser revisto.

## Formato da resposta

Entregue: (1) resumo da recomendação de preço, com a justificativa; (2) os cálculos mostrados passo a passo (custos, margem, ponto de equilíbrio, cenários); (3) a estrutura de planos ou ofertas; (4) como testar e o que medir; e (5) as premissas e os dados que o usuário precisa confirmar.

---

## Créditos e licença

Skill adaptada a partir da skill `pricing-strategy` (versão 1.0.0), de autoria de **Cafe Code AI**, publicada em [kursku/skills](https://github.com/kursku/skills) (pasta `packs/kit-510-ptbr/03-funis-vendas/pricing-strategy`). A licença **MIT** está declarada nos metadados da skill original; o repositório não traz um arquivo de licença próprio.

Alterações desta versão: correção da análise do teste A/B (o original dizia que a variante de R$ 797 mantinha a margem estável, quando a receita por visitante subia 21%), com tabela de receita por visitante; inclusão dos impostos no cálculo do ponto de equilíbrio e arredondamento para cima; troca do Google Optimize (descontinuado em 2023) por ferramentas atuais; avisos sobre preços diferentes simultâneos (Código de Defesa do Consumidor), tamanho de amostra e custo do parcelamento; troca do termo "TCO" por custo total do produto; matriz de valor percebido reorganizada; roteiro da objeção de preço reescrito com números a confirmar; seção de métricas, que estava cortada no original, refeita como tabela de fórmulas; inclusão das seções "Antes de começar", "Erros comuns" e "Formato da resposta"; checklist desmarcado para uso.

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
