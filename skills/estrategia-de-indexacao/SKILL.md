---
name: estrategia-de-indexacao
description: Diagnostica e resolve problemas de rastreamento e indexação no Google: robots.txt, sitemaps, noindex, canonical, redirecionamentos, conteúdo duplicado, parâmetros de URL, orçamento de rastreamento, renderização de JavaScript, hreflang e Google Search Console. Use quando páginas não aparecem no Google, aparecem as versões erradas ou ao auditar a parte técnica de SEO de um site.
---

# Estratégia de indexação

Você ajuda a garantir que os buscadores rastreiem e indexem as páginas certas de um site, na versão certa, e deixem de lado as que não interessam. Responda em português do Brasil, com exemplos de código prontos para usar.

Palavras-chave: rastreamento, indexação, orçamento de rastreamento (crawl budget), robots.txt, sitemap.xml, noindex, canonical, hreflang, Google Search Console, renderização, análise de logs, conteúdo duplicado, códigos HTTP, meta robots.

## Antes de começar

Pergunte o que não foi informado, de uma vez só:

- **Site:** tamanho aproximado (número de páginas), tipo (blog, e-commerce, institucional, portal), plataforma (WordPress, Shopify, VTEX, Next.js etc.).
- **Problema:** páginas que não indexam, páginas que não deveriam aparecer, queda de tráfego, migração de domínio?
- **Acessos:** Google Search Console, logs do servidor, ferramenta de rastreamento (Screaming Frog, Sitebulb).
- **Idiomas e países**, se o site tiver versões diferentes.

Peça os dados reais (relatórios do Search Console, robots.txt, exemplos de URLs). Não presuma o estado do site.

## Conceitos que mais geram confusão

- **robots.txt bloqueia o rastreamento, não a indexação.** Uma URL bloqueada ainda pode aparecer no Google (sem descrição) se tiver links apontando para ela.
- **noindex tira do índice, mas só funciona se o Google conseguir ler a página.** Não combine `noindex` com bloqueio no robots.txt.
- **canonical é uma sugestão.** O Google pode escolher outra versão se os sinais (links, redirecionamentos, sitemap) disserem outra coisa.
- **Conteúdo duplicado não gera penalidade.** O efeito é o Google escolher uma versão e ignorar as outras; o problema é quando ele escolhe a errada ou desperdiça rastreamento.
- **Orçamento de rastreamento só preocupa sites grandes.** Segundo o Google, sites com mais de cerca de 1 milhão de páginas, ou com mais de 10 mil páginas mudando todo dia. Para a maioria dos sites, indexação depende de qualidade e de sinais claros, não de orçamento.

## Primeiros passos rápidos

1. **Relatório "Páginas" do Search Console** (Indexação > Páginas): veja quantas páginas estão indexadas e os motivos de não indexação. Use a **Inspeção de URL** para checar páginas específicas.
2. **Confira o robots.txt** em `seudominio.com.br/robots.txt`: nada de bloquear CSS, JavaScript, imagens ou páginas importantes.
3. **Sitemaps enviados e sem erros** no Search Console (ex.: `sitemap.xml`, `sitemap-produtos.xml`), contendo só URLs que você quer indexadas.
4. **Canonical nas variações.** Para URLs com o mesmo conteúdo (ex.: `https://exemplo.com.br/produto?cor=azul` e `https://exemplo.com.br/produto/`), a variação aponta para a versão principal com `rel="canonical"`.

---

## Fluxo 1: rastreamento eficiente (sites grandes)

1. **Analise os logs do servidor.** Ferramentas: Screaming Frog Log File Analyser, ELK, Splunk ou os relatórios da hospedagem ou CDN. Filtre pelo Googlebot (confirme que é o robô verdadeiro pelo DNS reverso) e veja onde o rastreamento vai. Complemente com o relatório "Estatísticas de rastreamento" do Search Console (Configurações).
   Exemplo: 30% das visitas do Googlebot vão para URLs de filtro como `/categoria?cor=vermelha&tamanho=p`, que não trazem tráfego orgânico.

2. **Liste as URLs de baixo valor:** login, carrinho, checkout, busca interna, filtros e ordenações combinados, tags e categorias vazias, páginas de sessão. Exemplo: `/carrinho/` e `/minha-conta/` sendo rastreadas.

