---
name: analise-de-reunioes
description: Analisa transcrições de reuniões para mostrar padrões de comunicação de quem pede a análise, com exemplos e sugestões práticas. Aponta quando a pessoa evita conflito, usa muletas de linguagem ("tipo", "né", "então"), fala demais, interrompe, deixa de ouvir ou conduz mal a reunião. Use para desenvolver comunicação e liderança a partir de reuniões reais.
---

# Análise de reuniões

Você transforma transcrições de reuniões em observações práticas sobre a comunicação de quem pede a análise, para que a pessoa se comunique e lidere melhor. Responda em português do Brasil.

## Quando usar

- Entender os próprios padrões de comunicação em várias reuniões.
- Receber retorno sobre o próprio jeito de liderar e conduzir reuniões.
- Ver quando a pessoa evita conversas difíceis.
- Conhecer os próprios hábitos de fala e muletas de linguagem.
- Acompanhar a evolução ao longo do tempo.
- Juntar exemplos concretos para a própria avaliação de desempenho.

## Privacidade e uso ético

Transcrições têm a fala e os dados pessoais de outras pessoas.

- **Foco em quem pediu.** A análise é sobre a comunicação do próprio usuário. Os outros participantes aparecem só como contexto, sem avaliar nem rotular ninguém.
- **Ciência dos participantes.** Gravar e transcrever reuniões exige que os participantes saibam disso, conforme a LGPD e a política da empresa. Se não estiver claro que houve aviso, lembre o usuário.
- **Avaliar outras pessoas** (por exemplo, um gestor analisando a fala da equipe) só com o conhecimento delas e para desenvolvimento combinado, nunca como vigilância nem como base única para decisões de desempenho. Nesse caso, prefira que a própria pessoa receba a análise.
- **Dados sensíveis:** se a transcrição tiver informações de saúde, financeiras ou confidenciais, não as reproduza além do necessário para o exemplo.
- **Arquivos locais.** Mantenha as transcrições em pasta local; não envie para serviços que o usuário não autorizou.

## O que a análise cobre

1. **Padrões de comportamento** que se repetem: evitar conflito ou falar de forma indireta, tempo de fala e troca de turnos, perguntas em comparação com afirmações, sinais de escuta ativa, jeito de decidir.
2. **Eficácia da comunicação:** clareza e objetividade, muletas e atenuações, tom, condução da reunião.
3. **Retorno prático:** exemplos específicos, com o momento da reunião, dizendo o que aconteceu, por que importa e como fazer melhor.
4. **Evolução:** comparação entre períodos quando houver várias reuniões.

## Como preparar

1. Baixe as transcrições para uma pasta (ex.: `~/reunioes/`).
2. Abra essa pasta no Claude Code ou anexe os arquivos na conversa.
3. Peça a análise que quiser.

Exemplos de pedido:

```
Analise as reuniões desta pasta e me mostre quando eu evitei conflito.
```

```
Olhe minhas reuniões do último mês e identifique meus padrões de comunicação.
```

```
Compare meu jeito de conduzir reuniões nestas duas pastas.
```

```
Analise as transcrições desta pasta e:
1. Mostre quando eu interrompi alguém
2. Calcule quanto do tempo eu falei
3. Encontre momentos em que evitei dar um retorno direto
4. Conte minhas muletas de linguagem
5. Mostre exemplos em que ouvi bem
```

---

## Passo a passo

### 1. Levante os dados
- Procure transcrições na pasta (.txt, .md, .vtt, .srt, .docx).
- Veja se têm identificação de quem fala e marcação de tempo.
- Confirme o período das reuniões.
- Descubra como o usuário aparece nas transcrições (nome, apelido, "Locutor 1").

Avise das limitações: transcrição automática erra palavras e às vezes troca quem está falando, principalmente com vozes sobrepostas. Sem marcação de tempo, não dá para calcular tempo de fala com precisão; use a contagem de palavras como aproximação e diga isso.

### 2. Entenda o objetivo
Se o usuário não disse, pergunte o que ele quer descobrir:
- Comportamentos específicos (evitar conflito, interromper, muletas).
- Eficácia (clareza, objetividade, escuta).
- Condução de reuniões.
- Tempo de fala.
- Pontos a desenvolver.

### 3. Analise

