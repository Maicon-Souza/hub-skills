---
name: workflows-n8n
description: Projeta, configura e depura workflows no n8n: gatilhos (webhook, agendamento), requisições HTTP, nó Code em JavaScript, expressões, transformação de dados, credenciais, tratamento de erros, sub-workflows, lotes e boas práticas de segurança. Use ao criar ou corrigir uma automação no n8n, escrever código para o nó Code ou montar integrações entre sistemas.
---

# Workflows no n8n

Você projeta, configura e depura workflows no n8n, com os nós, expressões e manipulação de dados certos para automações confiáveis. Explique em português do Brasil; mantenha os nomes de nós, campos e códigos como aparecem no n8n (em inglês).

Os exemplos seguem o n8n 1.x e 2.x. Nomes de nós e opções mudam entre versões: confira a versão do usuário (Settings > About ou `n8n --version`) e a documentação oficial (docs.n8n.io) quando algo não bater.

Palavras-chave: n8n, automação, nós, expressões, webhook, HTTP Request, Code, transformação de dados, credenciais, tratamento de erros, Schedule Trigger, filas, sub-workflow, gatilhos.

## Antes de começar

Pergunte o que não foi informado, de uma vez só:

- **Onde o n8n roda:** n8n Cloud ou instalado (Docker, servidor), e a versão.
- **O que a automação faz:** gatilho (o que dispara), passos e destino dos dados.
- **Sistemas envolvidos** e se há API oficial, limites de requisição e credenciais disponíveis.
- **Volume:** quantos itens ou execuções por dia.
- **Dados pessoais** envolvidos (leads, clientes, funcionários).

Não invente endpoints, campos de API ou IDs: use a documentação do sistema ou marque com `[ ]`.

## Segurança e dados

- **Credenciais só no gerenciador de credenciais do n8n**, nunca escritas em nós, expressões ou código. Use permissões mínimas em cada credencial.
- **Proteja os webhooks:** autenticação (Header Auth, Basic Auth ou JWT), validação dos dados recebidos e, se possível, IPs permitidos. Um webhook aberto aceita requisições de qualquer pessoa.
- **Em instalação própria:** HTTPS, usuário administrador com senha forte, `N8N_ENCRYPTION_KEY` guardada com segurança (sem ela, as credenciais salvas não podem ser lidas), backups e atualizações em dia.
- **Dados pessoais (LGPD):** colete só o necessário, defina por quanto tempo os dados de execução ficam salvos (`EXECUTIONS_DATA_MAX_AGE` ou as configurações de retenção) e cuidado com dados pessoais copiados para planilhas e mensagens de chat.
- **Raspagem de sites:** prefira APIs oficiais; ao ler páginas, respeite os termos de uso, o robots.txt e um intervalo razoável entre requisições.

## Primeiros passos rápidos

1. **Instale ou acesse o n8n.** Com Docker:
   ```bash
   docker volume create n8n_data
   docker run -it --rm --name n8n -p 5678:5678 \
     -e GENERIC_TIMEZONE="America/Sao_Paulo" -e TZ="America/Sao_Paulo" \
     -v n8n_data:/home/node/.n8n \
     docker.n8n.io/n8nio/n8n
   ```
   Acesse `http://localhost:5678` e crie o usuário administrador. Para produção, use Docker Compose com banco PostgreSQL, HTTPS e um domínio.
2. **Primeiro workflow:** crie um workflow e adicione um nó **Webhook** com método `POST`.
3. **Processamento:** conecte um nó **Code** (modo "Run Once for Each Item") com:
   ```javascript
   return { json: { mensagem: 'Webhook recebido com sucesso!', dados: $json.body } };
   ```
   No Webhook, os dados enviados chegam em `$json.body`; cabeçalhos em `$json.headers` e parâmetros de URL em `$json.query`.
4. **Teste:** clique em "Listen for test event" e envie:
   ```bash
   curl -X POST -H "Content-Type: application/json" \
     -d '{"nome": "Maria", "email": "maria@exemplo.com"}' \
     https://seu-n8n/webhook-test/meu-caminho
   ```
   A URL de teste (`/webhook-test/`) só funciona enquanto o editor está escutando.
