---
name: kit-de-pesquisa-biomedica
description: Consulta mais de 15 bases biomédicas públicas (PubMed, ClinicalTrials.gov, ChEMBL, Open Targets, OpenFDA, OMIM, Reactome, KEGG, UniProt, GWAS Catalog e outras) por um único servidor MCP, para reposicionamento de medicamentos, descoberta de alvos terapêuticos, revisão de evidências clínicas e de literatura e checagem de segurança de medicamentos. Use em pesquisa sobre doenças, genes, alvos, medicamentos aprovados ou em estudo, ensaios clínicos e eventos adversos. É para pesquisa, não para orientar o tratamento de pacientes.
---

# Kit de pesquisa biomédica

Você usa um servidor MCP público que reúne mais de 15 bases de dados biomédicas para apoiar pesquisa: reposicionamento de medicamentos, descoberta de alvos, revisão de evidências e de literatura. Responda em português do Brasil, mantendo os nomes técnicos das bases e dos identificadores.

## Uso responsável

- **Pesquisa, não conduta clínica.** Os resultados apoiam hipóteses e revisões. Não use para indicar, trocar ou suspender tratamento de um paciente; isso é decisão médica, com as bulas e diretrizes oficiais.
- **Nenhum dado de paciente.** O servidor é de terceiros e público. Nunca envie nome, CPF, prontuário, exames ou qualquer dado que identifique alguém (são dados sensíveis pela LGPD). Pesquise por doença, gene, medicamento ou identificador público.
- **Contexto regulatório.** "Aprovado" nas bases (ChEMBL, OpenFDA) se refere em geral ao FDA, dos EUA. Para o Brasil, confira o registro e a bula na Anvisa.
- **Evidência tem níveis.** Associação genética, atividade em laboratório, relato de caso e ensaio clínico randomizado têm pesos muito diferentes. Deixe claro o tipo de evidência de cada achado.
- **Cite as fontes** (PMID, NCT, IDs do ChEMBL, Open Targets, OMIM) para que tudo possa ser conferido.

## Como acessar

**No Claude Code, como servidor MCP (recomendado):**

```bash
claude mcp add --transport http pesquisa-biomedica https://mcp.cloud.curiloo.com/tools/unified/mcp
```

As ferramentas passam a aparecer com o prefixo do servidor. A lista atualizada de ferramentas e parâmetros vem do próprio servidor; confira antes de montar consultas complexas.

**Por HTTP direto (JSON-RPC):**

```bash
curl -X POST https://mcp.cloud.curiloo.com/tools/unified/mcp \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","method":"tools/list","id":1}'
```

**Rodando localmente** (código aberto, licença MIT):

```bash
pip install medical-mcps
medical-mcps
# disponível em http://localhost:8000/tools/unified/mcp
```

O servidor público é mantido por terceiros e pode mudar ou sair do ar. Para uso contínuo ou institucional, prefira rodar localmente. Se o servidor não responder, avise o usuário em vez de inventar resultados.

## Bases disponíveis

| Base | Conteúdo | Para quê |
|---|---|---|
| ChEMBL | Milhões de compostos com dados de bioatividade, mecanismos e indicações | Medicamentos aprovados e em estudo, alvos, atividade |
| Open Targets | Associações doença-alvo com pontuação de evidência (0 a 1) | Identificar alvos terapêuticos |
| PubMed (e preprints) | Dezenas de milhões de artigos; preprints do bioRxiv e medRxiv | Revisão de literatura |
| ClinicalTrials.gov | Centenas de milhares de estudos registrados | Ensaios em andamento e concluídos |
| NCI Clinical Trials | Ensaios em câncer | Oncologia |
| OpenFDA | Eventos adversos (FAERS), bulas americanas, dispositivos | Segurança de medicamentos |
| FDA Orphan / Orange Book / USPTO | Designações órfãs, exclusividades e patentes | Viabilidade de reposicionamento |
| OMIM | Doenças genéticas, genes e fenótipos (exige chave) | Base genética de doenças monogênicas |
| Reactome, KEGG, Pathway Commons | Vias biológicas e interações | Mecanismos |
| UniProt | Proteínas, sequências, anotações | Propriedades de proteínas |
| GWAS Catalog | Associações genéticas de estudos de genoma completo | Variantes associadas a doenças |
| MyGene, MyVariant, MyChem, MyDisease | Anotações de genes, variantes, compostos e doenças | Consulta e cruzamento de dados |
| Node Normalization | Equivalência de identificadores entre bases | Converter IDs |
| Every Cure KG | Grafo de conhecimento de medicamentos e doenças | Hipóteses de reposicionamento |

### Ferramentas principais (prefixo por base)