**Evitar conflito:**
- Atenuações em excesso: "talvez", "meio que", "acho que", "de repente", "só uma ideia".
- Pedidos indiretos em vez de diretos.
- Mudar de assunto quando surge tensão.
- Concordar sem se comprometer ("é, mas...", "pode ser").
- Não tocar em problemas evidentes.
- Encerrar com "o que você achar melhor" quando a decisão era sua.

**Tempo de fala:**
- Porcentagem do tempo (ou das palavras) que o usuário falou.
- Interrupções feitas e sofridas.
- Duração média de cada fala.
- Proporção entre perguntas e afirmações.

**Muletas de linguagem:**
- Conte "é...", "hum", "tipo", "né", "então", "aí", "assim", "na verdade", "basicamente", "sabe?".
- Frequência por minuto ou por fala.
- Em que situações aumentam (assunto difícil, pergunta inesperada, diante de alguém mais sênior).
- Lembre que algumas muletas são normais na fala; o problema é o excesso, e transcrições automáticas às vezes cortam ou inventam essas palavras.

**Escuta ativa:**
- Perguntas que retomam o que outra pessoa disse.
- Paráfrases ou resumos da ideia do outro.
- Construir em cima da contribuição de alguém.
- Perguntas de esclarecimento.

**Liderança e condução:**
- Jeito de decidir (diretivo ou colaborativo).
- Como lida com discordâncias.
- Se dá espaço para quem fala pouco.
- Gestão do tempo e da pauta.
- Clareza de responsáveis, prazos e próximos passos no fim.

### 4. Dê exemplos concretos

Para cada padrão, use citações reais da transcrição:

```markdown
### [Nome do padrão]

**O que vimos:** [resumo do padrão em uma frase]
**Frequência:** [X vezes em Y reuniões]

**Exemplo 1: [reunião, data], [tempo]**

O que aconteceu:
> [citação real da transcrição]

Por que importa:
[o efeito ou a oportunidade perdida]

Como poderia ser:
[frase ou atitude alternativa, específica]

[2 ou 3 exemplos mais fortes]
```

Nunca invente citações nem números. Se um padrão aparecer pouco, diga isso em vez de forçar a conclusão.

### 5. Faça a síntese

```markdown
# Resumo da análise de reuniões

Período: [datas]
Reuniões analisadas: [X]
Duração total: [X horas]

## Principais padrões
### 1. [Padrão principal]
- O que vimos: [...]
- Por que importa: [...]
- Recomendação: [...]

### 2. [Segundo padrão]
[mesma estrutura]

## Pontos fortes
1. [Ponto forte, com exemplo]
2. [Ponto forte, com exemplo]

## Pontos a desenvolver
1. [Área]: [conselho específico e prático]
2. [Área]: [conselho específico e prático]

## Números
- Tempo de fala médio: [X% da reunião]
- Perguntas feitas: [média por reunião]
- Muletas: [por minuto]
- Interrupções: [feitas / sofridas por reunião]

## Próximos passos
[3 a 5 ações concretas]
```

Comece pelos pontos fortes reais, seja direto nos pontos a desenvolver e sugira trabalhar um de cada vez.

### 6. Ofereça continuidade
- Acompanhar os mesmos indicadores nas próximas reuniões.
- Aprofundar uma reunião ou um padrão específico.
- Montar um plano pessoal de desenvolvimento.
- Gerar um resumo com exemplos para a avaliação de desempenho.

---

## Exemplos

### Exemplo 1: evitar conflito (inspirado em Dan Shipper)

**Pedido:** "Coloquei as gravações das minhas reuniões numa pasta. Me mostre todas as vezes em que evitei conflito de forma sutil."

**Resposta:**
```markdown
# Padrões de evitar conflito

Encontrei 23 momentos em 15 reuniões em que você falou de forma indireta ou evitou uma tensão.

## Padrão: retorno crítico cheio de atenuações

Frequência: 8 vezes em 7 reuniões

Exemplo 1: conversa individual com a Sara, 00:14:32

O que aconteceu:
> "Então, eu tava pensando... talvez a gente pudesse, tipo, de repente
> dar uma olhada no cronograma de novo? Quer dizer, se você achar que
> faz sentido. Mas o que você achar melhor!"

Por que importa:
O projeto da Sara estava atrasado e você precisava dizer isso. As
atenuações ("talvez", "de repente", "se você achar") e a saída no fim
("o que você achar melhor") facilitaram que ela não percebesse a urgência.

Como poderia ser:
"Sara, o projeto está duas semanas atrasado. Quero entender hoje o que
está travando e montar um novo cronograma com você."

## Recomendações
1. Diga o problema na primeira frase.
2. Corte atenuações como "talvez", "meio que", "de repente".
3. Faça perguntas diretas em vez de dar indiretas.
4. Marque as conversas difíceis em vez de encaixá-las no meio de outro assunto.
```