5. **Ative o workflow:** depois de validado, ative pelo botão no topo. A partir daí vale a URL de produção (`/webhook/`), e as execuções aparecem em Executions.

---

## Fluxo 1: leads do formulário para Google Sheets e Slack

1. **Webhook (gatilho)**
   - Method: `POST`; Path: `lead-form`.
   - Authentication: `Header Auth` com um token que o formulário envia (evite `None` em produção).
   - Respond: `Using 'Respond to Webhook' Node`, para devolver uma resposta clara ao formulário (passo 5).
   - Exemplo de corpo recebido:
     ```json
     {
       "nome": "João Silva",
       "email": "joao.silva@empresa.com",
       "telefone": "+5511987654321",
       "origem": "Landing Page X",
       "consentimento": true
     }
     ```

2. **Validação (IF ou Code)**
   - Confira os campos obrigatórios, o formato do e-mail e o consentimento. O que falhar vai para um ramo que responde com erro.
   - Exemplo de condição no nó IF: `{{ $json.body.email }}` "is not empty" e `{{ $json.body.consentimento }}` "is true".

3. **Google Sheets (Append Row)**
   - Credencial: Google Sheets OAuth2 (no n8n Cloud, o login é direto; em instalação própria, crie o cliente OAuth no Google Cloud Console com a API do Sheets ativada e a URL de retorno indicada pelo n8n).
   - Document: escolha a planilha na lista ou cole o ID; Sheet: `Leads`.
   - Mapping: mapeie cada coluna com expressões:
     - Nome: `{{ $('Webhook').item.json.body.nome }}`
     - E-mail: `{{ $('Webhook').item.json.body.email }}`
     - Telefone: `{{ $('Webhook').item.json.body.telefone }}`
     - Origem: `{{ $('Webhook').item.json.body.origem }}`
     - Data de registro: `{{ $now.toISO() }}`

4. **Slack (Send Message)**
   - Credencial: app do Slack com token de bot e permissão `chat:write`, convidado para o canal.
   - Channel: `#leads-novos` (ou o ID do canal).
   - Text:
     ```
     Novo lead: {{ $('Webhook').item.json.body.nome }}
     Origem: {{ $('Webhook').item.json.body.origem }}
     ```
     Evite repetir no Slack dados pessoais que não são necessários ali (telefone, e-mail).

5. **Respond to Webhook**
   - Sucesso: código 200 com `{"status": "recebido"}`. No ramo de validação com erro: código 400 com `{"erro": "dados_invalidos"}`.

## Fluxo 2: monitorar estoque de um produto e avisar no Telegram

1. **Schedule Trigger** (substituiu o antigo Cron)
   - Trigger Interval: `Hours`, a cada `1` hora. Para horários específicos, use uma expressão cron (ex.: `0 8-20 * * 1-5`, de hora em hora, das 8h às 20h, em dias úteis).
   - O fuso segue o `GENERIC_TIMEZONE` da instância (ou o definido nas configurações do workflow).

2. **HTTP Request**
   - Method: `GET`; URL: `[endpoint da API do produto]` (de preferência a API oficial da loja).
   - Header: `User-Agent: [nome do seu monitor]`.
   - Em Options, ative "Retry on Fail" e defina um timeout.
   - Exemplo de resposta:
     ```json
     {
       "product_id": "SKU12345",
       "name": "Smartphone XYZ",
       "price": 999.99,
       "availability": "out_of_stock"
     }
     ```

3. **Code: detectar mudança de estado** (modo "Run Once for All Items")
   ```javascript
   // Guarda o último estado entre execuções.
   // Atenção: os dados estáticos só são salvos em execuções de produção
   // (workflow ativo), não em testes manuais.
   const estado = $getWorkflowStaticData('global');
   const produto = $input.first().json;
   const anterior = estado.disponibilidade ?? 'desconhecido';
   const atual = produto.availability;

   estado.disponibilidade = atual;

   if (atual === 'in_stock' && anterior !== 'in_stock') {
     return [{ json: { mensagem: `${produto.name} voltou ao estoque!`, status: atual } }];
   }

   // Sem mudança relevante: não segue para o próximo nó.
   return [];
   ```

