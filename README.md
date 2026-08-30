# My portfolio

My personal site, built with [Hugo](https://gohugo.io). Lives at [robertscharlie.github.io/Charlie-Roberts-Blog](https://robertscharlie.github.io/Charlie-Roberts-Blog/).

## Running it locally

Needs the **extended** version of Hugo:

- macOS: `brew install hugo`
- Windows: `winget install Hugo.Hugo.Extended`
- Linux: see the [install docs](https://gohugo.io/installation/linux/)

Then from this folder:

```
hugo server -D
```

Open the URL it prints (usually `http://localhost:1313`), and it live-reloads as I edit. `-D` includes drafts.

In VS Code, `Ctrl+Shift+B` (`Cmd+Shift+B` on Mac) runs the same thing as the default build task, and there's a "Hugo: New project" task for the command below.

## Adding a project

```
hugo new projects/my-new-project.md
```

Fill in the front matter, write the body, set `draft: false`, then push. It goes live on its own.

```yaml
---
title: "My Project"
date: 2026-08-22
draft: false
summary: "One sentence for the card preview."
tags: ["electronics"]
image: "images/projects/my-project/cover.jpg"   # optional, leave blank for a placeholder slot
category: "Electronics"
highlights:
  - "First bullet for the card"
math: false
---
```

Images live under `static/images/projects/<project-name>/`.

## Deploying

Pushing to `main` triggers `.github/workflows/hugo.yml`, which builds with Hugo and deploys straight to GitHub Pages. No manual step. If a build ever goes red, check the Actions tab.

## Profile picture

The hero shows a placeholder circle until there's a real photo. To set one: drop it in `static/images/`, then set `profilePicture = "images/whatever.jpg"` under `[params]` in `hugo.toml`.

## CV

The footer's CV link only shows up once `static/cv.pdf` actually exists. Drop a PDF there and it appears automatically.

## Custom domain

Live at `charliejfroberts.com`, DNS managed on Cloudflare.

1. `static/CNAME` contains the bare domain (no `https://`) — Hugo copies it to `public/CNAME` on every build, which is what keeps GitHub's custom-domain setting from reverting on the next deploy
2. DNS on Cloudflare, apex domain, records set to **DNS only** (grey cloud, not proxied) so GitHub can issue its own TLS cert:
   - Four `A` records at `@` to GitHub Pages' IPs: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - Optionally four `AAAA` records at `@` for IPv6: `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`
   - A `CNAME` record at `www` pointing to `robertscharlie.github.io` if `www.charliejfroberts.com` should also resolve
3. Repo Settings → Pages → Custom domain → `charliejfroberts.com` → save, wait for the DNS check to pass, then tick "Enforce HTTPS"
4. `baseURL` in `hugo.toml` matches for local builds; the deploy workflow overrides it at build time with whatever domain is set in step 3, so that's the actual source of truth in production

## Structure

```
content/projects/    each project is one .md file
content/about.md     the bio shown in the hero
layouts/              Go templates
static/css/main.css  the whole stylesheet (colours and fonts are CSS variables at the top)
static/images/        project photos
static/videos/         project video clips, embedded with the {{< video >}} shortcode
static/files/         PDFs linked from project pages
.github/workflows/    build + deploy
```