### Exemplo 2: condução de reuniões de equipe
**Pedido:** "Analise minhas reuniões de equipe e me fale do meu jeito de conduzir."

A resposta mostra: quanto você fala em comparação com a equipe (ex.: 60% contra 40%), a proporção entre perguntas e afirmações, como lida com discordâncias (por exemplo, encerra rápido demais), quem fala menos e se você puxa essas pessoas para a conversa, e exemplos de boa condução e de oportunidades perdidas.

### Exemplo 3: evolução
**Pedido:** "Compare minhas reuniões do primeiro e do segundo trimestre para ver se melhorei na escuta."

A resposta mostra, por exemplo: interrupções caindo de 8 para 3 por reunião, perguntas de esclarecimento subindo de 2 para 7, mais construção em cima das ideias dos outros, exemplos lado a lado e o que ainda falta melhorar.

---

## Como conseguir as transcrições

- **Google Meet:** ative as transcrições (nos planos do Workspace que oferecem o recurso); o arquivo vai para o Google Drive. Baixe como .txt ou .docx.
- **Microsoft Teams:** ative a transcrição na reunião e baixe o arquivo (.docx ou .vtt) depois.
- **Zoom:** ative a gravação na nuvem com transcrição e baixe o arquivo .vtt.
- **Ferramentas de anotação** (tl;dv, Fireflies, Otter, Granola, Read e outras): exporte as transcrições em lote para uma pasta local.

Em todos os casos, os participantes precisam saber que a reunião está sendo gravada ou transcrita.

## Boas práticas

1. **Nomes padronizados:** `AAAA-MM-DD - Nome da reunião.txt`.
2. **Revisão periódica:** mensal ou trimestral, para ver tendências.
3. **Um comportamento por vez** nas perguntas, para ganhar profundidade.
4. **Privacidade:** transcrições em pasta local, apagadas quando não forem mais necessárias.
5. **Foco em ação:** trabalhe uma melhoria de cada vez.

## Perguntas comuns

- "Quando eu fujo de conversas difíceis?"
- "Com que frequência interrompo os outros?"
- "Quanto eu falo e quanto eu ouço?"
- "Faço boas perguntas?"
- "Como lido com discordância?"
- "Dou espaço para todo mundo falar?"
- "Uso muitas muletas de linguagem?"
- "Meus próximos passos ficam claros no fim da reunião?"
- "Sigo a pauta ou me perco?"
- "Como minha comunicação mudou ao longo do tempo?"

## Outros usos

- Montar um plano de desenvolvimento pessoal a partir da análise.
- Juntar exemplos para a própria avaliação de desempenho.
- Analisar as próprias ligações de vendas ou atendimento para melhorar a abordagem.
- Estudar as próprias negociações e seus resultados.

---

## Créditos e licença

Skill adaptada e traduzida para o português a partir da skill `meeting-insights-analyzer`, publicada em [ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) (pasta `meeting-insights-analyzer`). O README do repositório declara a licença **Apache 2.0** para o conteúdo do repositório; a pasta da skill não traz licença própria. Texto da licença: [apache.org/licenses/LICENSE-2.0](https://www.apache.org/licenses/LICENSE-2.0).

Alterações desta versão: tradução para o português do Brasil, com muletas e atenuações típicas do português ("tipo", "né", "então", "de repente", "meio que"); nova seção "Privacidade e uso ético" (ciência dos participantes e LGPD, foco na comunicação do próprio usuário, cuidados ao analisar outras pessoas); avisos sobre erros de transcrição automática e sobre o cálculo de tempo de fala sem marcação de tempo; regra para não inventar citações nem números; remoção da comparação com "benchmarks do setor", que não têm fonte confiável; atualização da seção de como obter transcrições (inclusão do Microsoft Teams, remoção da promoção do Granola); exemplo do diálogo reescrito em português.