4. **Telegram (Send Message)**
   - Credencial: token do bot criado no @BotFather.
   - Chat ID: o ID do usuário ou grupo (grupos começam com `-100`).
   - Text: `{{ $json.mensagem }}`

---

## Modelos

### Aviso no Discord a partir de um webhook

A forma mais simples é usar um webhook do próprio canal do Discord (Configurações do canal > Integrações > Webhooks > Novo webhook), sem bot:

1. **Webhook** (`POST`, path `discord-lead`, com Header Auth).
2. **HTTP Request**
   - Method: `POST`
   - URL: a URL do webhook do Discord, guardada como credencial ou variável (não escrita direto no nó, porque quem tem a URL pode postar no canal).
   - Body: JSON
     ```json
     {
       "content": "Novo lead: {{ $json.body.leadName }} ({{ $json.body.companyName }})"
     }
     ```

Para usar o nó nativo do Discord (com bot), monte o workflow na interface, que já escolhe a versão correta do nó, e exporte o JSON depois para versionar.

### Code: filtrar e transformar itens (modo "Run Once for All Items")

```javascript
const saida = [];

for (const item of $input.all()) {
  const dados = item.json;

  if (dados.status === 'completed' && dados.value > 100) {
    saida.push({
      json: {
        id: dados.id,
        descricao: `Processo ${dados.name} finalizado.`,
        valorFinal: Math.round(dados.value * 1.05 * 100) / 100, // +5%, com 2 casas decimais
        processadoEm: new Date().toISOString(),
      },
    });
  }
}

return saida;
```

Para filtros e mudanças de campos simples, os nós **Filter**, **Edit Fields (Set)** e **IF** costumam ser mais fáceis de manter que código.

---

## Checklist

- [ ] Credenciais no gerenciador do n8n, com permissões mínimas; nada escrito em nós ou código.
- [ ] Webhooks com autenticação e validação dos dados recebidos.
- [ ] Cada nó testado manualmente com dados reais de exemplo.
- [ ] Tratamento de erros: workflow de erro (Error Trigger) definido nas configurações do workflow e "On Error" ajustado nos nós críticos.
- [ ] "Retry on Fail" em chamadas a APIs externas.
- [ ] Expressões no formato atual (`$json`, `$('Nome do nó').item.json`).
- [ ] Workflow ativado só depois da validação; URL de produção usada nos sistemas externos.
- [ ] Execuções monitoradas e retenção de dados de execução definida.
- [ ] Nós Code otimizados para grandes volumes (lotes, sem carregar tudo de uma vez).
- [ ] Anotações (Sticky Notes) explicando as partes complexas.
- [ ] Idempotência: reenvio do mesmo webhook ou execução repetida não duplica registros.
- [ ] Dados pessoais tratados conforme a LGPD.

## Métricas de referência

Valores indicativos da skill original, sem fonte citada; dependem muito do servidor, das APIs externas e do tipo de workflow.

| Métrica | Ideal | Aceitável |
|---|---|---|
| Tempo médio de execução | menos de 500 ms | menos de 2 s |
| Taxa de sucesso | acima de 99,5% | acima de 98% |
| Memória por execução | menos de 256 MB | menos de 512 MB |
| Execuções por minuto (webhook) | acima de 100 | acima de 50 |
| Erros de credencial ou API | 0% | menos de 0,1% |
| Tempo para resolver um erro | menos de 30 min | menos de 60 min |

Para volumes altos, use o modo de fila (queue mode, com Redis e workers) em instalação própria.

## Erros comuns

1. **Credenciais inválidas ou expiradas.** Tokens OAuth vencem ou não têm a permissão certa.
   Como evitar: confira os escopos no provedor (Google Cloud, Slack), reconecte a credencial no n8n e use o mínimo de permissões.
2. **JSON inválido ou campos no lugar errado.** Corpo da requisição mal formado, ou leitura de `$json.nome` quando o dado está em `$json.body.nome`.
   Como evitar: inspecione a saída de cada nó no painel de execução antes de mapear. Use o nó Edit Fields (Set) para organizar os campos.
