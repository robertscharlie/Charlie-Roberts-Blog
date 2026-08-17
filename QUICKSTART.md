# Quickstart (VS Code)

## First time setup

1. **Install Hugo** — `brew install hugo` (Mac) / `winget install Hugo.Hugo.Extended` (Windows) / see [Linux docs](https://gohugo.io/installation/linux/). Confirm with `hugo version`.
2. **Open this folder in VS Code** — `File → Open Folder`. When prompted, install the recommended extensions (or open the Extensions panel and search `@recommended`).
3. **Preview the site** — press `Ctrl+Shift+B` / `Cmd+Shift+B` (runs the "Hugo: Preview site" task), or type `hugo server -D` in the integrated terminal. Open the localhost URL it prints.
4. **Push to GitHub** — use the Source Control panel in VS Code (stage → commit → push), or the terminal:
   ```
   git init
   git add .
   git commit -m "Initial site"
   git remote add origin <your repo URL>
   git push -u origin main
   ```
5. **Turn on GitHub Pages** — in the repo on GitHub: `Settings → Pages → Build and deployment → Source → GitHub Actions`. The included workflow builds and deploys automatically on every push.
6. **Connect your domain** — edit `static/CNAME` with your domain, add the DNS records in the main `README.md`, then set the custom domain under `Settings → Pages` and enable HTTPS.

## Adding a new project (ongoing)

Run the **"Hugo: New project"** task (`Terminal → Run Task…`), or:
```
hugo new projects/my-new-project.md
```
Fill in the front matter (`title`, `date`, `summary`, `designator`), write the body, set `draft: false`, then commit and push. It appears on the site automatically, styled the same as everything else.

See `README.md` for the full reference (DNS details, customising colours/type, project structure).
