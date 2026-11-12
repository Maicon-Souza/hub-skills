---
name: codigo-limpo
description: Padrões pragmáticos de código para escrever e editar código de forma direta e enxuta, sem complicação desnecessária nem comentários óbvios, conferindo dependências antes de editar e validando antes de concluir. Use ao escrever, refatorar ou revisar código.
---

# Código limpo: padrões pragmáticos

Seja **conciso, direto e focado na solução**. Explique em português do Brasil; o código segue a língua e as convenções do projeto (nomes em inglês, se o projeto usa inglês).

## Princípios

| Princípio | Regra |
|---|---|
| **SRP** | Responsabilidade única: cada função ou classe faz uma coisa |
| **DRY** | Não se repita: extraia duplicações reais e reaproveite |
| **KISS** | Mantenha simples: a solução mais simples que funciona |
| **YAGNI** | Você não vai precisar: não construa o que ninguém pediu |
| **Escoteiro** | Deixe o código um pouco melhor do que encontrou, dentro do escopo da tarefa |

O DRY não justifica abstrair duas coisas parecidas por acaso: espere a terceira repetição ou uma razão real para mudarem juntas.

## Nomes

| Elemento | Convenção |
|---|---|
| **Variáveis** | Revelam a intenção: `userCount`, não `n` |
| **Funções** | Verbo + substantivo: `getUserById()`, não `user()` |
| **Booleanos** | Em forma de pergunta: `isActive`, `hasPermission`, `canEdit` |
| **Constantes** | Maiúsculas com sublinhado: `MAX_RETRY_COUNT` |

Siga primeiro o padrão da linguagem e do projeto (ex.: `snake_case` em Python, `PascalCase` para classes).

> **Regra:** se precisa de um comentário para explicar um nome, troque o nome.

## Funções

| Regra | Descrição |
|---|---|
| **Pequenas** | Em geral até 20 linhas, de preferência 5 a 10 |
| **Uma coisa** | Fazem uma coisa e fazem bem |
| **Um nível** | Um nível de abstração por função |
| **Poucos argumentos** | Até 3, de preferência 0 a 2; acima disso, um objeto de opções |
| **Sem efeitos colaterais escondidos** | Não alteram os argumentos recebidos de forma inesperada |

Os números são referência, não lei: uma função de 30 linhas clara é melhor que cinco de 6 linhas espalhadas sem motivo.

## Estrutura

| Padrão | Como aplicar |
|---|---|
| **Cláusulas de guarda** | Retorno antecipado para casos de borda |
| **Plano em vez de aninhado** | Evite aninhamento profundo (até 2 níveis) |
| **Composição** | Funções pequenas combinadas |
| **Proximidade** | Código relacionado perto um do outro |

## Como agir

| Situação | Ação |
|---|---|
| Pedido de funcionalidade | Escreva diretamente, seguindo o padrão do projeto |
| Relato de bug | Encontre a causa, corrija e diga em uma ou duas frases o que era |
| Requisito pouco claro | Pergunte, não suponha |
| Mudança grande ou arriscada | Diga o plano em poucas linhas antes de mexer |

## O que evitar

| ❌ Evite | ✅ Faça |
|---|---|
| Comentar cada linha | Apague comentários óbvios; comente o porquê, não o quê |
| Função auxiliar para uma linha | Escreva a linha no lugar |
| Fábrica para 2 objetos | Crie os objetos diretamente |
| `utils.ts` com uma função | Coloque o código onde é usado |
| "Primeiro importamos..." | Só escreva o código |
| Aninhamento profundo | Cláusulas de guarda |
| Números mágicos | Constantes com nome |
| Funções gigantes | Divida por responsabilidade |
| Refatorar o que não foi pedido | Sugira à parte; mantenha a mudança focada |

## Antes de editar qualquer arquivo

Pergunte-se:

| Pergunta | Por quê |
|---|---|
| **Quem importa este arquivo?** | Pode quebrar |
| **O que este arquivo importa?** | A interface pode ter mudado |
| **Que testes cobrem isto?** | Podem falhar |
| **É um componente compartilhado?** | Vários lugares são afetados |

