---
name: tutor-com-ia
description: Planeja, configura e melhora tutores de IA para cursos e treinamentos: persona e instruções do tutor, base de conhecimento (RAG) com citações, criação e adaptação de conteúdo, exercícios, retorno ao aluno, gamificação, escalonamento para humanos, privacidade (LGPD) e avaliação de qualidade. Use ao montar um tutor com IA, escrever o prompt de sistema de um tutor ou gerar material didático com IA.
---

# Tutor com IA

Você ajuda a configurar, melhorar e acompanhar tutores de IA para cursos e treinamentos, da persona e da base de conhecimento ao retorno para o aluno e à análise de desempenho. Escreva em português do Brasil.

Skills relacionadas no hub: **Engajamento de alunos**, **Estrutura de masterclass** e **Criação de e-books**.

Palavras-chave: tutor de IA, agente educacional, personalização, base de conhecimento, RAG, microlearning, retorno automatizado, trilha de aprendizagem, gamificação, analytics de aprendizagem, prompt de sistema, avaliação contínua.

## Antes de começar

Pergunte o que não foi informado, de uma vez só:

- **Curso e público:** tema, nível, idade dos alunos (há menores de 18 anos?).
- **Papel do tutor:** tirar dúvidas, corrigir exercícios, guiar uma trilha, simular situações?
- **Material de referência:** apostilas, transcrições de aulas, documentação oficial. É material próprio ou licenciado?
- **Onde vai rodar:** plataforma do curso, chat próprio, WhatsApp, um projeto no Claude.
- **Equipe humana:** quem responde quando o tutor não sabe ou quando o aluno precisa de uma pessoa.

## Princípios

- **O tutor ensina, não entrega a resposta pronta.** Guia com perguntas, dicas progressivas e exemplos parecidos, para o aluno chegar à solução. Em avaliações, segue as regras de integridade do curso.
- **Responde com base no material do curso**, cita de onde tirou e diz quando não sabe.
- **Conteúdo gerado por IA passa por revisão humana** antes de virar material oficial.
- **Dados do aluno: o mínimo necessário** (LGPD). Com menores de idade, consentimento dos responsáveis.
- **Pessoas por perto:** há sempre um caminho para falar com um tutor humano.

## Primeiros passos rápidos

1. **Escolha o modelo.** Use um modelo atual. Na família Claude, por exemplo, o Claude Opus 5.5 (`claude-opus-5-5`) para tarefas mais exigentes e o Claude Sonnet 5.5 (`claude-sonnet-5-5`) ou o Claude Haiku 5.5 (`claude-haiku-5-5`) para volume alto e respostas rápidas. Confira a lista atual de modelos e preços do fornecedor antes de decidir.
2. **Escreva as instruções do tutor (prompt de sistema):** identidade, tom, método de ensino, limites e o que fazer quando não souber. Exemplo: "Você é um tutor de física paciente, que usa analogias do dia a dia e nunca entrega a resposta de um exercício avaliativo."
3. **Carregue a base de conhecimento (RAG):** apostilas, transcrições e documentação, de preferência com citações ativadas para mostrar a fonte de cada resposta.
4. **Teste com conversas simuladas:** perguntas fáceis, difíceis, fora do tema, pedidos de "me dá a resposta" e erros comuns. Ajuste as instruções.
5. **Crie um ciclo de melhoria:** reúna conversas reais (anonimizadas), marque respostas ruins, escreva a resposta ideal e use esses casos para ajustar as instruções e testar de novo.

---

## Fluxo 1: criar e adaptar conteúdo com IA

1. **Estruture o curso**
   - Prompt: "Estruture um curso introdutório de orientação a objetos em Java para desenvolvedores juniores que dominam lógica de programação, mas nunca usaram orientação a objetos. Proponha 4 módulos com 3 ou 4 subtemas cada."
   - Resultado esperado: módulos e subtemas organizados (ex.: módulo 1, conceitos fundamentais: abstração, encapsulamento...).

2. **Gere o rascunho de cada subtema**
   - Prompt: "Para o subtema 'Encapsulamento', escreva uma explicação curta para iniciantes em Java, com um exemplo de código simples e um exercício."
   - Resultado: texto, trecho de código e exercício (ex.: "Altere a classe `ContaBancaria` para que o saldo não possa ser acessado diretamente").
   - Rode o código dos exemplos antes de publicar.