- **ChEMBL:** `chembl_find_drugs_by_indication`, `chembl_find_drugs_by_target`, `chembl_get_mechanism`, `chembl_get_molecule`, `chembl_get_activities`, `chembl_get_drug_indications`, `chembl_search_molecules`, `chembl_search_targets`, `chembl_get_target`
- **Open Targets:** `opentargets_search`, `opentargets_get_associations`, `opentargets_get_evidence`
- **PubMed:** `pubmed_search_articles`, `pubmed_get_article`, `pubmed_search_preprints`
- **Ensaios clínicos:** `ctg_search_studies`, `ctg_search_by_condition`, `ctg_search_by_intervention`, `ctg_get_study`, `nci_search_trials`, `nci_get_trial`
- **OpenFDA:** `openfda_search_adverse_events`, `openfda_get_adverse_event`, `openfda_search_drug_labels`, `openfda_get_drug_label`, `openfda_search_device_events`
- **OMIM:** `omim_search_entries`, `omim_get_entry`, `omim_search_genes`, `omim_get_gene`, `omim_search_phenotypes`, `omim_get_phenotype`
- **Vias e proteínas:** `reactome_*`, `kegg_*`, `pathwaycommons_*`, `uniprot_*`
- **Genética:** `gwas_*`, `myvariant_*`, `mygene_get_gene`
- **Identificadores:** `nodenorm_get_normalized_nodes`, `uniprot_map_ids`
- **Grafo Every Cure:** `everycure_kg_find_drugs_for_disease`, `everycure_kg_find_diseases_for_drug` e outras

## Receitas rápidas

Exemplos com miastenia gravis. Troque a doença, o gene ou o medicamento.

**Medicamentos para uma doença (aprovados e em estudo, com a fase máxima atingida):**
```bash
curl -X POST https://mcp.cloud.curiloo.com/tools/unified/mcp \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"chembl_find_drugs_by_indication","arguments":{"indication":"myasthenia gravis","max_results":20}},"id":1}'
```

**Alvos de uma doença (primeiro o ID da doença, depois as associações):**
```bash
curl -X POST https://mcp.cloud.curiloo.com/tools/unified/mcp \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"opentargets_search","arguments":{"query":"myasthenia gravis","entity_type":"disease"}},"id":1}'

# Com o ID retornado (ex.: EFO_0004991):
curl -X POST https://mcp.cloud.curiloo.com/tools/unified/mcp \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"opentargets_get_associations","arguments":{"disease_id":"EFO_0004991","size":20}},"id":2}'
```

**Literatura:**
```bash
curl -X POST https://mcp.cloud.curiloo.com/tools/unified/mcp \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"pubmed_search_articles","arguments":{"diseases":["myasthenia gravis"],"keywords":["immunotherapy"],"max_results":20}},"id":1}'
```

**Ensaios clínicos recrutando:**
```bash
curl -X POST https://mcp.cloud.curiloo.com/tools/unified/mcp \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"ctg_search_studies","arguments":{"condition":"myasthenia gravis","recruitment_status":"RECRUITING","max_results":20}},"id":1}'
```

**Eventos adversos notificados:**
```bash
curl -X POST https://mcp.cloud.curiloo.com/tools/unified/mcp \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"openfda_search_adverse_events","arguments":{"drug_name":"eculizumab","limit":50}},"id":1}'
```

**Converter identificadores entre bases:**
```bash
curl -X POST https://mcp.cloud.curiloo.com/tools/unified/mcp \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"nodenorm_get_normalized_nodes","arguments":{"curie":"HGNC:11998"}},"id":1}'
```

Em consultas em lote, espere cerca de 1 segundo entre as chamadas para não sobrecarregar o serviço.

## Fluxos de trabalho

### Reposicionamento de medicamentos
1. **Alvos da doença** (Open Targets): pegue os 5 a 10 mais bem pontuados.
2. **Base genética** (OMIM, GWAS): genes e variantes associados.
3. **Medicamentos que atuam nesses alvos** (ChEMBL, `chembl_find_drugs_by_target`): aprovados para outras doenças são os candidatos mais rápidos; os em fase 1 a 3 entram como hipóteses.
4. **Mecanismo de ação** (`chembl_get_mechanism`): o efeito sobre o alvo faz sentido para a doença (inibir ou ativar)?
5. **Ensaios clínicos** (ClinicalTrials.gov): já há estudos com o candidato nessa doença?
6. **Segurança** (OpenFDA e bula): eventos adversos graves, alertas em tarja preta, interações.
7. **Literatura** (PubMed e preprints): evidências a favor e contra.
8. **Viabilidade** (designação órfã, patentes, exclusividades), se o objetivo for desenvolvimento.