3. **Expressões apontando para dados que não existem.** `{{ $json.campoInexistente }}` ou referência a um nó que não rodou.
   Como evitar: confira a saída do nó anterior, teste no editor de expressões, use IF para tratar ausência de dados e o operador `??` para valores padrão (`{{ $json.cidade ?? 'não informada' }}`).
4. **Nomes de nós antigos em tutoriais.** Function (hoje Code), Cron (hoje Schedule Trigger), Split In Batches (hoje Loop Over Items), Item Lists (dividido em Split Out, Aggregate, Remove Duplicates, Sort, Limit e Summarize).
5. **Testar só pela URL de teste.** Depois de ativar, os sistemas externos precisam chamar a URL de produção.

## Dicas avançadas

1. **Sub-workflows com Execute Workflow:** separe tarefas reutilizáveis (validação, notificação, atualização de estoque) em workflows próprios, iniciados pelo gatilho "When Executed by Another Workflow". O workflow principal fica limpo e cada parte tem seu tratamento de erro.
2. **Estado entre execuções:** `$getWorkflowStaticData('global')` (ou `'node'`) guarda pequenas informações entre execuções de produção, como o último ID processado ou a data da última execução bem-sucedida. Para volumes maiores ou dados importantes, use um banco de dados ou uma tabela.
3. **Lotes:** o nó **Loop Over Items** divide listas grandes em lotes (ex.: 1.000 e-mails em lotes de 50), e o nó **Wait** cria intervalos para respeitar limites de requisição. O HTTP Request também tem a opção Batching.
4. **Respond to Webhook:** devolve ao sistema que chamou o código HTTP, os cabeçalhos e o corpo que você quiser. Ex.: `200` com `{"status": "recebido", "pedidoId": "XYZ123"}` ou `400` com `{"erro": "dados_invalidos"}`.
5. **Configurações por ambiente:** para URLs e parâmetros que mudam entre desenvolvimento e produção, use as Variables do n8n (`{{ $vars.CRM_API_URL }}`, conforme o plano) ou variáveis de ambiente (`{{ $env.CRM_API_URL }}`). Versões recentes podem bloquear o acesso a `$env` nos nós por padrão (`N8N_BLOCK_ENV_ACCESS_IN_NODE`); confira a configuração da sua instância. Segredos continuam nas credenciais.

## Formato da resposta

Entregue: o desenho do workflow (gatilho, nós em ordem, ramos de erro); a configuração de cada nó com as expressões; o código dos nós Code; como testar (dados de exemplo e comando curl); e os pontos de atenção (credenciais, limites, dados pessoais, versão do n8n). Se o usuário pedir o JSON para importar, avise que as versões dos nós podem variar e que o mais seguro é montar na interface e exportar.

---

## Créditos e licença

Skill adaptada a partir da skill `n8n-workflow` (versão 1.0.0), de autoria de **Cafe Code AI**, publicada em [kursku/skills](https://github.com/kursku/skills) (pasta `packs/kit-510-ptbr/12-ia-automacao/n8n-workflow`). A licença **MIT** está declarada nos metadados da skill original; o repositório não traz um arquivo de licença próprio.

Alterações desta versão: atualização para o n8n atual (nó Function trocado por Code, Cron por Schedule Trigger, Split In Batches por Loop Over Items, Item Lists pelos nós que o substituíram, Webhook Response por Respond to Webhook, expressões `$node[...]` por `$('...')`); correção do código de persistência, que usava a função inexistente `setWorkflowStaticData` e não retornava nada em um dos casos, com aviso de que o estado só é salvo em execuções de produção; correção do exemplo de webhook, que lia `$json.nome` em vez de `$json.body.nome`; modelo de Discord com versão de nó incerta trocado por webhook do canal via HTTP Request; nova seção "Segurança e dados" (credenciais, autenticação de webhooks, chave de criptografia, LGPD e raspagem de sites); validação e resposta ao formulário no fluxo de leads; comando Docker com volume nomeado, imagem oficial e fuso de São Paulo; nota sobre o bloqueio de `$env`; inclusão das seções "Antes de começar" e "Formato da resposta"; aviso de que as métricas não têm fonte; checklist desmarcado para uso.

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
