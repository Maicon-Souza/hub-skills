<div align="center">

# 🧠 Hub de Skills

**Skills prontas para usar no Claude. Escolha, baixe e instale.**

[![Site](https://img.shields.io/badge/site-hub--skills-D97757?style=for-the-badge)](https://maicon-souza.github.io/hub-skills/)
![Skills](https://img.shields.io/badge/skills-39-2B2B2B?style=for-the-badge)
![Áreas](https://img.shields.io/badge/áreas-10-2B2B2B?style=for-the-badge)
![Idioma](https://img.shields.io/badge/idioma-pt--BR-2B2B2B?style=for-the-badge)

### [👉 Abrir o hub](https://maicon-souza.github.io/hub-skills/)

</div>

---

## O que é uma skill?

Uma skill é um pacote de instruções que o Claude carrega quando a tarefa pede. Em vez de explicar o mesmo processo toda vez, você instala a skill uma vez e o Claude passa a seguir aquele método sempre que você pedir algo relacionado: revisar um código, montar uma cadência de follow-up, investigar um phishing.

Todas as skills deste hub foram escritas em português do Brasil, com foco em uso prático no dia a dia.

## 🚀 Como instalar

### No Claude (web ou app)

Funciona no plano gratuito.

1. Baixe o `.zip` da skill pelo [site](https://maicon-souza.github.io/hub-skills/) ou pela pasta [`downloads/`](downloads). Não precisa descompactar.
2. No Claude, abra **Configurações** → **Recursos** e ative **Execução de código e criação de arquivos**. Só precisa fazer isso uma vez.
3. Vá em **Personalizar** → **Skills**.
4. Clique em **+** → **Criar skill** → **Enviar uma skill** e escolha o `.zip`.
5. Ative a skill e peça algo relacionado a ela no chat.

> [!TIP]
> Se a opção de skills não aparecer ou estiver cinza, confira o passo 2. Em contas Team ou Enterprise, o administrador precisa ter liberado as skills.

### No Claude Code

Descompacte o `.zip` e copie a pasta para `~/.claude/skills/`:

```bash
unzip codigo-limpo.zip -d ~/.claude/skills/
```

## 📚 Catálogo

<details open>
<summary><strong>🧑‍🏫 Skill da aula</strong> · 1 skill</summary>

| Skill | O que faz |
|---|---|
| [Analista de leads](skills/analista-de-leads) | Lê uma planilha de leads no Google Drive e entrega ranking com nota 0–100, o Top 3 do dia com mensagens de WhatsApp e insights do funil. |

</details>

<details open>
<summary><strong>💻 Tecnologia</strong> · 7 skills</summary>

| Skill | O que faz |
|---|---|
| [Código limpo](skills/codigo-limpo) | Padrões pragmáticos para escrever e editar código: nomes claros, funções pequenas, sem complicação nem comentário óbvio. |
| [Revisão de código com IA](skills/revisao-de-codigo-com-ia) | Revisa diffs e PRs com foco em correção, segurança, desempenho e testes, e monta revisões automáticas com a API do Claude. |
| [Documentação de API](skills/documentacao-de-api) | Gera documentação de APIs REST, GraphQL e WebSocket a partir do código, com exemplos e especificação OpenAPI. |
| [Design de tabelas no PostgreSQL](skills/design-de-tabelas-postgres) | Tipos de dados, chaves, índices, particionamento, JSONB, RLS e evolução segura do schema. |
| [Especialista em Docker](skills/especialista-em-docker) | Dockerfiles e Compose otimizados: multi-stage, imagens menores, segurança, healthchecks e diagnóstico. |
| [Dashboards do Grafana](skills/dashboards-grafana) | Dashboards para aplicações, infra, bancos e SLOs, com PromQL, variáveis, alertas e provisionamento como código. |
| [Workflows no n8n](skills/workflows-n8n) | Projeta e depura automações: webhooks, HTTP Request, nó Code, expressões, erros e sub-workflows. |

</details>

<details open>
<summary><strong>🔐 Segurança da Informação</strong> · 4 skills</summary>

| Skill | O que faz |
|---|---|
| [Investigação de phishing](skills/investigacao-de-phishing) | Análise de cabeçalhos, URLs e anexos, quem recebeu e clicou, expurgo, bloqueios e relatório final. |
| [Playbook de resposta a incidentes](skills/playbook-de-resposta-a-incidentes) | Playbooks por tipo de ataque com árvores de decisão, RACI e automação em SOAR, alinhados ao NIST 800-61. |
| [Implementação da ISO 27001](skills/implementacao-iso-27001) | Implantação de SGSI pela ISO 27001:2022, do escopo à Declaração de Aplicabilidade e certificação. |
| [Auditoria de buckets S3](skills/auditoria-de-buckets-s3) | Encontra acesso público, políticas permissivas e falta de criptografia, com AWS CLI, Prowler e remediação. |

</details>

<details open>
<summary><strong>📣 Marketing</strong> · 8 skills</summary>

| Skill | O que faz |
|---|---|
| [Copywriting](skills/copywriting) | Textos de sites, landing pages e anúncios: títulos, CTAs e proposta de valor, sem cara de IA. |
| [Humanizador de texto](skills/humanizador-de-texto) | Reescreve textos com cara de IA para soarem humanos, sem mudar o conteúdo. |
| [Otimização de perfil do LinkedIn](skills/otimizacao-de-perfil-linkedin) | Audita e reescreve o perfil seção por seção, do título às recomendações. |
| [Redação de posts do LinkedIn](skills/redacao-de-posts-linkedin) | Posts com 20 fórmulas de gancho escolhidas pelo objetivo e filtro contra texto artificial. |
| [Plano de Reels](skills/plano-de-reels) | Planeja e roteiriza vídeos curtos para Reels, TikTok e Shorts, com ganchos, legendas e métricas. |
| [Estratégia para TikTok](skills/estrategia-tiktok) | Tendências, ganchos, busca no app, lives, duetos, monetização e análise de desempenho. |
| [Táticas de engajamento](skills/taticas-de-engajamento) | Mais comentários, compartilhamentos e participação nas redes, sem iscas. |
| [Playbook de Black Friday](skills/playbook-black-friday) | Campanha de e-mail completa: aquecimento, lista VIP, oferta, carrinho abandonado e pós-venda. |

</details>

<details open>
<summary><strong>📈 Growth</strong> · 5 skills</summary>

| Skill | O que faz |
|---|---|
| [Redução de churn](skills/reducao-de-churn) | Health score, playbooks de CS, reengajamento, pesquisa de saída, win-back, NRR e LTV. |
| [SEO local](skills/seo-local) | SEO para negócios físicos: perfil no Google, NAP consistente, citações, links e páginas locais. |
| [Perfil da Empresa no Google](skills/perfil-da-empresa-no-google) | Otimiza o antigo Google Meu Negócio: categorias, fotos, postagens, avaliações e respostas. |
| [Estratégia de indexação](skills/estrategia-de-indexacao) | robots.txt, sitemaps, canonical, redirecionamentos, hreflang e Search Console. |
| [Visualização de dados com GA4](skills/visualizacao-de-dados-ga4) | Dashboards do GA4 no Looker Studio e BigQuery: funis, atribuição, engajamento e alertas. |

</details>

<details open>
<summary><strong>💼 Vendas</strong> · 3 skills</summary>

| Skill | O que faz |
|---|---|
| [Roteiro de ligação de vendas](skills/roteiro-de-ligacao-de-vendas) | Prospecção, descoberta (SPIN, BANT), objeções e fechamento, sem promessas sem base. |
| [Sistema de follow-up](skills/sistema-de-follow-up) | Cadências multicanal com gatilhos, textos prontos e nutrição de leads, respeitando a LGPD. |
| [Estratégia de precificação](skills/estrategia-de-precificacao) | Custos, impostos, margem, ponto de equilíbrio, valor percebido, planos e objeção de preço. |

</details>

<details open>
<summary><strong>🤝 Consultoria</strong> · 3 skills</summary>

| Skill | O que faz |
|---|---|
| [Comunicação com clientes](skills/comunicacao-com-clientes) | Boas-vindas, kick-off, atas, relatórios, mudanças de escopo, atrasos e gestão de crise. |
| [Health score de clientes](skills/health-score-de-clientes) | Indicadores e pesos, faixas de risco, alertas e planos de recuperação para clientes B2B. |
| [Análise de reuniões](skills/analise-de-reunioes) | Lê transcrições e mostra seus padrões de comunicação, com exemplos e sugestões. |

</details>

<details open>
<summary><strong>🎓 Educação</strong> · 5 skills</summary>

| Skill | O que faz |
|---|---|
| [Estrutura de masterclass](skills/estrutura-de-masterclass) | Promessa, módulos, roteiros de aula, atividades e projeto final com rubrica. |
| [Tutor com IA](skills/tutor-com-ia) | Configura tutores de IA para cursos: prompt, base de conhecimento, exercícios e LGPD. |
| [Engajamento de alunos](skills/engajamento-de-alunos) | Retenção e conclusão com análise de abandono, gamificação e microlearning. |
| [Criação de e-books](skills/criacao-de-ebooks) | Do tema ao sumário, capítulos, diagramação e página de captura. |
| [Podcast para cursos](skills/podcast-para-cursos) | Roteiros para o ouvido, estudos de caso, atividades pós-escuta e acessibilidade. |

</details>

<details open>
<summary><strong>🎨 Design</strong> · 2 skills</summary>

| Skill | O que faz |
|---|---|
| [Crítica de design](skills/critica-de-design) | Feedback estruturado sobre usabilidade, hierarquia, consistência e acessibilidade. |
| [Design em tela](skills/design-em-tela) | Pôsteres e peças visuais em PNG ou PDF, com 40 fontes livres incluídas. |

</details>

<details open>
<summary><strong>🥗 Saúde</strong> · 1 skill</summary>

| Skill | O que faz |
|---|---|
| [Analisador nutricional](skills/analisador-nutricional) | Analisa diários alimentares e compara com as recomendações, com limites claros e encaminhamento profissional. |

</details>

## 🗂️ Estrutura do repositório

```
hub-skills/
├── index.html        # o site do hub (busca e filtro por área)
├── skills.json       # catálogo que alimenta o site
├── skills/           # código-fonte de cada skill (SKILL.md + arquivos de apoio)
│   └── <nome-da-skill>/
│       └── SKILL.md
└── downloads/        # .zip prontos para enviar ao Claude
```



---