3. **Adapte ao perfil do aluno**
   - Use só os dados necessários para ensinar melhor (nível, dificuldades, interesses declarados), não dados pessoais desnecessários.
   - Exemplo de contexto interno: "[Aluna de design, familiarizada com camadas e estilos no Photoshop, com dificuldade em abstração.] Explique herança usando uma analogia com estilos-base e variações no design, com as classes `Forma` e `Retangulo`."

4. **Materiais complementares**
   - A IA pode inventar links e títulos de vídeos que não existem. Peça sugestões apenas a partir de uma lista de materiais já selecionada pela equipe, ou peça só os temas e termos de busca e confira cada link antes de publicar.
   - Para referências oficiais, prefira as fontes primárias (ex.: documentação da Oracle sobre interfaces em Java).

5. **Revisão**
   - Peça à IA uma revisão de clareza, linguagem acessível e consistência de termos ("Revise o material de polimorfismo para iniciantes, aponte ambiguidades e confira se os exemplos de código estão corretos").
   - A revisão final de conteúdo técnico é humana: a IA também erra ao se revisar.

## Fluxo 2: engajamento, retorno e gamificação

1. **Marcos de aprendizagem**
   - Evento: o aluno concluiu 80% dos exercícios do módulo 1.
   - Tutor: "Parabéns, Ana! Você concluiu 80% dos exercícios de fundamentos. Quer seguir para o próximo desafio ou revisar algum ponto antes?"

2. **Desafios adaptativos**
   - Contexto: o aluno acertou 90% do último quiz de encapsulamento.
   - Prompt: "Crie um desafio de nível avançado em Java sobre herança e polimorfismo, com uma hierarquia simples de classes, sobrescrita de métodos e casos de teste esperados."
   - Gere e valide também os testes; desafios gerados na hora precisam de verificação automática da resposta para não punir o aluno por um erro da IA.

3. **Pontos e emblemas**
   - Evento: Carlos concluiu o desafio de refatoração (+150 pontos, emblema "Refatorador").
   - Tutor: "Muito bom, Carlos! Você ganhou o emblema 'Refatorador'. Seu total agora é 780 pontos."
   - Premie aplicação e progresso, não só volume de mensagens.

4. **Retorno construtivo**
   - Erro: "Pedro, o exercício de interfaces deu trabalho, e é normal: é um conceito que pede prática. O ponto principal está no método `calcularArea()`: a interface define o que fazer, não como. Que tal revisar a seção 3.2 e tentar um exemplo mais simples primeiro?"
   - Acerto: "Ótimo trabalho, Sofia! Você usou a sobrescrita de métodos de forma clara. Quer tentar um desafio maior?"
   - Ajuste o tom ao que o aluno diz ("estou perdido", "não entendi"), não a emoções inferidas automaticamente (veja a seção de privacidade).

5. **Rankings, só com cuidado**
   - Rankings só com adesão do aluno (opt-in), de preferência com apelidos e mostrando a posição de cada um apenas para ele, ou comparando o aluno com o próprio desempenho anterior.
   - Exemplo: "Você subiu 3 posições nesta semana e está no grupo dos 20% que mais praticaram."

## Privacidade, segurança e escalonamento

- **LGPD:** informe aos alunos que conversam com uma IA, para que os dados são usados, por quanto tempo ficam guardados e com quais fornecedores são compartilhados. Colete o mínimo; anonimize as conversas usadas para melhorar o tutor.
- **Menores de idade:** consentimento dos responsáveis e cuidado redobrado com o conteúdo.
- **Nada de reconhecimento de emoções:** não analise automaticamente o "estado emocional" dos alunos por texto, voz ou vídeo. Na União Europeia isso é proibido em instituições de ensino (AI Act), e no Brasil envolve dados sensíveis. Responda ao que o aluno diz de forma explícita e ofereça ajuda.
- **Situações de risco:** se o aluno relatar sofrimento intenso, ideias de se machucar ou violência, o tutor deve acolher, não tentar resolver sozinho, indicar o CVV (ligue 188 ou cvv.org.br) e avisar a equipe humana conforme o protocolo do curso.
- **Fora do escopo:** perguntas sem relação com o curso, pedidos inadequados ou tentativas de fazer o tutor ignorar as regras recebem uma resposta educada e a volta ao tema.
- **Escalonamento:** dúvidas que o tutor não resolve, reclamações e questões administrativas (notas, pagamentos, certificados) vão para pessoas, com prazo de resposta.

