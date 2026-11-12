---
name: dashboards-grafana
description: Cria e mantém dashboards do Grafana prontos para produção, para monitorar aplicações, infraestrutura, bancos de dados, SLOs e indicadores de negócio, com painéis, consultas PromQL, variáveis, alertas e provisionamento como código (arquivos, Terraform, Ansible). Use ao montar ou revisar um dashboard do Grafana ou escolher o que monitorar.
---

# Dashboards do Grafana

Você cria e mantém dashboards do Grafana prontos para produção, para dar visibilidade sobre sistemas e negócio. Explique em português do Brasil; mantenha em inglês as chaves de JSON, as consultas e os nomes técnicos. Títulos e descrições dos painéis seguem a língua da equipe.

## Quando usar

- Visualizar métricas do Prometheus (ou de outras fontes: Loki, PostgreSQL, CloudWatch, Elasticsearch).
- Criar dashboards personalizados.
- Montar dashboards de SLO.
- Monitorar infraestrutura e bancos de dados.
- Acompanhar indicadores de negócio.

Não use para tarefas sem relação com dashboards do Grafana.

## Antes de começar

Pergunte o que não foi informado, de uma vez só:

- **Versão do Grafana** (os exemplos são para o Grafana 11 e 12).
- **Fonte de dados** e os nomes reais das métricas e labels (ex.: `http_requests_total` com `service`, `status`). Se possível, peça um exemplo de métrica ou acesso ao Prometheus.
- **Público do dashboard:** quem está de plantão, a equipe de desenvolvimento, a gestão?
- **O que precisa ser respondido em 5 segundos** ao abrir o dashboard.
- **Como o dashboard será publicado:** pela interface, por arquivo de provisionamento ou por Terraform.

Use as métricas que existem de fato. Se não souber o nome de uma métrica, pergunte ou marque com `[ ]`; consultas com nomes inventados geram painéis vazios.

## Princípios de design

### 1. Hierarquia da informação

```
┌──────────────────────────────────────────┐
│  Números críticos (stat)                 │
├──────────────────────────────────────────┤
│  Tendências principais (séries temporais)│
├──────────────────────────────────────────┤
│  Detalhes (tabelas, heatmaps)            │
└──────────────────────────────────────────┘
```

O mais importante fica no topo, à esquerda. Detalhes vão para baixo, em linhas (rows) recolhíveis, ou para um dashboard de detalhe ligado por link.

### 2. Método RED (serviços)
- **Rate:** requisições por segundo.
- **Errors:** taxa de erros.
- **Duration:** latência (percentis, não média).

### 3. Método USE (recursos)
- **Utilization:** % do tempo em que o recurso está ocupado.
- **Saturation:** fila ou espera.
- **Errors:** contagem de erros.

---

## Estrutura de um dashboard

### Dashboard de monitoramento de API

O JSON abaixo é o próprio dashboard, como usado em arquivos de provisionamento. Pela API HTTP do Grafana, ele vai dentro de `{"dashboard": {...}, "overwrite": true}`.

```json
{
  "title": "Monitoramento da API",
  "uid": "api-monitoramento",
  "tags": ["api", "producao"],
  "timezone": "browser",
  "refresh": "30s",
  "time": {"from": "now-6h", "to": "now"},
  "templating": {
    "list": [
      {
        "name": "datasource",
        "type": "datasource",
        "query": "prometheus",
        "label": "Fonte de dados"
      }
    ]
  },
  "panels": [
    {
      "title": "Requisições por segundo",
      "type": "timeseries",
      "datasource": {"type": "prometheus", "uid": "${datasource}"},
      "targets": [
        {
          "refId": "A",
          "expr": "sum by (service) (rate(http_requests_total[$__rate_interval]))",
          "legendFormat": "{{service}}"
        }
      ],
      "fieldConfig": {"defaults": {"unit": "reqps"}},
      "gridPos": {"x": 0, "y": 0, "w": 12, "h": 8}
    },
    {
      "title": "Taxa de erros (%)",
      "type": "timeseries",
      "datasource": {"type": "prometheus", "uid": "${datasource}"},
      "targets": [
        {
          "refId": "A",
          "expr": "100 * sum(rate(http_requests_total{status=~\"5..\"}[$__rate_interval])) / sum(rate(http_requests_total[$__rate_interval]))",
          "legendFormat": "Erros"
        }
      ],
      "fieldConfig": {
        "defaults": {
          "unit": "percent",
          "min": 0,
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {"color": "green", "value": null},
              {"color": "red", "value": 5}
            ]
          },
          "custom": {"thresholdsStyle": {"mode": "line"}}
        }
      },
      "gridPos": {"x": 12, "y": 0, "w": 12, "h": 8}
    },
    {
      "title": "Latência P95",
      "type": "timeseries",
      "datasource": {"type": "prometheus", "uid": "${datasource}"},
      "targets": [
        {
          "refId": "A",
          "expr": "histogram_quantile(0.95, sum by (le, service) (rate(http_request_duration_seconds_bucket[$__rate_interval])))",
          "legendFormat": "{{service}}"
        }
      ],
      "fieldConfig": {"defaults": {"unit": "s"}},
      "gridPos": {"x": 0, "y": 8, "w": 24, "h": 8}
    }
  ]
}
```

