---
name: seo-meta
description: Configura title, description, canonical, hreflang e Open Graph/Twitter Card (preview de compartilhamento no WhatsApp, Instagram, Facebook) em todas as páginas. Use com /seo-meta.
---

# Meta tags e Open Graph

## Entrada
- Nome do negócio, cidade/região e serviço principal. Se não estiverem no
  código, pergunte.
- Idiomas do site.
- Imagem de preview: se não existir, sugira criar uma 1200x630 (JPG, < 300 KB).

## Passos
1. Detecte o framework e onde o `<head>` é montado (layout global, `metadata`
   do Next.js, `useHead` do Nuxt, `<svelte:head>`, componente SEO do Astro,
   template PHP etc.). Prefira um componente único que todas as páginas usam.
2. Liste todas as páginas/rotas públicas.
3. Para cada página, defina:
   - `<title>`: até ~60 caracteres, único por página, formato
     `Serviço em Cidade | Nome`.
   - `<meta name="description">`: 120–160 caracteres, único, com chamada para
     ação.
   - `<link rel="canonical">` com URL absoluta, apontando para a própria página.
   - `<html lang="pt-BR">` (ou o idioma correto da página).
   - `<meta name="viewport" content="width=device-width, initial-scale=1">`.
   - `<meta name="robots" content="index, follow, max-image-preview:large">`
     nas páginas indexáveis. Páginas como "obrigado" e 404 levam
     `noindex, follow` e ficam sem canonical.
4. Site multilíngue: em cada versão, um `alternate` para cada idioma e um
   `x-default`, com URLs absolutas e recíprocas (a versão EN aponta para a PT
   e vice-versa):
   ```html
   <link rel="alternate" hreflang="pt-BR" href="https://dominio.com/">
   <link rel="alternate" hreflang="en" href="https://dominio.com/en/">
   <link rel="alternate" hreflang="x-default" href="https://dominio.com/">
   ```
5. Open Graph:
   ```html
   <meta property="og:type" content="website">
   <meta property="og:site_name" content="">
   <meta property="og:title" content="">
   <meta property="og:description" content="">
   <meta property="og:url" content="">
   <meta property="og:image" content="https://dominio.com/og.jpg">
   <meta property="og:image:width" content="1200">
   <meta property="og:image:height" content="630">
   <meta property="og:image:type" content="image/jpeg">
   <meta property="og:image:alt" content="">
   <meta property="og:locale" content="pt_BR">
   <meta property="og:locale:alternate" content="en_US">
   ```
   `width`/`height` fazem o preview aparecer já no primeiro compartilhamento;
   sem eles, WhatsApp e Facebook costumam mostrar o link sem imagem até o
   cache ser refeito.
6. Twitter/X:
   ```html
   <meta name="twitter:card" content="summary_large_image">
   <meta name="twitter:title" content="">
   <meta name="twitter:description" content="">
   <meta name="twitter:image" content="">
   ```
7. URLs de `og:image`, `canonical` e `hreflang` devem ser absolutas. Se o
   domínio final não for conhecido, use a configuração do framework
   (`site` no Astro, `metadataBase` no Next.js) ou uma variável `SITE_URL`, e
   avise.

## Regras
- Não duplique tags já existentes; substitua.
- Se o projeto guarda textos em arquivos de tradução/dicionário, title,
  description e `og:image:alt` vão para lá.
- Em SPA sem SSR/pré-render, avise que crawlers de redes sociais não executam
  JS e as tags precisam estar no HTML inicial.
- Procure `noindex` herdado de staging (meta tag, header `X-Robots-Tag` ou
  configuração do framework/CMS) e remova nas páginas que devem indexar.

## Verificação
- Cada página tem exatamente um `title`, uma `description` e um `canonical`.
- `hreflang` recíproco entre as versões, com `x-default`.
- Sugira testar em https://www.opengraph.xyz ou no depurador de
  compartilhamento do Facebook após publicar.

## Saída
Tabela com página, title e description definidos.
