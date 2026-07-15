# CLAUDE.md

Personal website of Adomas Valantinas — live at https://avalantinas.com via
GitHub Pages (repo `euchas/euchas.github.io`, branch `main`, served from root).

**Read `README.md` first** — it is the complete project log: architecture, deploy
flow, Porkbun DNS records, HTTPS troubleshooting, image-optimization recipe, and
the history of the 31 MB bundle this site was reconstructed from.

Ground rules:
- `index.html` is the single source of truth. Never regenerate from
  `Adomas Valantinas - Website (standalone).html` (gitignored 31 MB bundle) —
  it's only kept as raw material. `old/` is also gitignored archive material.
- Deploy = commit + push to `main`. Nothing else. Keep total site weight ~1 MB;
  convert new images to WebP at ~2× display size before adding (recipe in README).
- Do not delete `CNAME`, `robots.txt`, or `sitemap.xml`.
- Do not write the user's email address in plain text anywhere in the repo
  (anti-scraper; it's assembled by JS at the bottom of index.html).
- The user is not a web developer — explain changes plainly.