Notas:
- `$__rate_interval` ajusta a janela ao intervalo de coleta e ao zoom; prefira-o a um valor fixo como `[5m]`.
- O fuso pode ser `"browser"` (o de quem abre) ou fixo, como `"America/Sao_Paulo"`.
- Alertas não ficam mais no JSON do painel; veja a seção de alertas.

## Tipos de painel

O antigo painel `graph` foi removido no Grafana 11; use `timeseries`. Unidades, limites e cores ficam em `fieldConfig`.

### 1. Stat (valor único)

```json
{
  "type": "stat",
  "title": "Requisições no período",
  "datasource": {"type": "prometheus", "uid": "${datasource}"},
  "targets": [{"refId": "A", "expr": "sum(increase(http_requests_total[$__range]))", "instant": true}],
  "options": {
    "reduceOptions": {"values": false, "calcs": ["lastNotNull"]},
    "colorMode": "value",
    "textMode": "auto"
  },
  "fieldConfig": {"defaults": {"unit": "short", "decimals": 0}}
}
```

Contadores (`_total`) só crescem e zeram quando o serviço reinicia; mostre `increase()` ou `rate()`, nunca o valor bruto do contador.

### 2. Série temporal

```json
{
  "type": "timeseries",
  "title": "Uso de CPU",
  "datasource": {"type": "prometheus", "uid": "${datasource}"},
  "targets": [{
    "refId": "A",
    "expr": "100 * (1 - avg by (instance) (rate(node_cpu_seconds_total{mode=\"idle\"}[$__rate_interval])))",
    "legendFormat": "{{instance}}"
  }],
  "fieldConfig": {
    "defaults": {
      "unit": "percent",
      "min": 0,
      "max": 100,
      "thresholds": {
        "mode": "absolute",
        "steps": [
          {"color": "green", "value": null},
          {"color": "yellow", "value": 80},
          {"color": "red", "value": 90}
        ]
      }
    }
  }
}
```

### 3. Tabela

```json
{
  "type": "table",
  "title": "Situação dos serviços",
  "datasource": {"type": "prometheus", "uid": "${datasource}"},
  "targets": [{"refId": "A", "expr": "up", "format": "table", "instant": true}],
  "transformations": [
    {
      "id": "organize",
      "options": {
        "excludeByName": {"Time": true, "__name__": true},
        "renameByName": {"instance": "Instância", "job": "Serviço", "Value": "Situação"}
      }
    }
  ],
  "fieldConfig": {
    "defaults": {
      "mappings": [
        {"type": "value", "options": {
          "0": {"text": "Fora do ar", "color": "red"},
          "1": {"text": "No ar", "color": "green"}
        }}
      ]
    }
  }
}
```

### 4. Heatmap

```json
{
  "type": "heatmap",
  "title": "Distribuição da latência",
  "datasource": {"type": "prometheus", "uid": "${datasource}"},
  "targets": [{
    "refId": "A",
    "expr": "sum by (le) (increase(http_request_duration_seconds_bucket[$__rate_interval]))",
    "format": "heatmap",
    "legendFormat": "{{le}}"
  }],
  "options": {
    "calculate": false,
    "yAxis": {"unit": "s"},
    "color": {"scheme": "Oranges"}
  }
}
```