3. **Escolha a ferramenta certa para cada caso:**

   | Situação | Solução |
   |---|---|
   | Área que não deve nem ser rastreada (admin, carrinho, checkout, busca interna) | `Disallow` no robots.txt |
   | Página útil para o usuário, mas que não deve aparecer no Google (agradecimento, filtros sem valor) | `<meta name="robots" content="noindex">`, sem bloquear no robots.txt |
   | Variação de uma página principal (parâmetros, ordenação, rastreio de campanha) | `rel="canonical"` para a principal |
   | URL que mudou de endereço definitivamente | Redirecionamento 301 |
   | Página que deixou de existir sem substituta | Código 404 ou 410 |

   Exemplo de robots.txt:
   ```
   User-agent: *
   Disallow: /wp-admin/
   Allow: /wp-admin/admin-ajax.php
   Disallow: /carrinho/
   Disallow: /minha-conta/
   Disallow: /busca

   Sitemap: https://www.exemplo.com.br/sitemap.xml
   ```

4. **noindex para ajuste fino.** `<meta name="robots" content="noindex">` no `<head>` (ou o cabeçalho HTTP `X-Robots-Tag: noindex` para PDFs e outros arquivos). Os links da página continuam sendo seguidos; não é preciso acrescentar `follow`.

5. **Servidor rápido e estável.** O ritmo de rastreamento depende principalmente do tempo de resposta do servidor e da ausência de erros (5xx, timeouts). Servidor lento ou com erros faz o Google rastrear menos. As métricas de experiência da página (Core Web Vitals: LCP, INP e CLS) são outra coisa: importam para o usuário e como sinal de qualidade, mas não determinam o volume de rastreamento.

6. **Links internos levam o rastreamento.** Páginas importantes devem estar a poucos cliques da home, com links de texto descritivos. Não use `nofollow` em links internos (por exemplo, para a política de privacidade): isso não economiza rastreamento e só atrapalha a leitura da estrutura do site.

## Fluxo 2: duplicidade e canonização

1. **Encontre as duplicações.** No relatório "Páginas", os motivos mais comuns são "Cópia sem página canônica selecionada pelo usuário", "Cópia, o Google escolheu uma canônica diferente da do usuário" e "Página alternativa com tag canônica adequada" (este último é normal). Complete com Screaming Frog ou Sitebulb.
   Fontes típicas: HTTP e HTTPS, com e sem www, barra no final ou não, parâmetros de campanha (`?utm_source=`), sessão, ordenação e filtros, versões para impressão, conteúdo republicado em outros sites.

2. **Escolha a versão principal de cada grupo**: a URL mais limpa, em HTTPS, no domínio preferido (com ou sem www), com o padrão de barra final usado no site todo.

3. **Canonical em todas as páginas.** Cada página indexável aponta para si mesma (canonical autorreferente, que é o recomendado); as variações apontam para a principal. Use URL absoluta.
   Exemplo, em `https://www.exemplo.com.br/produtos/camiseta-basica/?cor=azul`:
   ```html
   <link rel="canonical" href="https://www.exemplo.com.br/produtos/camiseta-basica/" />
   ```

4. **Redirecione as versões do domínio com 301.** Tudo para `https://www.exemplo.com.br` (ou para a versão sem www, se for a escolhida). Exemplo para Apache (`.htaccess`):
   ```apache
   RewriteEngine On
   RewriteCond %{HTTPS} off [OR]
   RewriteCond %{HTTP_HOST} !^www\. [NC]
   RewriteRule ^(.*)$ https://www.exemplo.com.br%{REQUEST_URI} [L,R=301]
   ```
   No Nginx, use um bloco `server` que retorne `301` para o domínio preferido. Evite cadeias de redirecionamento (A → B → C).

5. **Parâmetros de URL.** A antiga ferramenta de parâmetros do Search Console foi desativada em 2022. Hoje o controle é feito com canonical, links internos sempre para a versão limpa, robots.txt para combinações de filtros sem valor e parâmetros de campanha fora dos links internos.

6. **Acompanhe depois das mudanças.** Em 2 a 8 semanas, o relatório "Páginas" deve mostrar menos cópias sem canônica e as versões principais indexadas. Use a Inspeção de URL para confirmar a canônica escolhida pelo Google.

## Outros pontos de atenção

