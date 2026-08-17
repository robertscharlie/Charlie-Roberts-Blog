# EE project portfolio (Hugo)

A static site scaffold for a project portfolio, built with [Hugo](https://gohugo.io). Deploys for free on GitHub Pages, with room for a custom domain.

Design: a "schematic paper" theme — grid-paper background, a copper trace accent, and a hero navigation element drawn as a circuit trace connecting to Projects / About. Section headings use real EE reference designators (U1, GND) instead of generic icons.

## 1. Install Hugo locally

You need the **extended** version (for the built-in SCSS/asset pipeline support).

- macOS: `brew install hugo`
- Windows: `choco install hugo-extended` or `winget install Hugo.Hugo.Extended`
- Linux: see the [Hugo install docs](https://gohugo.io/installation/linux/) — or download the `_extended_` `.deb`/`.rpm`/`.tar.gz` from the [releases page](https://github.com/gohugoio/hugo/releases)

Check it worked:

```
hugo version
```

## 2. Run it locally

From this folder:

```
hugo server -D
```

Open the URL it prints (usually `http://localhost:1313`). `-D` includes draft posts (front matter `draft: true`) so you can preview unpublished work. Leave it running — it live-reloads as you edit.

## 3. Add content

New project:

```
hugo new projects/my-project.md
```

This uses `archetypes/default.md`. Fill in the front matter at the top of the new file:

```yaml
---
title: "My Project Title"
date: 2026-08-17
draft: false          # set false to publish
summary: "One sentence for the card preview."
tags: ["circuits"]
designator: "U1"      # shown as the reference-designator badge
math: false            # set true to load MathJax on this page
---
```

## 4. Put it on GitHub

```
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/yourusername/yourrepo.git
git push -u origin main
```

Then in the repo on GitHub: **Settings → Pages → Build and deployment → Source → GitHub Actions**. The included workflow (`.github/workflows/hugo.yml`) builds the site with Hugo and deploys it automatically on every push to `main`.

## 5. Add your custom domain

1. Edit `static/CNAME` and replace `yourdomain.com` with your actual domain (just the bare domain, no `https://`).
2. In your domain registrar's DNS settings:
   - **Apex domain** (`yourdomain.com`): add four `A` records pointing at GitHub Pages' IPs:
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```
   - **Subdomain** (`www.yourdomain.com` or `blog.yourdomain.com`): add a `CNAME` record pointing to `yourusername.github.io`.
   - If you're using Cloudflare as your registrar/DNS, set those records to "DNS only" (grey cloud) at first — you can turn proxying back on once Pages confirms the domain.
3. Back in **Settings → Pages**, enter the domain under "Custom domain" and save. Once DNS propagates (minutes to a few hours), tick **Enforce HTTPS** — GitHub provisions a free SSL certificate automatically.
4. Update `baseURL` in `hugo.toml` to your real domain, e.g. `baseURL = "https://yourdomain.com/"`.

## Structure

```
content/
  projects/      # portfolio projects
  about.md
layouts/         # HTML templates (Go templates)
static/css/      # stylesheet, CNAME
.github/workflows/hugo.yml   # build + deploy on push
```

## Customising

- Colours, type, and spacing are all CSS custom properties at the top of `static/css/main.css` — change the palette there and it cascades everywhere.
- Site title, tagline, nav, and description live in `hugo.toml`.
- The hero circuit-trace graphic is inline SVG in `layouts/partials/hero.html` — it's just three nodes on a bus line, easy to relabel or extend if you add more sections.
