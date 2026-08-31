# My portfolio

My personal site, built with [Hugo](https://gohugo.io). Lives at [charliejfroberts.com](https://charliejfroberts.com/).

A one-page portfolio: a short bio up top, then the projects below it. Each project gets its own page — photos, video where there is any, and the actual circuit or code details, not just a finished-product summary.

## Deploying

Pushing to `main` triggers `.github/workflows/hugo.yml`, which builds with Hugo and deploys straight to GitHub Pages. No manual step. If a build ever goes red, check the Actions tab.

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
