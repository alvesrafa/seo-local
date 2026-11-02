---
name: seo-files
description: Gera sitemap.xml, robots.txt e llms.txt (no formato llmstxt.org) para buscadores e agentes de IA. Use com /seo-files.
---

# sitemap.xml, robots.txt e llms.txt

## Entrada
- Domínio final (com https). Se desconhecido, use a configuração do framework
  ou uma variável `SITE_URL` e avise.
- Idiomas do site.

## Passos

### sitemap.xml
1. Liste todas as rotas públicas indexáveis (exclua admin, login, obrigado,
   404, rotas com `noindex`).
2. Prefira a geração nativa do framework (`app/sitemap.ts` no Next.js,
   `@astrojs/sitemap`, `@nuxtjs/sitemap`, plugin do WordPress). Em site
   estático, gere o arquivo na pasta pública. Anote o nome real do arquivo
   gerado (o `@astrojs/sitemap` gera `sitemap-index.xml`, não `sitemap.xml`).
3. Formato:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
     <url>
       <loc>https://dominio.com/</loc>
       <lastmod>2026-01-01</lastmod>
     </url>
   </urlset>
   ```
   - `lastmod` só com a data real da última mudança; data inventada ou sempre
     "hoje" faz o Google ignorar o campo.
   - `priority` e `changefreq` são ignorados pelo Google; não precisam ser
     incluídos.
   - Site multilíngue: cada URL lista suas versões com
     `<xhtml:link rel="alternate" hreflang="..." href="..."/>` (os plugins de
     sitemap fazem isso quando configurados com os idiomas).

### robots.txt
```
User-agent: *
Allow: /
Disallow: /admin/

Sitemap: https://dominio.com/sitemap.xml
```
- A linha `Sitemap:` aponta para o arquivo que existe de verdade (confira o
  nome gerado no passo anterior).
- Verifique se não existe `Disallow: /` herdado de ambiente de staging.
- Pergunte se o usuário quer bloquear crawlers de IA. Por padrão, deixe tudo
  liberado. Se quiser bloquear, separe os dois tipos:
  - **Treino** (bloquear não tira o site das respostas): `GPTBot`,
    `ClaudeBot`, `Google-Extended`, `CCBot`, `Applebot-Extended`.
  - **Busca e citação** (bloquear tira o site das respostas do ChatGPT,
    Claude e Perplexity): `OAI-SearchBot`, `ChatGPT-User`, `Claude-SearchBot`,
    `Claude-User`, `PerplexityBot`. Para negócio local, mantenha liberado.

### llms.txt
Arquivo Markdown na raiz descrevendo o negócio para agentes de IA, no formato
de https://llmstxt.org: um H1, um resumo em citação, texto livre opcional e
seções H2 com listas de links `- [nome](url): nota`.

```
# Nome do Negócio

> Resumo de uma ou duas frases: o que faz, para quem e onde.

Telefone (11) 99999-9999. Rua Exemplo, 123, Bairro, Cidade/UF.
Atendimento de segunda a sexta, 8h às 18h.

## Serviços
- [Serviço A](https://dominio.com/servico-a): descrição curta, preço se publicado

## Contato
- [WhatsApp](https://wa.me/5511999999999): (11) 99999-9999
- [Instagram](https://instagram.com/perfil): @perfil

## Páginas principais
- [Sobre](https://dominio.com/sobre)
- [Perguntas frequentes](https://dominio.com/faq)
```

- Informação solta (telefone, endereço, horário) vai no texto antes das
  seções H2; dentro das seções, prefira itens com link. O Lighthouse
  (auditoria Agentic Browsing) reprova `llms.txt` com URLs soltas em vez de
  links Markdown.
- Se o framework permite, gere o arquivo a partir do mesmo conteúdo da página
  (rota/endpoint que lê os dados ou traduções), em vez de um `.txt` estático
  que fica desatualizado.
- Site multilíngue: um arquivo por idioma (`/llms.txt`, `/en/llms.txt`), cada
  um com link para o outro.
- Opcional: anuncie no `<head>`
  `<link rel="alternate" type="text/markdown" href="/llms.txt" title="llms.txt">`
  e no fim do `robots.txt` um comentário `# /llms.txt`.

Aviso ao usuário: `llms.txt` é uma proposta de padrão sem adoção oficial
confirmada pelos grandes buscadores. Custo baixo, benefício incerto.

## Verificação
- Os três arquivos respondem 200 na raiz do domínio (no build, existem na
  pasta de saída).
- Todas as URLs do sitemap respondem 200 e não têm `noindex`.
- O `canonical` de cada página bate com a URL no sitemap.

## Saída
Liste os arquivos criados e o número de URLs no sitemap.