- **Indexação mobile-first.** O Google indexa a versão para celular. Conteúdo, links internos e dados estruturados precisam estar completos nela. Para testar, use a Inspeção de URL do Search Console e o Lighthouse (o antigo teste de compatibilidade com dispositivos móveis foi desativado em 2023).
- **JavaScript.** Sites que montam o conteúdo no navegador (React, Vue, Angular) podem ter atraso ou falhas de indexação. Confira na Inspeção de URL se o HTML renderizado mostra o conteúdo e os links; quando possível, use renderização no servidor ou geração estática.
- **hreflang** para sites com versões em idiomas ou países diferentes: cada versão lista todas as outras e a si mesma, com códigos corretos (ex.: `pt-BR`, `pt-PT`, `es`), e as páginas se citam de volta.
- **Remoção urgente.** A ferramenta "Remoções" do Search Console esconde uma URL por cerca de 6 meses. Para remover de vez, use noindex, 404/410 ou proteção por senha.
- **Paginação.** O Google não usa mais `rel="prev/next"`; cada página da paginação deve ter canonical para si mesma e links rastreáveis para as próximas.

---

## Modelos

### robots.txt para e-commerce

```
User-agent: *
Disallow: /admin/
Disallow: /carrinho/
Disallow: /checkout/
Disallow: /minha-conta/
Disallow: /busca
Disallow: /*?*sort=
Disallow: /*?*filter=

Sitemap: https://www.exemplo-loja.com.br/sitemap.xml
Sitemap: https://www.exemplo-loja.com.br/sitemap-produtos.xml
```

Antes de bloquear filtros, confirme que nenhuma combinação de filtro traz tráfego orgânico relevante (ex.: "tênis de corrida feminino"). Se trouxer, transforme essa combinação numa página de categoria própria, com URL limpa. Teste as regras no relatório do robots.txt do Search Console antes de publicar.

### Canonical numa página com parâmetros

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Camiseta básica | Exemplo Loja</title>
  <link rel="canonical" href="https://www.exemplo.com.br/produtos/camiseta-basica/" />
</head>
<body>
  <!-- conteúdo da página -->
</body>
</html>
```

### Sitemap XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://www.exemplo.com.br/produtos/camiseta-basica/</loc>
    <lastmod>2026-07-26</lastmod>
  </url>
  <url>
    <loc>https://www.exemplo.com.br/categorias/camisetas/</loc>
    <lastmod>2026-07-25</lastmod>
  </url>
</urlset>
```

O Google ignora `<priority>` e `<changefreq>`; o que ele usa é o `<lastmod>`, desde que seja verdadeiro (só muda quando o conteúdo muda de fato). Cada arquivo aceita até 50 mil URLs ou 50 MB; acima disso, use um índice de sitemaps.

---

## Checklist

- [ ] Relatório "Páginas" do Search Console revisado, com os motivos de não indexação entendidos.
- [ ] robots.txt sem bloqueio de CSS, JavaScript, imagens ou páginas importantes.
- [ ] Nenhuma página com noindex bloqueada no robots.txt.
- [ ] Sitemaps enviados, sem erros, só com URLs indexáveis (200, canônicas, sem noindex).
- [ ] Canonical autorreferente nas páginas indexáveis e apontando para a principal nas variações.
- [ ] noindex removido de páginas que devem aparecer no Google.
- [ ] HTTP → HTTPS e www/sem www com 301, sem cadeias.
- [ ] Links internos sempre para a versão limpa das URLs.
- [ ] Servidor rápido e sem erros 5xx frequentes.
- [ ] Conteúdo e links completos na versão para celular.
- [ ] Conteúdo carregado por JavaScript visível no HTML renderizado.
- [ ] hreflang correto, se houver idiomas ou países diferentes.
- [ ] Logs analisados (sites grandes).

## Métricas para acompanhar

Não há referência universal: compare o site com ele mesmo ao longo do tempo.

| Métrica | Onde ver | O que buscar |
|---|---|---|
| Páginas indexadas ÷ páginas que você quer indexar | Search Console (Páginas) e sitemaps | Próximo de 100% das URLs do sitemap |
| Motivos de não indexação | Search Console (Páginas) | Queda de "cópia sem canônica", "rastreada, mas não indexada", erros 5xx e 404 inesperados |
| Tempo médio de resposta e erros de servidor | Search Console (Estatísticas de rastreamento) | Estável e baixo |
| Rastreamento em URLs de baixo valor | Logs do servidor | Em queda depois dos ajustes |
| Core Web Vitals (LCP até 2,5 s; INP até 200 ms; CLS até 0,1) | Search Console, PageSpeed Insights | Maioria das URLs como "boas" |
| Cliques e impressões orgânicos | Search Console (Desempenho) | Em alta nas páginas principais |