## Variáveis

### Variáveis de consulta

```json
{
  "templating": {
    "list": [
      {"name": "datasource", "type": "datasource", "query": "prometheus"},
      {
        "name": "namespace",
        "type": "query",
        "datasource": {"type": "prometheus", "uid": "${datasource}"},
        "query": "label_values(kube_pod_info, namespace)",
        "refresh": 1,
        "multi": false
      },
      {
        "name": "service",
        "type": "query",
        "datasource": {"type": "prometheus", "uid": "${datasource}"},
        "query": "label_values(kube_service_info{namespace=\"$namespace\"}, service)",
        "refresh": 2,
        "multi": true,
        "includeAll": true
      }
    ]
  }
}
```

`refresh: 1` atualiza ao abrir o dashboard; `2`, também ao mudar o período.

### Uso nas consultas

```
sum(rate(http_requests_total{namespace="$namespace", service=~"$service"}[$__rate_interval]))
```

Com seleção múltipla ou "All", use `=~` (regex), não `=`.

## Alertas

Os alertas antigos, configurados dentro do painel, foram removidos no Grafana 11. Hoje há dois caminhos:

**1. Regras do Grafana Alerting**, criadas na interface (Alerting > Alert rules) ou provisionadas por arquivo:

```yaml
# provisioning/alerting/regras-api.yaml
apiVersion: 1
groups:
  - orgId: 1
    name: api
    folder: Monitoramento
    interval: 1m
    rules:
      - uid: api-taxa-erros-alta
        title: Taxa de erros alta na API
        condition: C
        for: 5m
        data:
          - refId: A
            datasourceUid: "[uid-do-prometheus]"
            relativeTimeRange: {from: 600, to: 0}
            model:
              expr: 100 * sum(rate(http_requests_total{status=~"5.."}[5m])) / sum(rate(http_requests_total[5m]))
          - refId: C
            datasourceUid: __expr__
            model:
              type: threshold
              expression: A
              conditions:
                - evaluator: {type: gt, params: [5]}
        labels:
          severidade: critica
        annotations:
          summary: "Taxa de erros acima de 5% há 5 minutos"
        noDataState: NoData
        execErrState: Error
```

As notificações (Slack, e-mail, Teams, PagerDuty) são definidas nos pontos de contato (contact points) e nas políticas de notificação, não na regra.

**2. Regras do Prometheus com Alertmanager**, quando o time já usa esse fluxo:

```yaml
groups:
  - name: api
    rules:
      - alert: TaxaDeErrosAlta
        expr: 100 * sum(rate(http_requests_total{status=~"5.."}[5m])) / sum(rate(http_requests_total[5m])) > 5
        for: 5m
        labels:
          severidade: critica
        annotations:
          summary: "Taxa de erros acima de 5% há 5 minutos"
```

Alerte pelo que afeta o usuário (erros, latência, SLO), não por toda métrica disponível; alerta demais vira ruído e é ignorado.

## Provisionamento de dashboards

```yaml
# provisioning/dashboards/dashboards.yaml
apiVersion: 1

providers:
  - name: padrao
    orgId: 1
    folder: Monitoramento
    type: file
    disableDeletion: false
    updateIntervalSeconds: 30
    allowUiUpdates: false
    options:
      path: /var/lib/grafana/dashboards
      foldersFromFilesStructure: true
```

Com `allowUiUpdates: false`, a fonte da verdade é o arquivo: mudanças feitas na interface não podem ser salvas por cima, o que evita perder alterações na próxima atualização.

## Padrões comuns

### Infraestrutura
Uso de CPU e memória por nó, I/O de disco e espaço livre, tráfego de rede, quantidade de pods por namespace e situação dos nós.

### Banco de dados
Consultas por segundo, uso do pool de conexões e conexões ativas, latência das consultas (P50, P95, P99), tamanho do banco, atraso da replicação e consultas lentas.

### Aplicação
Taxa de requisições, taxa de erros, tempo de resposta em percentis, usuários ou sessões ativas, taxa de acerto do cache e tamanho das filas.

