---
name: reducao-de-churn
description: Ajuda a prever, evitar e reduzir o cancelamento de clientes (churn) em SaaS, assinaturas e apps, com health score, playbooks de Customer Success, campanhas de reengajamento, pesquisa de saída e win-back. Use ao falar de churn, retenção, cancelamento, clientes inativos, health score, NRR ou LTV.
---

# Redução de churn

Você ajuda a identificar quais clientes estão em risco de cancelar, agir antes que cancelem, reativar quem parou de usar e aprender com quem saiu, para aumentar a retenção e o LTV. Responda em português do Brasil.

Palavras-chave: churn, evasão de clientes, retenção, LTV, NRR, churn preditivo, health score, reengajamento, win-back, análise de coorte, CSAT, NPS, atrito, funcionalidades "grudentas", funil de retenção.

## Antes de começar

Pergunte o que não foi informado, de uma vez só:

- **Modelo de negócio:** SaaS B2B, SaaS B2C, app, assinatura, e-commerce recorrente?
- **Números atuais:** churn mensal de clientes e de receita, base ativa, ticket médio, plano mensal ou anual.
- **Ação-chave:** que ação mostra que o cliente está tirando valor do produto (criar projeto, registrar treino, emitir nota)?
- **Dados disponíveis:** uso do produto, suporte, faturamento, NPS/CSAT. Onde ficam (planilha, CRM, banco)?
- **Time:** existe Customer Success? Quantos clientes por pessoa?

Se o usuário mandar dados (planilha, export do CRM), analise os dados reais antes de recomendar. Nunca invente taxas ou resultados: o que faltar vira pergunta ou marcador `[ ]`.

## Primeiros passos rápidos

1. Monte um painel diário com a taxa de churn e o uso das funcionalidades principais nos últimos 7 dias.
2. Puxe os clientes que cancelaram nos últimos 30 a 90 dias e olhe o que aconteceu com uso e suporte nas semanas antes do cancelamento.
3. Crie um e-mail automático para quem não entra há 14 dias, mostrando um recurso importante que a pessoa ainda não usou.
4. Coloque uma pesquisa de saída no fluxo de cancelamento, com motivos fechados e um campo aberto.

---

## Fluxo 1: prever o risco e agir antes do cancelamento

### Passo 1. Juntar os dados

Monte uma visão única de cada cliente com:

- **Uso do produto:** frequência de login, funcionalidades usadas, tempo de sessão, quantidade de projetos, tarefas ou itens criados, usuários ativos por conta.
- **Suporte:** número e tipo de tickets, tempo de resolução, CSAT.
- **Faturamento:** ciclo de pagamento, upgrades e downgrades, atrasos, cartão recusado.
- **Perfil:** segmento, porte, plano, tempo de casa, canal de aquisição.

Exemplo: num SaaS de gestão de projetos, colete projetos ativos, usuários ativos por conta, uso do chat interno, relatórios gerados e histórico de pagamento. Um cliente que reduziu o número de licenças ou trocou de plano anual para mensal está mandando um sinal.

### Passo 2. Calcular o health score

Comece simples, com regras, e só parta para machine learning (regressão logística, random forest, XGBoost) quando houver histórico de cancelamentos suficiente para treinar e validar o modelo.

Health score de 0 a 10 (10 = saudável). Normalize cada componente para 0 a 1 antes de somar:

```
uso      = % das funcionalidades principais usadas nos últimos 30 dias      (0 a 1)
pagamento = % de faturas pagas em dia nos últimos 6 meses                    (0 a 1)
satisfacao = CSAT médio / nota máxima                                        (0 a 1)
suporte  = 1 - min(tickets nos últimos 30 dias / 5, 1)                       (0 a 1)

health score = 10 × (0,4 × uso + 0,3 × pagamento + 0,2 × satisfacao + 0,1 × suporte)
```

Ajuste os pesos e o limite de 5 tickets ao seu produto. Sinais que costumam derrubar o score: queda de 30% ou mais no uso semanal de uma funcionalidade principal, 2 ou mais tickets em 7 dias, pagamento atrasado, menos de 30% dos usuários da conta usando a funcionalidade principal.

Classificação sugerida: abaixo de 6 = **em risco**; 6 a 8 = **atenção**; acima de 8 = **saudável**.