## Erros comuns

1. **Bloquear CSS, JavaScript ou imagens no robots.txt.** O Google não consegue renderizar a página como o usuário vê.
   Como evitar: tire os bloqueios de recursos. Exemplo: se `Disallow: /assets/` bloqueia CSS e JS, remova ou seja mais específico.
2. **noindex junto com Disallow.** Bloqueada, a página não é rastreada, o Google nunca lê o noindex e ela pode continuar no índice.
   Como evitar: para tirar do índice, libere o rastreamento e use só o noindex. Depois que sair, se quiser, bloqueie.
3. **Canonical apontando para a versão errada.** Canonical para uma URL com parâmetros, redirecionada, com noindex ou com 404.
   Como evitar: canonical sempre para a URL limpa, que responde 200 e é indexável. Exemplo: em `/produto/?cor=vermelho`, o canonical é `/produto/`.
4. **Sitemaps sujos.** URLs 404, redirecionadas, bloqueadas ou com noindex no sitemap confundem os sinais.
   Como evitar: sitemaps gerados automaticamente pelo sistema, só com URLs indexáveis, revisados no Search Console.
5. **Mudanças grandes sem acompanhamento.** Migração de domínio ou de plataforma sem mapa de redirecionamentos derruba o tráfego.
   Como evitar: mapa de 301 de URL para URL, testes antes de publicar e acompanhamento diário do Search Console nas semanas seguintes.

## Dicas avançadas

1. **Sitemaps segmentados.** Em sites grandes, separe por tipo (produtos, categorias, artigos) para ver a taxa de indexação de cada grupo no Search Console.
2. **Arquitetura de links.** A home leva às categorias principais, que levam aos produtos mais importantes. Páginas órfãs (sem nenhum link interno) raramente são bem indexadas.
3. **Logs para achar desperdício.** Exemplo: o Googlebot visita milhares de URLs 404 de um diretório antigo. Redirecione as que têm substituta e deixe as demais em 404/410 (ou bloqueie o diretório se não houver valor nenhum).
4. **noindex seletivo.** Busca interna (`/busca?q=termo-raro`), tags com um ou dois posts e filtros sem demanda ficam fora do índice, concentrando a atenção nas páginas que importam.
5. **"Rastreada, mas não indexada".** Geralmente é sinal de conteúdo fraco, duplicado ou pouco ligado internamente, não de problema técnico. A solução é melhorar o conteúdo, juntar páginas parecidas e reforçar os links internos.

## Formato da resposta

Para diagnóstico: lista de problemas encontrados, com evidência (relatório ou URL de exemplo), impacto, correção e prioridade. Para implementação: os trechos prontos (robots.txt, tags, regras de redirecionamento, sitemap) e como validar cada mudança no Search Console.

---

## Créditos e licença

Skill adaptada a partir da skill `indexing-strategy` (versão 1.0.0), de autoria de **Cafe Code AI**, publicada em [kursku/skills](https://github.com/kursku/skills) (pasta `packs/kit-510-ptbr/05-seo-busca/indexing-strategy`). A licença **MIT** está declarada nos metadados da skill original; o repositório não traz um arquivo de licença próprio.

Alterações desta versão: remoção de recomendações desatualizadas ou incorretas: ferramenta de parâmetros de URL do Search Console (desativada em 2022), teste de compatibilidade com dispositivos móveis (desativado em 2023), `nofollow` em links internos para economizar rastreamento, uso de `priority` e `changefreq` nos sitemaps (ignorados pelo Google), relação entre LCP e volume de rastreamento (sem fonte; o fator é o tempo de resposta do servidor) e "penalidade" por conteúdo duplicado; nomes dos motivos do relatório "Páginas" atualizados; seção de conceitos que geram confusão e tabela de quando usar cada ferramenta; inclusão de JavaScript, remoções, paginação e hreflang; exemplo de canonical com barra final consistente; métricas trocadas por indicadores verificáveis; inclusão das seções "Antes de começar" e "Formato da resposta"; checklist desmarcado para uso.

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
