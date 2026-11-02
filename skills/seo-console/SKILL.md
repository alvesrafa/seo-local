---
name: seo-console
description: Guia o cadastro do site no Google Search Console (verificação via Google Analytics, Tag Manager ou meta tag), o envio do sitemap e a importação no Bing Webmaster Tools. Use com /seo-console.
---

# Google Search Console

Esta skill não consegue concluir sozinha: a verificação exige login na conta
Google do dono. O agente prepara e confere o site; o usuário faz os cliques.

## Pré-requisitos (o agente verifica no código)
1. Site publicado no domínio final, com HTTPS.
2. Um método de verificação no `<head>` de todas as páginas. Pergunte qual o
   usuário prefere:
   - **Google Analytics 4** (se ele quer medir visitas): tag `gtag.js` com
     `G-XXXXXXX`. Se não estiver, peça o ID de medição e insira:
     ```html
     <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXX"></script>
     <script>
       window.dataLayer = window.dataLayer || [];
       function gtag(){dataLayer.push(arguments);}
       gtag('js', new Date());
       gtag('config', 'G-XXXXXXX');
     </script>
     ```
     O GA usa cookies: avise sobre aviso de cookies/consentimento (LGPD).
   - **Google Tag Manager**, se o site já usa: verificação pela opção "Google
     Tag Manager".
   - **Meta tag HTML** (sem script nem cookies): o Search Console fornece
     `<meta name="google-site-verification" content="...">`. O usuário copia o
     código no passo 3 abaixo e o agente insere no `<head>`.
3. Sitemap acessível (se não existir, rode a skill `seo-files` antes).

## Passo a passo para o usuário
1. Acesse https://search.google.com/search-console. Se for verificar pelo GA,
   use **o mesmo e-mail que é administrador/editor da propriedade no Google
   Analytics**.
2. Adicionar propriedade:
   - **Domínio** (esquerda): cobre todos os subdomínios e protocolos, mas só
     verifica por registro TXT no DNS. Boa opção se o usuário tem acesso ao
     painel do domínio (Registro.br, Cloudflare etc.).
   - **Prefixo do URL** (direita): permite verificar por GA, GTM ou meta tag.
     Informe `https://dominio.com/`.
3. Em "Outros métodos de verificação", escolha o método preparado e clique em
   Verificar. Na meta tag, publique o site com a tag antes de clicar.
4. Após verificar, vá em **Sitemaps** e envie a URL do sitemap (a mesma da
   linha `Sitemap:` do robots.txt).
5. Em **Inspeção de URL**, inspecione a home e clique em "Solicitar
   indexação".

## Bing Webmaster Tools
Recomende logo em seguida: https://www.bing.com/webmasters → "Importar do
Google Search Console". Leva dois minutos e o índice do Bing alimenta as
buscas do ChatGPT, do Copilot e do DuckDuckGo.

## Problemas comuns
- Falha na verificação: tag fora do `<head>`, carregada via JS tardio, site
  não republicado depois de inserir a tag, ou e-mail sem permissão de edição
  no GA.
- Variação `www` vs. sem `www`: cadastre a versão canônica (a que o site
  redireciona).
- Não remova a tag nem o registro DNS depois: a verificação é revalidada
  periodicamente.

## Saída
Informe o status dos pré-requisitos e entregue o passo a passo.
