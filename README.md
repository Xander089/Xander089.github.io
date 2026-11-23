# Xander089.github.io

Portfolio site, built with [Astro](https://astro.build) and deployed to GitHub Pages through GitHub Actions (`.github/workflows/deploy.yml`).

## Local development

```powershell
npm install --registry=https://registry.npmjs.org/
npm run dev      # http://localhost:4321
npm run build    # static output in dist/
```

On Windows you can also run `serve-page.cmd` from the parent folder.

## Deploy

One-time setup: **Settings → Pages → Source: GitHub Actions**. After that, every push to `main` publishes the site.