### SLO
Disponibilidade e latência dentro da meta, orçamento de erro restante (error budget) e taxa de consumo do orçamento (burn rate) em janelas curta e longa.

Para começar, os dashboards prontos da comunidade (grafana.com/grafana/dashboards) são uma boa base, como o "Node Exporter Full"; adapte às suas métricas e padrões.

## Boas práticas

1. **Comece por modelos** da comunidade e adapte.
2. **Nomes consistentes** para painéis e variáveis.
3. **Agrupe métricas relacionadas** em linhas (rows).
4. **Período padrão adequado** (ex.: últimas 6 horas) e atualização não agressiva (30 s ou mais).
5. **Use variáveis** (fonte de dados, ambiente, serviço) em vez de duplicar dashboards.
6. **Descreva os painéis**: o que mostram e o que fazer se algo estiver fora do normal.
7. **Configure as unidades** (`reqps`, `percent`, `s`, `bytes`).
8. **Limites com sentido** para as cores, ligados a SLOs ou ao comportamento normal.
9. **Cores consistentes** entre dashboards (ex.: erro sempre vermelho).
10. **Teste com períodos diferentes** (1 hora, 1 dia, 1 semana) e com variáveis em "All".
11. **Poucos painéis por dashboard** (até cerca de 20); consultas pesadas deixam tudo lento. Para métricas caras, use regras de gravação (recording rules) no Prometheus.
12. **Cuidado com dados pessoais**: não exiba e-mails, CPFs ou outros dados de usuários em painéis ou labels (LGPD).

## Dashboard como código

### Terraform

```hcl
resource "grafana_folder" "monitoramento" {
  title = "Monitoramento de produção"
}

resource "grafana_dashboard" "api_monitoramento" {
  config_json = file("${path.module}/dashboards/api-monitoramento.json")
  folder      = grafana_folder.monitoramento.uid
}
```

### Ansible

```yaml
- name: Copiar dashboards do Grafana
  ansible.builtin.copy:
    src: "{{ item }}"
    dest: /var/lib/grafana/dashboards/
    owner: grafana
    group: grafana
    mode: "0644"
  with_fileglob:
    - "dashboards/*.json"
```

Com o provisionamento por arquivo, o Grafana relê a pasta sozinho no intervalo de `updateIntervalSeconds`; não é preciso reiniciar o serviço.

Guarde os JSONs no Git e revise as mudanças por pull request. Ao exportar da interface, use "Export > Export as JSON" e remova os IDs numéricos (`id`), mantendo o `uid` fixo.

## Formato da resposta

Entregue: (1) o objetivo e o público do dashboard em uma frase; (2) o layout proposto (linhas e painéis, de cima para baixo); (3) o JSON completo do dashboard, válido para a versão informada; (4) as regras de alerta, se pedidas; e (5) os pontos a confirmar (nomes de métricas e labels, UID da fonte de dados, versão do Grafana).

---

## Créditos e licença

Skill adaptada e traduzida para o português a partir da skill `grafana-dashboards` do projeto [wshobson/agents](https://github.com/wshobson/agents) (plugin `observability-monitoring`), de **Seth Hobson**, sob a licença **MIT** (Copyright (c) 2024 Seth Hobson). Encontrada na coletânea GovHub-br/gov-hub-skills (pasta `04-infra-devops/grafana-dashboards`).

Alterações desta versão: tradução para o português do Brasil; exemplos atualizados para o Grafana 11 e 12 (painel `graph`, removido, trocado por `timeseries`; `yaxes` trocado por `fieldConfig`; heatmap no formato atual; fonte de dados como variável; `$__rate_interval`); alertas legados dentro do painel, removidos no Grafana 11, trocados por regras do Grafana Alerting e do Prometheus; painel stat corrigido para não mostrar o valor bruto de um contador; tabela com mapeamento de valores; JSON do dashboard sem o invólucro da API, com nota sobre quando usá-lo; Terraform com `uid` da pasta e Ansible sem reinício desnecessário; remoção das referências a arquivos de exemplo e de outras skills que não acompanham esta versão; inclusão das seções "Antes de começar", "Formato da resposta", padrão de SLO e boas práticas sobre desempenho e dados pessoais.

Texto da licença original:

```
MIT License

Copyright (c) 2024 Seth Hobson

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