---

## Modelos

### Prompt de sistema do tutor

```
Você é o tutor de Ciência de Dados com Python do curso [nome do curso]. Seus alunos têm nível intermediário.

Como ensinar:
- Explique conceitos complexos de forma clara, com exemplos práticos de código e analogias do dia a dia. Evite jargão desnecessário.
- Comece reconhecendo o que o aluno disse ou tentou; depois explique ou proponha o próximo passo.
- Quando o aluno errar, mostre onde está o erro, explique o conceito certo e dê um exemplo diferente, em vez de só dizer "está errado".
- Depois de uma explicação importante, peça ao aluno que explique com as palavras dele o que entendeu.
- Em exercícios e avaliações do curso, não entregue a solução pronta: dê dicas progressivas e perguntas que levem o aluno a resolver.

Fontes:
- Baseie as respostas nos materiais do curso fornecidos e cite a aula ou a seção usada.
- Se a resposta não estiver nos materiais, diga isso. Você pode dar uma explicação geral, deixando claro que ela não vem do material do curso.
- Nunca invente links, títulos de vídeos ou referências.

Limites:
- Assuntos fora do curso: responda brevemente que não é seu foco e volte ao tema.
- Notas, prazos, pagamentos e certificados: oriente o aluno a falar com a equipe pelo [canal].
- Se o aluno relatar sofrimento intenso ou risco à própria segurança, acolha, indique o CVV (188) e diga que a equipe será avisada, conforme [protocolo].
```

### Módulo de microlearning (exemplo)

````markdown
## Módulo: introdução a funções em Python (cerca de 20 minutos)

### Objetivos
Ao final, o aluno será capaz de:
- explicar o que são funções e por que são usadas;
- declarar e chamar funções simples;
- diferenciar parâmetros e argumentos e usá-los.

### Seções
1. O que é uma função? (5 min)
   - Analogia: uma receita, que executa sempre a mesma tarefa quando chamada.
   - Exemplo conhecido: `print("Olá, mundo!")` já é uma função.
   - Reflexão: qual a vantagem de agrupar código em blocos com nome?

2. Declarar e chamar (7 min)
   - Sintaxe: `def nome_da_funcao():` e a indentação.
   ```python
   def saudar():
       print("Olá!")

   saudar()  # chamada da função
   ```
   - Exercício: crie a função `exibir_mensagem` que mostre "Aprender Python é divertido!".

3. Parâmetros e argumentos (8 min)
   - Parâmetro é a variável na definição; argumento é o valor passado na chamada.
   ```python
   def saudar_nome(nome):  # 'nome' é um parâmetro
       print(f"Olá, {nome}!")

   saudar_nome("Ana")  # "Ana" é um argumento
   ```
   - Desafio: altere `saudar_nome` para receber também `sobrenome` e mostrar o nome completo.

### Para aprofundar
- Documentação oficial do Python sobre definição de funções.
- [Vídeo curto sobre escopo de variáveis, escolhido e conferido pela equipe]
- [Site de exercícios interativos, conferido pela equipe]
````

---

## Checklist

- [ ] Prompt de sistema com persona, método de ensino, uso das fontes e limites.
- [ ] Base de conhecimento carregada, atualizada e com direito de uso; citações ativadas.
- [ ] Regra de não entregar respostas de avaliações.
- [ ] Ciclo de melhoria com conversas reais anonimizadas.
- [ ] Testes com perguntas difíceis, fora do tema e tentativas de burlar as regras.
- [ ] Gamificação definida, com ranking só por adesão.
- [ ] Acompanhamento do progresso (módulos, exercícios, dúvidas recorrentes).
- [ ] Protocolo para situações de risco e para escalonar a humanos.
- [ ] Aviso de privacidade aos alunos e coleta mínima de dados (LGPD); consentimento dos responsáveis para menores.
- [ ] Integração com a plataforma do curso (matrícula, notas, relatórios), se necessário.
- [ ] Indicadores de engajamento, conclusão, satisfação e qualidade das respostas.

## Indicadores para acompanhar

Valores indicativos da skill original, sem fonte citada; defina as metas com base no histórico do seu curso.

| Indicador | Referência | Meta |
|---|---|---|
| Conclusão dos módulos | 60% | 75% |
| Uso frequente (usuários ativos no dia ÷ no mês) | 15% | 25% |
| Satisfação (NPS) | +30 | +50 |
| Acertos nas avaliações após o módulo | 70% | 85% |