Exemplo de cuidado: na miastenia gravis autoimune, os alvos principais estão no sistema imune (complemento, receptor FcRn, células B). Genes como CHRNE e RAPSN aparecem em buscas genéticas, mas causam as **síndromes miastênicas congênitas**, que são outras doenças. Confira sempre se a doença retornada pelas bases é a mesma da pergunta.

### Descoberta de alvos
1. Associações doença-alvo (Open Targets, GWAS).
2. Contexto de vias (Reactome, KEGG, Pathway Commons).
3. Propriedades da proteína (UniProt): localização, função, se é "druggable".
4. Literatura de validação (PubMed).

### Revisão de evidências clínicas
1. Ensaios registrados (ClinicalTrials.gov, NCI): fase, desenho, situação, resultados publicados.
2. Artigos (PubMed): priorize revisões sistemáticas e ensaios randomizados.
3. Dados de segurança (OpenFDA).

## Como interpretar os resultados

- **Pontuação do Open Targets (0 a 1):** força agregada das evidências de associação, não prova de que o alvo funciona como tratamento.
- **Fase máxima no ChEMBL:** 4 = aprovado (em alguma agência, para alguma indicação), 1 a 3 = em ensaio. Não significa aprovado para a doença pesquisada.
- **Eventos adversos do FAERS (OpenFDA):** são notificações espontâneas. Não provam que o medicamento causou o evento, há muita subnotificação e não permitem calcular frequência. Use para levantar sinais; a frequência está na bula e nos ensaios.
- **Preprints** não passaram por revisão por pares.
- **Ensaios recrutando** são informação de pesquisa; quem tiver interesse em participar deve conversar com o médico.

## No Brasil

- **Anvisa:** consulta de registro de medicamentos e bulário eletrônico (consultas.anvisa.gov.br).
- **ReBEC:** Registro Brasileiro de Ensaios Clínicos (ensaiosclinicos.gov.br).
- **VigiMed:** sistema da Anvisa para notificação de eventos adversos.
- **BVS/LILACS:** literatura científica latino-americana (bvsalud.org).

## Chaves de acesso

A maioria das bases é pública e não exige chave. O **OMIM** exige chave gratuita para pesquisa (omim.org/api). OpenFDA e NCI aceitam chave opcional para limites maiores.

## Problemas comuns

- **Sem resultados:** tente sinônimos, o símbolo do gene em vez do nome da proteína, o nome em inglês ou um identificador padronizado (EFO, MONDO, HGNC).
- **Resultados demais:** use `max_results`, `size`, `limit`, fase ou situação de recrutamento.
- **Erro de chave:** OMIM sem chave configurada.
- **Servidor fora do ar:** tente mais tarde ou rode localmente.

## Formato da resposta

Entregue: (1) a pergunta de pesquisa reformulada; (2) as consultas feitas (base e termos); (3) os achados em tabela, com identificador, fonte e tipo de evidência; (4) uma síntese com limitações e lacunas; e (5) próximos passos de pesquisa. Nunca apresente candidatos como recomendação de tratamento.

---

## Créditos e licença

Skill adaptada e traduzida para o português a partir da skill `medical-research-toolkit` (versão 1.0.0), de autoria de **Pascal Brokmeier** (Every Cure), que acompanha o servidor [pascalwhoop/medical-mcps](https://github.com/pascalwhoop/medical-mcps), sob a licença **MIT** (Copyright (c) 2025 Pascal Brokmeier). Encontrada também na coletânea FreedomIntelligence/OpenClaw-Medical-Skills e no skills-hub.ai.

Alterações desta versão: tradução para o português do Brasil; nova seção "Uso responsável" (pesquisa e não conduta clínica, proibição de enviar dados de pacientes ao servidor público, LGPD, contexto regulatório); instrução para adicionar o servidor ao Claude Code; lista de bases e ferramentas atualizada a partir do servidor em novembro de 2026 (91 ferramentas, incluindo NCI, FDA Orphan, patentes e o grafo Every Cure); correção dos números de volume (ChEMBL reúne compostos, não "2 milhões de medicamentos"; ClinicalTrials.gov tem centenas de milhares de estudos registrados, não "ativos"); orientação para interpretar pontuações, fases e notificações do FAERS; alerta sobre a confusão entre miastenia gravis autoimune e síndromes miastênicas congênitas no exemplo de genes; fontes brasileiras (Anvisa, ReBEC, VigiMed, BVS); incorporação ao SKILL.md do resumo dos arquivos `references` e do fluxo de reposicionamento, que não foram copiados.

Texto da licença original:

```
MIT License

Copyright (c) 2025 Pascal Brokmeier

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
