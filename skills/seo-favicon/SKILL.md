---
name: seo-favicon
description: Configura o favicon do site a partir de um PNG ou SVG (gera tamanhos, ícones Apple/Android, manifest e insere as tags no head), inclusive o ícone que aparece no resultado do Google. Use com /seo-favicon.
---

# Favicon

## Entrada
- PNG de origem, idealmente quadrado com no mínimo 512x512, ou um SVG. Se o
  usuário não indicar um, procure por `logo`, `icon` ou `favicon` no projeto e
  pergunte qual usar.
- Se o PNG não for quadrado ou tiver menos de 512px, avise antes de gerar.
- Logo horizontal (com nome escrito) fica ilegível a 16px: sugira usar só o
  símbolo/monograma.

## Passos
1. Detecte o framework e a pasta pública (`public/`, `static/`, `app/` no
   Next.js App Router, raiz em sites estáticos).
2. Gere a partir da origem (sharp, ImageMagick ou equivalente disponível):
   - `favicon.ico` com 16, 32 e 48px, **na raiz do site** (o Google e vários
     clientes pedem `/favicon.ico` direto)
   - `favicon-32x32.png`, `favicon-16x16.png`
   - `apple-touch-icon.png` 180x180 com fundo sólido (sem transparência)
   - `icon-192.png` e `icon-512.png`
   - Se a origem é SVG, copie também como `favicon.svg`.
3. Crie ou atualize `site.webmanifest` com `name`, `short_name`, `icons`,
   `theme_color` e `background_color` (cores tiradas do design do site).
4. Insira no `<head>` do layout global, sem duplicar tags existentes:
   ```html
   <link rel="icon" href="/favicon.ico" sizes="48x48">
   <link rel="icon" type="image/svg+xml" href="/favicon.svg"> <!-- só se houver SVG -->
   <link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
   <link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
   <link rel="apple-touch-icon" href="/apple-touch-icon.png">
   <link rel="manifest" href="/site.webmanifest">
   <meta name="theme-color" content="#000000">
   ```
   No Next.js App Router, prefira os arquivos convencionados (`app/icon.png`,
   `app/apple-icon.png`, `app/favicon.ico`) em vez de tags manuais.
5. Remova favicons antigos ou padrão do template (ex.: `vite.svg`,
   `favicon.svg` do Astro, favicon do create-react-app) e as tags que apontam
   para eles.

## Verificação
- Todos os arquivos existem e respondem 200 nos caminhos referenciados.
- Apenas um conjunto de tags de ícone no HTML final.
- O favicon não está bloqueado no `robots.txt` (o Google precisa rastreá-lo
  para mostrá-lo no resultado de busca; a troca lá pode levar semanas).

## Saída
Liste os arquivos criados e as tags inseridas.