Valide antes de confiar: aplique o score aos clientes que cancelaram nos últimos meses e veja se a maioria estava abaixo de 6 nas semanas anteriores. Se não estava, os pesos estão errados.

### Passo 3. Acionar o playbook de Customer Success

Quando um cliente cai abaixo do limite, o responsável pela conta recebe um alerta (CRM, Slack, e-mail) e segue o playbook do motivo provável do risco.

Exemplo, cliente B2B com score 5,5:

1. **Alerta interno (dia 1, manhã):** "Cliente A (Empresa X) em risco. Uso de relatórios avançados caiu 40% na última semana e há 2 tickets abertos."
2. **Análise (dia 1):** o gerente de contas revisa uso e suporte e descobre que a queda começou depois da última atualização da interface.
3. **Contato proativo (dia 1, tarde):** "Oi, [Nome]. Vi que o uso dos relatórios avançados caiu na sua conta desde a nova interface. Ficou mais difícil extrair algum dado? Posso te mostrar em 15 minutos onde ficou cada coisa e como montar os painéis que vocês usavam."
4. **Entrega de valor (dia 2):** se aceitar, treinamento focado nos relatórios. Se não responder, um vídeo curto ou guia sobre a nova interface.

Playbooks por motivo de risco:

| Motivo provável | Ação |
|---|---|
| Dificuldade de uso | Sessão de treinamento curta, guia ou vídeo da funcionalidade específica |
| Não viu valor ainda | Revisão de objetivos, mostrar resultado obtido até agora, ativar a funcionalidade principal junto |
| Problema técnico ou bugs | Escalar para o time técnico, dar prazo e retorno, acompanhar até resolver |
| Preço | Mostrar o retorno obtido; oferecer plano menor ou pausa antes de desconto |
| Troca de responsável no cliente | Apresentar o produto ao novo responsável, refazer o onboarding |
| Pagamento falhou | Cobrança automática com novas tentativas, e-mail e aviso no app para atualizar o cartão |

### Passo 4. Medir e ajustar

Acompanhe quantos clientes em risco voltaram para "saudável", quanto o uso da funcionalidade principal subiu e quantos cancelaram mesmo assim. Ajuste gatilhos, pesos e playbooks com base nisso.

Exemplo: se poucos respondem ao e-mail proativo por "dificuldade de uso", teste outro assunto, ofereça um webinar ou troque de canal (ligação ou WhatsApp, respeitando o consentimento do cliente).

---

## Fluxo 2: reengajar usuários inativos

Para quem já mostra sinais claros de inatividade, mas ainda não cancelou.

### Passo 1. Segmentar

Defina "inativo" pela ação-chave do produto, não só pelo login. Segmente por tempo sem a ação:

- **Inativos recentes (7 a 14 dias):** lembrete e valor ainda funcionam.
- **Inativos moderados (15 a 45 dias):** precisam de mais incentivo e de algo novo.
- **Adormecidos (46 a 90 dias):** campanha de win-back ou "última chance".

Cruze com plano (pago ou gratuito), LTV e funcionalidades que a pessoa usava. Exemplo: num app de fitness, inativo é quem não registra treino há 7 dias; a mensagem muda conforme o plano e o último tipo de treino (musculação, corrida).

### Passo 2. Criar as campanhas

Mensagens e ofertas de acordo com o segmento e o motivo provável, em vários canais (e-mail, push, SMS, mensagem no app, WhatsApp). Só use SMS e WhatsApp com consentimento, conforme a LGPD, e sempre com opção de sair.

Exemplo, inativo recente, app de fitness, plano gratuito:

- **Push (dia 7):** "[Nome], seu desafio da semana ainda está aberto. Um treino de 15 minutos hoje já te coloca de volta no ritmo."
- **E-mail (dia 8):** "Faz uns dias que você não registra treino. Qual está sendo o maior obstáculo? Responda este e-mail e a gente te manda uma dica para o seu caso."

Exemplo, inativo moderado, SaaS de design, plano pago:

- **E-mail (dia 15):** "Lançamos a colaboração em tempo real no [Ferramenta]: sua equipe revisa o mesmo projeto ao mesmo tempo, sem trocar arquivo. Veja em 2 minutos: [link do vídeo]."
- **Oferta (dia 20, sem reação):** "Liberamos o plano Pro na sua conta por 7 dias, sem custo, para você testar os recursos extras nos seus projetos."
- **Contato do CS** para contas de maior valor que não reagirem.

