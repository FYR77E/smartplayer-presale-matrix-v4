# smartplayer-presale-matrix-v4

Static presale matrix site for SmartPlayer. This repo is intentionally simple: one main `index.html`, SEO assets, robots/sitemap, and no build step.

## Start Here

- `index.html` — the whole application: HTML, CSS, data, state, render logic, command palette, compare view.
- `robots.txt` / `sitemap.xml` — crawl configuration.
- `og-image.png` / `og-image.svg` — social preview assets.
- `../_meta/wiki/smartplayer-presale-matrix-v4.md` — Obsidian project note.
- `../_meta/wiki/presale-matrix-refactor-plan.md` — safe refactor plan.
- `../_meta/wiki/smartplayer-wiki-facts.md` — source facts for platforms/modules.
- `../_meta/wiki/wiki-recheck-2026-04-25.md` — fact recheck history.

## How To Work Safely

1. Read the relevant wiki note before changing platform facts.
2. Do not mix fact-fix and refactor in the same commit.
3. For data changes, update wiki/output notes if the source or interpretation changed.
4. For UI changes, smoke-test Dashboard, Matrix, Compare, Platforms, command palette, details panel, print/PDF.
5. Keep the site static unless there is an explicit architecture decision to add a build step.

## Local Checks

There is no package script yet. Use a cheap syntax check for inline JS/JSON-LD:

```bash
node -e "const fs=require('fs');const html=fs.readFileSync('index.html','utf8');const scripts=[...html.matchAll(/<script([^>]*)>([\\s\\S]*?)<\\/script>/gi)];scripts.forEach((m,i)=>{/application\\/ld\\+json/.test(m[1])?JSON.parse(m[2]):new Function(m[2]);console.log('script',i+1,'ok')})"
```

## Refactor Boundary

Current known debt: `index.html` is a single-file app. The approved safe direction is incremental extraction:

1. data namespace, no behavior change;
2. pure helper functions;
3. render modules;
4. optional build step only after the static contract is stable.

Do not start with a framework rewrite.
