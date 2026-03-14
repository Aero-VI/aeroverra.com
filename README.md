# aeroverra.com

The public-facing website for [Aeroverra](https://aeroverra.com) — an open-source, self-hosted software company building alternatives to big tech products.

## What This Is

A static single-page site. No framework, no build step, no dependencies beyond a browser. Hosted via GitHub Pages.

## Structure

```
aeroverra.com/
├── index.html   # Single-page site — all HTML, CSS, and content
├── CNAME        # GitHub Pages custom domain config
├── README.md    # This file
└── .gitignore
```

## Making Changes

This is a static site. Edit `index.html` directly.

For anything beyond trivial copy fixes:
1. Branch from `main` using the standard naming convention (`fix/`, `feature/`, `chore/`, `docs/`)
2. Make your changes
3. Test at desktop (≥1280px), tablet (768px), and mobile (≤375px) viewports
4. Open a PR — include before/after screenshots for visual changes
5. Get at least one approval before merging

See [Engineering Standards & Processes](../../memory/processes/engineering-process.md) for the full process.

## Deployment

Automatic. GitHub Pages deploys `main` branch on push. Changes are live within ~1 minute of merge.

Custom domain: `aeroverra.com` (configured via CNAME + DNS at registrar).

## Contributing

This repo is public. If you spot a bug or want to improve the site, open an issue or PR. Follow our standard PR process.

## License

© 2026 Aeroverra. All rights reserved.