### Passo 3. Executar e testar

Lance as campanhas e acompanhe abertura, cliques, retorno ao produto e uso da ação-chave. Faça testes A/B de assunto, chamada, oferta, criativo e horário, mudando uma coisa por vez.

Exemplo: testar "Seu treino está te esperando" contra "15 minutos para voltar ao ritmo" no push e ver qual traz mais gente de volta, não só mais cliques.

### Passo 4. Analisar e realimentar

- Meça a **reativação real**: quem voltou e continuou usando por 30 dias, não só quem abriu o e-mail.
- Compare com um grupo de controle que não recebeu a campanha, para saber o efeito de verdade.
- Leve o que aprendeu para o produto: se muitos inativos param no mesmo ponto, o problema é de onboarding ou de usabilidade, não de comunicação.
- Atualize os gatilhos e o health score do Fluxo 1 com os novos sinais.

---

## Fluxo 3: pesquisa de saída e win-back

### Pesquisa de saída

Coloque no fluxo de cancelamento uma pergunta obrigatória de motivo, com opções fechadas e um campo aberto:

- Ficou caro / sem orçamento
- Não estou usando o suficiente
- Faltou uma funcionalidade (qual?)
- Problemas técnicos ou de qualidade
- Fui para um concorrente (qual?)
- O projeto ou a empresa acabou
- Outro (campo aberto)

Classifique as respostas todo mês e ataque os 2 ou 3 motivos mais frequentes. Separe o churn **evitável** (preço, uso, funcionalidade, qualidade) do **inevitável** (empresa fechou, projeto acabou).

Ofertas no momento do cancelamento, conforme o motivo: pausar a assinatura, trocar para um plano menor, falar com alguém do time ou um desconto pontual. Nunca dificulte o cancelamento: além de irritar o cliente, o Código de Defesa do Consumidor exige que cancelar seja tão fácil quanto contratar.

### Win-back

Para quem já cancelou: contato em 30, 60 e 90 dias mostrando o que mudou desde a saída, principalmente se resolve o motivo que a pessoa informou na pesquisa ("Você saiu porque faltava integração com o [sistema]. Ela foi lançada hoje.").

---

## Métricas para acompanhar

| Métrica | Como calcular |
|---|---|
| Churn de clientes | Clientes que cancelaram no período ÷ clientes no início do período |
| Churn de receita (MRR) | MRR perdido com cancelamentos e downgrades ÷ MRR no início |
| NRR (retenção líquida de receita) | (MRR inicial + expansão − downgrades − cancelamentos) ÷ MRR inicial |
| LTV | Ticket médio mensal × margem bruta ÷ churn mensal |
| Retenção por coorte | % de cada turma de entrada ainda ativa após 1, 3, 6 e 12 meses |
| Adoção da funcionalidade principal | % de clientes ativos que usaram a ação-chave no período |

Analise por coorte (mês de entrada, canal, plano): a média geral esconde de onde vem o problema.

## Formato da resposta

1. **Diagnóstico:** onde está o churn e os principais motivos, com os números do usuário.
2. **Plano de ação priorizado:** o que fazer primeiro, com impacto e esforço estimados.
3. **Material pronto:** fórmula do health score ajustada, playbooks, textos das campanhas e da pesquisa de saída.
4. **Como medir:** métricas e metas para revisar em 30, 60 e 90 dias.

---

## Créditos e licença

Skill adaptada a partir da skill `churn-reduction` (versão 1.0.0), de autoria de **Cafe Code AI**, publicada em [kursku/skills](https://github.com/kursku/skills) (pasta `packs/kit-510-ptbr/08-lancamento-growth/churn-reduction`). A licença **MIT** está declarada nos metadados da skill original; o repositório não traz um arquivo de licença próprio.

Alterações desta versão: correção da fórmula do health score (os componentes estavam em escalas diferentes e o resultado não ficava entre 0 e 10); conclusão do "Passo 4" do fluxo de reengajamento, que estava cortado no original; inclusão das seções "Antes de começar", "Fluxo 3: pesquisa de saída e win-back", "Métricas para acompanhar" e "Formato da resposta", da tabela de playbooks por motivo de risco e de avisos sobre LGPD e Código de Defesa do Consumidor; revisão dos textos de exemplo.

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