Exemplo:
```
Arquivo a editar: UserService.ts
└── Quem importa? → UserController.ts, AuthController.ts
└── Precisam mudar também? → Confira as assinaturas das funções
```

> **Regra:** edite o arquivo e todos os que dependem dele na mesma tarefa. Nunca deixe imports quebrados ou chamadas desatualizadas.

## Resumo

| Faça | Não faça |
|---|---|
| Escreva o código direto | Escreva tutoriais |
| Deixe o código se explicar | Comentários óbvios |
| Corrija bugs e diga a causa em uma frase | Longas explicações antes da correção |
| Mantenha pequenas coisas no lugar | Crie arquivos desnecessários |
| Nomes claros | Abreviações |
| Funções pequenas | Funções de 100+ linhas |

> **Lembre:** o usuário quer código funcionando, não uma aula de programação.

## Antes de dizer que terminou

| Verificação | Pergunta |
|---|---|
| **Objetivo** | Fiz exatamente o que foi pedido? |
| **Arquivos** | Alterei todos os arquivos necessários? |
| **Funciona** | Testei ou verifiquei a mudança? |
| **Sem erros** | Lint, checagem de tipos e testes passam? |
| **Nada esquecido** | Algum caso de borda ficou de fora? |

> **Regra:** se alguma verificação falhar, corrija antes de concluir. Se não deu para verificar algo (ex.: não há testes), diga isso claramente.

## Validação

Use as ferramentas do próprio projeto. Procure os comandos em `package.json`, `Makefile`, `pyproject.toml`, `README` ou na configuração de CI. Exemplos comuns:

| Tipo de projeto | Lint | Tipos | Testes |
|---|---|---|---|
| JavaScript/TypeScript | `npm run lint` | `npx tsc --noEmit` | `npm test` |
| Python | `ruff check .` | `mypy .` | `pytest` |
| Go | `go vet ./...` | (compilação) | `go test ./...` |

Rode só o que é relevante para a mudança e não instale ferramentas novas sem perguntar.

### O que fazer com o resultado

1. **Rode** e leia a saída inteira.
2. **Separe** erros, avisos e o que passou.
3. **Corrija os erros causados pela sua mudança** e rode de novo.
4. **Para erros que já existiam antes** ou fora do escopo da tarefa, resuma e pergunte antes de mexer:

```markdown
## Resultado: [comando]

Erros (X)
- [arquivo:linha] descrição

Avisos (Y)
- [arquivo:linha] descrição

Corrijo os X erros que já existiam?
```

> Rodar a validação e ignorar a saída é tarefa não concluída.

---

## Créditos e licença

Skill adaptada e traduzida para o português a partir da skill `clean-code` (versão 2.0) do projeto [vudovn/antigravity-kit](https://github.com/vudovn/antigravity-kit), de **VUDOVN**, sob a licença **MIT** (Copyright (c) 2026 VUDOVN). Encontrada na coletânea GovHub-br/gov-hub-skills (pasta `05-qualidade-testes/clean-code`), que não registra a origem; a mesma skill aparece em várias outras coletâneas.

Alterações desta versão: tradução para o português do Brasil; remoção da tabela de "scripts de verificação obrigatórios", que chamava scripts de outros agentes e skills que não acompanham esta versão, trocada por uma seção que usa as ferramentas de lint, tipos e testes do próprio projeto; remoção da restrição de ferramentas (`allowed-tools`), que impedia rodar essas validações, e do campo não padrão `priority`; regra de resultado ajustada (erros causados pela própria mudança são corrigidos direto; os que já existiam são perguntados antes); bugs corrigidos com uma frase sobre a causa, em vez de nenhuma explicação; limites de tamanho de função e de argumentos tratados como referência; cautela com DRY prematuro e com refatorações fora do escopo; respeito às convenções de nomes de cada linguagem.

Texto da licença original:

```
MIT License

Copyright (c) 2026 VUDOVN

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