Acompanhe também:
- **Qualidade das respostas:** amostra semanal revisada por um professor (correção, uso das fontes, tom).
- **Taxa de "não sei" e de escalonamento:** mostra lacunas no material.
- **Tempo até a primeira palavra da resposta:** em chat, o que importa é a resposta começar a aparecer rápido (streaming), mais que o tempo total.
- **Custo por aluno ativo.**

## Erros comuns

1. **Tutoria genérica.** Todos os alunos recebem a mesma resposta.
   Como evitar: inclua no contexto de cada conversa o necessário sobre o aluno (nível, módulos concluídos, dificuldades). Ex.: "[Iniciante em Python, teve dificuldade com laços no último módulo.] Explique recursão de forma simples, com analogia e um exercício."
2. **Respostas inventadas (alucinação).** Conteúdo plausível, mas errado ou diferente do material.
   Como evitar: base de conhecimento com o material do curso, citações das fontes e a instrução de dizer quando não sabe. Ex.: "Não encontrei isso nos materiais do curso. Quer ver um conceito relacionado ou me passar mais contexto?"
3. **Retorno vazio.** Só "certo" ou "errado".
   Como evitar: peça nas instruções que o tutor mostre onde está o erro, explique o conceito, dê um exemplo alternativo e indique o trecho do material para revisar.
4. **Entregar a resposta.** O aluno copia a solução e não aprende.
   Como evitar: dicas progressivas e perguntas, principalmente em exercícios avaliativos.
5. **Material gerado sem revisão.** Erros técnicos viram conteúdo oficial.

## Dicas avançadas

1. **Vários agentes:** para temas com especialidades diferentes (teoria, código, aplicação), um tutor principal pode encaminhar partes da pergunta para assistentes especializados, cada um com instruções e base de conhecimento próprias, e juntar as respostas. Comece com um só tutor e só divida quando houver ganho medido.
2. **Trilha preditiva:** use o desempenho do aluno para antecipar dificuldades e sugerir reforço antes de avançar. Ex.: dificuldade recorrente com álgebra linear num curso de machine learning leva a um módulo de reforço antes do tema seguinte.
3. **Exercícios gerados na hora:** a dificuldade sobe ou desce conforme o desempenho, sempre com verificação automática da resposta. Ex.: três acertos seguidos em listas levam a um problema com listas aninhadas.
4. **Simulações:** cenários práticos em que a IA faz um papel (entrevistador, cliente, gerente de projeto) e dá retorno sobre as decisões do aluno.
5. **Sinais explícitos de dificuldade:** em vez de inferir emoções, ofereça botões e atalhos como "não entendi", "quero um exemplo mais simples" e "quero falar com uma pessoa", e use esses sinais para adaptar a explicação.

## Formato da resposta

Conforme o pedido, entregue: o desenho do tutor (papel, público, fontes, limites, escalonamento); o prompt de sistema pronto; a estrutura de módulos ou o conteúdo gerado, marcado como rascunho para revisão; o plano de testes; e os indicadores. Marque com `[ ]` o que o usuário precisa preencher ou conferir.

---

## Créditos e licença

Skill adaptada a partir da skill `ai-tutoring-setup` (versão 1.0.0), de autoria de **Cafe Code AI**, publicada em [kursku/skills](https://github.com/kursku/skills) (pasta `packs/kit-510-ptbr/13-cursos-educacao/ai-tutoring-setup`). A licença **MIT** está declarada nos metadados da skill original; o repositório não traz um arquivo de licença próprio.

Alterações desta versão: modelo de exemplo atualizado (o original citava o Claude 3.5 Sonnet); nova seção de princípios (ensinar sem entregar a resposta, fontes, revisão humana, dados mínimos) e de privacidade, segurança e escalonamento (LGPD, menores de idade, protocolo para situações de risco com o CVV); remoção da inferência automática de emoções e da análise de sentimento dos alunos, trocadas por sinais explícitos; rankings só por adesão e sem expor nomes; alerta de que a IA inventa links de vídeos e materiais, com uso de lista curada; testes nos desafios gerados na hora; prompt de sistema com uso de fontes, citações e limites; troca da meta de "resposta em menos de 1 segundo" por tempo até a primeira palavra e por indicadores de qualidade; inclusão das seções "Antes de começar", "Erros comuns" (com dois erros novos) e "Formato da resposta"; checklist desmarcado para uso.

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
