---
name: seo-images
description: Converte imagens PNG/JPG para WebP, redimensiona, aplica lazy loading, cuida da imagem LCP e atualiza as referências no código para melhorar a velocidade (PageSpeed, Core Web Vitals). Use com /seo-images.
---

# Imagens em WebP e performance

## Passos
1. Inventário: liste todas as imagens em `public/`, `assets/`, `static/`,
   `src/` e CSS (`background-image`), com tamanho em KB e dimensões. Marque as
   que não são referenciadas em lugar nenhum: se forem importadas, vão para o
   build à toa; proponha remover (com confirmação).
2. Converta PNG/JPG para WebP (sharp, `cwebp` ou ImageMagick):
   - Fotos: qualidade 75–80.
   - Ilustrações/logos com transparência: WebP com alpha ou `lossless` se a
     qualidade cair.
   - Redimensione para no máximo 2x a largura exibida (ex.: exibida a 800px →
     máx. 1600px).
   - Logos simples: considere SVG em vez de WebP.
3. Mantenha os originais fora da pasta pública (ou apague só com confirmação
   do usuário).
4. Atualize todas as referências no código (HTML, JSX/TSX, CSS, PHP, JSON de
   CMS).
5. Se o framework tem componente de imagem (`next/image`, `astro:assets`,
   `nuxt/image`), use-o em vez de converter manualmente; ele já gera WebP/AVIF.
   Passe `widths`/`sizes` (ou equivalente) para gerar `srcset` responsivo; sem
   isso o celular baixa a versão de desktop.
6. Em HTML puro:
   ```html
   <img src="/img/foto.webp" width="800" height="600" alt="" loading="lazy" decoding="async">
   ```
   - Sempre `width` e `height` (evita layout shift/CLS).
   - `loading="lazy"` em tudo **exceto** o que aparece na primeira tela. A
     imagem principal (LCP) leva `fetchpriority="high"` e nunca lazy; logo do
     header também sem lazy.
   - `alt` descritivo; vazio apenas para imagens decorativas. Se o projeto
     guarda textos em dicionário/tradução, o `alt` vai para lá.
7. Imagem LCP em `background-image` do CSS não é descoberta cedo pelo
   navegador: troque por `<img>` ou adicione
   `<link rel="preload" as="image" href="..." fetchpriority="high">`.
8. Animação de entrada no hero: elemento que começa com `opacity: 0` não conta
   como LCP até aparecer e atrasa a métrica. Comece em `opacity: 0.01` ou anime
   só `transform`.
9. Vídeos: `preload="none"` (ou `metadata`) e `poster` em WebP; vídeo de fundo
   sem som com `muted playsinline` e em resolução compatível com a tela.
10. Exceções: favicons e `og:image` ficam em PNG/JPG (compatibilidade com redes
    sociais e navegadores).

## Verificação
- Nenhuma referência quebrada (procure no código pelos nomes antigos).
- Tabela antes/depois com o total em KB.
- Sugira rodar PageSpeed Insights (https://pagespeed.web.dev) antes e depois,
  no mobile.

## Saída
Tabela: arquivo, tamanho antes, tamanho depois, economia total.
