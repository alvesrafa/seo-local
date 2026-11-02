---
name: seo-plan
description: Faz uma varredura de SEO técnico e local no site (código e/ou URL publicada), pontua favicon, meta tags, JSON-LD, sitemap/robots/llms.txt, Search Console e imagens, e aciona as skills correspondentes para corrigir. Use com /seo-plan, ou quando pedirem auditoria, checklist ou plano de SEO (SEO audit, SEO plan).
---

# Plano de SEO local

Esta é a skill de entrada do pacote. Ela diagnostica, pede aprovação e chama as
demais na ordem certa. As skills são referenciadas pelo nome (`seo-images`,
`seo-favicon`, `seo-meta`, `seo-schema`, `seo-files`, `seo-console`). Instaladas
como plugin, elas aparecem com prefixo (`/seo-local:seo-images`); é a mesma
skill.

## Modo de operação
- **Código**: se houver projeto aberto, analise os arquivos.
- **URL**: se o usuário passar uma URL, busque o HTML publicado (WebFetch ou
  `curl -sL`) e os arquivos da raiz: `/robots.txt`, o sitemap indicado nele,
  `/llms.txt`, `/favicon.ico`, `/site.webmanifest`. Confira também se
  `http://` redireciona para `https://` e se `www` e sem `www` caem na mesma
  versão.
- Se ambos estiverem disponíveis, use os dois: o código mostra a intenção, a
  URL mostra o que foi publicado.
- Site em mais de um idioma: faça as checagens em cada versão.

## Checagens e pontuação

Cada item vale até 10 pontos. Total máximo 60.

| Item | Skill | Critérios |
|---|---|---|
| Favicon | `seo-favicon` | `favicon.ico` na raiz, PNG 32px, apple-touch-icon 180px sem transparência, manifest com ícones 192/512, sem favicon padrão do template |
| Meta tags | `seo-meta` | title e description únicos e no tamanho, canonical absoluto, `lang` correto, viewport, OG completo (imagem 1200x630 com width/height/alt), twitter card, `hreflang` se multilíngue, nenhum `noindex` acidental |
| JSON-LD | `seo-schema` | subtipo de LocalBusiness, nome/telefone/endereço/horário, serviços, FAQPage se houver FAQ visível, JSON válido, dados batem com a página |
| Arquivos | `seo-files` | sitemap válido sem URLs quebradas, robots sem `Disallow: /` e com `Sitemap:` apontando para o arquivo que existe, llms.txt com links Markdown |
| Search Console | `seo-console` | método de verificação presente (GA4, GTM ou meta tag), HTTPS no domínio final, sitemap acessível (o cadastro em si não é verificável daqui; pergunte ao usuário) |
| Imagens | `seo-images` | sem PNG/JPG pesados (> 200 KB) no conteúdo, `width`/`height` presentes, lazy fora da primeira tela, imagem LCP sem lazy e visível desde o primeiro quadro |

**Nota de cada item:** `10 × critérios atendidos ÷ critérios aplicáveis`,
arredondado. Critério que não se aplica (ex.: `hreflang` em site de um idioma,
FAQPage sem FAQ) sai da conta. Item inexistente vale 0. Assim duas rodadas no
mesmo site dão a mesma nota.

## Relatório
Entregue antes de alterar qualquer coisa:

```
Pontuação: 34/60

| Item          | Nota | Problemas                                  | Ação        |
|---------------|------|--------------------------------------------|-------------|
| Imagens       | 2    | 14 PNGs somando 6,2 MB; hero com lazy      | seo-images  |
| JSON-LD       | 0    | ausente                                    | seo-schema  |
| ...           |      |                                            |             |

Fora do site (não pontua): Google Business Profile, Bing Webmaster Tools
```

Ordene por prioridade: maior impacto e menor nota primeiro. Ordem padrão de
impacto: imagens (velocidade pesa no ranking e na conversão, principalmente no
celular) → meta tags → JSON-LD → arquivos → Search Console → favicon.

### Fora do site
Não dá para checar pelo código, mas para negócio local pesa mais que qualquer
item acima. Pergunte e inclua no relatório como recomendação:
- **Google Business Profile** criado, verificado, com categoria certa, horário,
  fotos e o link do site. É ele que aparece no mapa e no "perto de mim".
- **NAP consistente**: nome, endereço e telefone escritos igual no site, no
  Business Profile, no Instagram e em diretórios.
- **Bing Webmaster Tools**: importa a propriedade do Search Console em dois
  cliques e alimenta as buscas do ChatGPT, do Copilot e do DuckDuckGo.

## Execução
1. Pergunte ao usuário quais itens corrigir (padrão: todos com nota < 10).
2. Antes de executar, colete de uma vez os dados que várias skills precisam
   (nome, tipo de negócio, serviço, cidade, telefone, endereço, horário,
   domínio, idiomas, ID do GA) para não perguntar repetidamente.
3. Execute as skills na ordem: `seo-images` → `seo-favicon` → `seo-meta` →
   `seo-schema` → `seo-files` → `seo-console`. Esta ordem evita retrabalho
   (ex.: og:image e sitemap já com nomes finais).
4. Para cada skill, siga o SKILL.md dela integralmente.
5. Se o projeto tem comandos de verificação (lint, typecheck, build, testes,
   ou os listados no CLAUDE.md/AGENTS.md), rode depois de cada skill.
6. Ao final, rode as checagens novamente e mostre a pontuação antes/depois.

## Regras
- Não altere nada sem o relatório aprovado.
- Não invente dados do negócio; pergunte.
- Siga as convenções do projeto (i18n, componentes, tokens de design). Se o
  projeto guarda textos em dicionário, títulos, descrições e `alt` vão para lá.
- Se uma skill não puder ser concluída (ex.: falta domínio), registre como
  pendência e siga para a próxima.
