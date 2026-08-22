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

## Custom domain (not set up yet)

If I buy one later:

1. Add a `static/CNAME` file containing the bare domain (no `https://`)
2. DNS at the registrar: four `A` records to GitHub Pages' IPs (`185.199.108.153`, `.109.153`, `.110.153`, `.111.153`) for the apex domain, or a `CNAME` record to `robertscharlie.github.io` for a subdomain
3. Settings → Pages → enter the domain, wait for it to verify, tick "Enforce HTTPS"
4. Update `baseURL` in `hugo.toml` to match

## Structure

```
content/projects/    each project is one .md file
content/about.md     the bio shown in the hero
layouts/              Go templates
static/css/main.css  the whole stylesheet (colours and fonts are CSS variables at the top)
static/images/        project photos
static/files/         PDFs linked from project pages
.github/workflows/    build + deploy
```
