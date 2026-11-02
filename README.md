# seo-local

**🇧🇷 Português** · [🇺🇸 English](#english)

Skills de agente para colocar o SEO técnico e local de um site em ordem: o
agente lê o código (ou a URL publicada), dá uma nota de 0 a 60, mostra o que
está faltando e corrige item por item, sem inventar dados do negócio.

Feito para sites de pequenos negócios: clínica, escritório, restaurante,
pousada, agência de passeios, loja. Funciona com qualquer stack (Astro,
Next.js, Nuxt, WordPress, HTML puro...) no Claude Code e em outros agentes que
leem `SKILL.md`.

## Comece pelo `/seo-plan`

Você não precisa decorar as outras skills. O `/seo-plan` faz a varredura,
entrega o relatório e chama as demais na ordem certa:

```
> /seo-plan

Pontuação: 34/60

| Item           | Nota | Problemas                                 | Ação        |
|----------------|------|-------------------------------------------|-------------|
| Imagens        | 2    | 14 PNGs somando 6,2 MB; hero com lazy     | seo-images  |
| JSON-LD        | 0    | ausente                                   | seo-schema  |
| Meta tags      | 6    | sem og:image, description duplicada       | seo-meta    |
| Arquivos       | 5    | robots aponta para sitemap inexistente    | seo-files   |
| Search Console | 5    | sem método de verificação                 | seo-console |
| Favicon        | 3    | favicon padrão do Vite                    | seo-favicon |

Fora do site (não pontua): Google Business Profile, Bing Webmaster Tools

Quais itens corrijo? (padrão: todos abaixo de 10)
```

Ele não altera nada antes de você aprovar o relatório, pergunta de uma vez
os dados que faltam (nome, telefone, endereço, horário, domínio) e, no fim,
roda as checagens de novo e mostra a nota antes/depois.

## As skills

| Skill | O que faz |
|---|---|
| `/seo-plan` | **Ponto de entrada.** Diagnóstico com nota, relatório e execução das outras na ordem certa |
| `/seo-images` | Converte PNG/JPG para WebP, redimensiona, lazy loading, imagem LCP e vídeos |
| `/seo-favicon` | Gera `.ico`, PNGs, apple-touch-icon, manifest e as tags do `<head>` a partir de um PNG/SVG |
| `/seo-meta` | Title, description, canonical, `hreflang`, Open Graph e Twitter Card (preview no WhatsApp/Instagram) |
| `/seo-schema` | Dados estruturados `LocalBusiness` (ou subtipo), serviços, preços, horário, `WebSite` e FAQ |
| `/seo-files` | `sitemap.xml`, `robots.txt` (com escolha consciente sobre bots de IA) e `llms.txt` no formato llmstxt.org |
| `/seo-console` | Prepara a verificação (GA4, GTM ou meta tag) e guia o cadastro no Google Search Console e no Bing |

Cada uma também funciona sozinha: `/seo-schema` num site que já está bem no
resto, por exemplo.

## Instalação

### Claude Code (plugin, recomendado)

Dentro do Claude Code:

```
/plugin marketplace add alvesrafa/seo-local
/plugin install seo-local@seo-local
```

Instalado como plugin, os comandos ganham prefixo: `/seo-local:seo-plan`,
`/seo-local:seo-schema` etc. Para atualizar: `/plugin marketplace update seo-local`.

### Claude Code (cópia manual)

Sem prefixo nos comandos. Para todos os seus projetos:

```bash
git clone https://github.com/alvesrafa/seo-local.git
cp -r seo-local/skills/* ~/.claude/skills/
```

Só para um projeto, copie para `.claude/skills/` dentro dele.

### Outros agentes

As skills seguem o formato aberto `SKILL.md` (pasta com frontmatter `name` e
`description`). Em agentes compatíveis, copie as pastas de `skills/` para o
diretório de skills do agente.

## Como usar

1. Abra o agente na pasta do projeto do site.
2. Rode `/seo-plan`. Se o site já está no ar, passe a URL também:
   `/seo-plan https://meusite.com.br`. Com código e URL, ele compara o que
   você escreveu com o que foi publicado.
3. Leia o relatório e diga o que corrigir.
4. Responda as perguntas sobre o negócio. Ele nunca inventa telefone,
   endereço, horário, preço ou avaliação.
5. Publique o site e siga o passo a passo final do Search Console (essa parte
   precisa do seu login no Google).

Dica: se o projeto tem um `CLAUDE.md` ou `AGENTS.md` com regras (i18n,
componentes, comandos de build), as skills seguem essas regras e rodam as
verificações do projeto depois de cada etapa.

## O que fica de fora

- Conteúdo e palavras-chave (o que escrever na página).
- Link building e autoridade de domínio.
- Google Business Profile: o `/seo-plan` lembra e explica, mas o cadastro é
  feito por você em https://business.google.com.

## Licença

MIT

---

<a id="english"></a>

## English

[🇧🇷 Português](#seo-local) · **🇺🇸 English**

Agent skills that put a website's technical and local SEO in order: the agent
reads the code (or the live URL), scores it from 0 to 60, shows what's missing
and fixes it item by item, without making up business data.

Built for small-business websites: clinics, law offices, restaurants, inns,
tour operators, shops. Works with any stack (Astro, Next.js, Nuxt, WordPress,
plain HTML...) in Claude Code and other agents that read `SKILL.md`.

> The skill instructions are written in Brazilian Portuguese and some defaults
> are Brazil-specific (`pt-BR`, BRL, LGPD, Registro.br). The agent follows
> them fine in any language and answers in yours; you can also ask it to adapt
> the defaults to your country.

### Start with `/seo-plan`

You don't need to learn the other skills. `/seo-plan` runs the audit, delivers
a scored report and calls the others in the right order. It changes nothing
until you approve the report, asks for missing data once (name, phone,
address, hours, domain), and at the end re-runs the checks to show the
before/after score.

### The skills

| Skill | What it does |
|---|---|
| `/seo-plan` | **Entry point.** Scored audit, report, and runs the others in order |
| `/seo-images` | Converts PNG/JPG to WebP, resizes, lazy loading, LCP image and videos |
| `/seo-favicon` | Generates `.ico`, PNGs, apple-touch-icon, manifest and `<head>` tags from a PNG/SVG |
| `/seo-meta` | Title, description, canonical, `hreflang`, Open Graph and Twitter Card (link previews) |
| `/seo-schema` | `LocalBusiness` (or subtype) structured data, services, prices, hours, `WebSite` and FAQ |
| `/seo-files` | `sitemap.xml`, `robots.txt` (with an informed choice about AI bots) and `llms.txt` per llmstxt.org |
| `/seo-console` | Prepares verification (GA4, GTM or meta tag) and walks you through Google Search Console and Bing |

Each one also works on its own.

### Install

**Claude Code plugin (recommended):**

```
/plugin marketplace add alvesrafa/seo-local
/plugin install seo-local@seo-local
```

As a plugin, commands are namespaced: `/seo-local:seo-plan`,
`/seo-local:seo-schema`, etc. Update with `/plugin marketplace update seo-local`.

**Manual copy (no namespace):**

```bash
git clone https://github.com/alvesrafa/seo-local.git
cp -r seo-local/skills/* ~/.claude/skills/   # all projects
# or copy into <project>/.claude/skills/ for a single project
```

**Other agents:** the skills use the open `SKILL.md` format; copy the folders
in `skills/` into your agent's skills directory.

### Usage

1. Open the agent in your website's project folder.
2. Run `/seo-plan`. If the site is live, pass the URL too:
   `/seo-plan https://example.com`.
3. Read the report and pick what to fix.
4. Answer the questions about the business. It never invents a phone number,
   address, hours, price or review.
5. Deploy, then follow the final Search Console steps (they need your Google
   login).

### Out of scope

Content and keyword strategy, link building, and the Google Business Profile
setup itself (`/seo-plan` reminds you about it).

### License

MIT
