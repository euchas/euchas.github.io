# avalantinas.com — personal website of Adomas Valantinas

Single-page static site for Adomas Valantinas, planetary scientist (Mars research:
water–rock interactions, Perseverance/TGO missions; also DJ "Teller of Blue").
Live at **https://avalantinas.com**. This file is the complete project log — enough
for any agent or human to pick up where things left off.

## Architecture

- **Pure static site, no build step, no framework.** Four pages sharing
  `assets/styles.css` and a common nav (tab pills, `.active` marks current page)
  + footer — edit nav/footer in ALL four files when changing them:
  - `index.html` — landing: hero (with station + CV buttons), about, contact
    (click-to-reveal email JS at the bottom)
  - `research.html` — approach panels, missions strip, selected publications
  - `press.html` — two press stories with outlet links
  - `radio.html` — Teller of Blue DJ page
- `cv.pdf` — the CV, linked prominently from nav on every page (stable URL
  avalantinas.com/cv.pdf; to update, overwrite this file). Split into pages
  2026-07-16 after colleague feedback (CV/affiliation findability, collaborative
  "we" wording for published work, tab navigation).
- `assets/images/` — WebP images sized ~2× display size. `assets/fonts/` — self-hosted
  woff2 (IBM Plex Mono 400/500, Space Grotesk 400–600; latin + latin-ext subsets).
- `favicon.svg`, `robots.txt`, `sitemap.xml` (all four pages), `CNAME` (contains
  `avalantinas.com`, required by GitHub Pages — do not delete).
- Total site weight ≈ 1.25 MB. Keep it that way: optimize any new image before adding.
- Deliberately NOT included yet: GitHub profile link (user said hold off).

## Hosting & deploy

- **GitHub Pages**, repo `euchas/euchas.github.io` (GitHub user `euchas`), branch `main`,
  served from repo root. Custom domain + enforced HTTPS configured in Pages settings.
- **Deploy = push to main.** Edit files → `git add -A && git commit && git push` →
  live in ~1 minute. Check build: `gh api repos/euchas/euchas.github.io/pages/builds/latest`.
- Git remotes: `origin` = GitHub (deploys), `gitlab` = old ETH GitLab mirror (unused).
- `gh` CLI installed at `C:\Program Files\GitHub CLI\gh.exe`, authenticated as `euchas`,
  wired as git credential helper (`gh auth setup-git` already run).

## Domain & DNS (Porkbun)

Domain **avalantinas.com** registered at porkbun.com. DNS records:

| Type | Host | Value | Purpose |
|---|---|---|---|
| A ×4 | (root) | 185.199.108.153 / .109.153 / .110.153 / .111.153 | GitHub Pages |
| CNAME | www | euchas.github.io | www redirect |
| MX ×2 | (root) | fwd1/fwd2.porkbun.com | Porkbun email forwarding — keep |
| TXT | (root) | v=spf1 include:_spf.porkbun.com ~all | email SPF — keep |
| TXT | (root) | google-site-verification=… | Search Console ownership |

HTTPS cert is auto-managed by GitHub (Let's Encrypt). **If cert ever gets stuck**
(https fails while http works): re-trigger with
`gh api repos/euchas/euchas.github.io/pages -X PUT -f cname=` then
`-f cname=avalantinas.com`, wait ~2 min, then re-enable
`-f https_enforced=true`. (This happened at initial setup because the domain was
configured before DNS pointed at GitHub.)

## Search / Google

- Google Search Console verified (DNS TXT method) on the user's Google account;
  `sitemap.xml` submitted and indexing requested on 2026-07-15. New domains take
  days to appear; check with a `site:avalantinas.com` search.
- If pages are added later, list them in `sitemap.xml` (update `<lastmod>`).

## History & source material (important)

- The site was reconstructed on 2026-07-15 from
  `Adomas Valantinas - Website (standalone).html` — a **31 MB single-file bundle**
  (Claude artifact export: base64 images in a `__bundler/manifest` JSON + React
  runtime). It was too big for GitHub's 25 MB web upload and slow to load.
  Images were extracted, EXIF-rotated where needed, resized, and converted to WebP
  (21 MB → ~1 MB); React was replaced with plain HTML/CSS/JS; mobile responsiveness,
  favicon, OG tags, sitemap were added.
- **`index.html` is now the single source of truth. Never regenerate from the bundle.**
- Gitignored, local-only: the 31 MB bundle (root) and `old/` (earlier drafts v1–v5,
  original full-resolution photos, CV pdf). Original images also live inside the bundle.

## Recipes

**Optimize a new image** (Python + Pillow are installed):
```python
from PIL import Image, ImageOps
im = ImageOps.exif_transpose(Image.open("photo.jpg")).convert("RGB")  # exif_transpose matters!
w = 1600  # ~2x the CSS display width
im = im.resize((w, round(im.height * w / im.width)), Image.LANCZOS)
im.save("assets/images/name.webp", "WEBP", quality=80, method=6)
```
Reference it with explicit `width`/`height` attributes (CSS already has `img { height: auto }`).

**Preview locally:** `python -m http.server 8613` in the repo, open http://127.0.0.1:8613.

**Contact email:** deliberately not written in plain text anywhere (anti-scraper);
it's assembled by the JS snippet at the bottom of `index.html`.

## Content notes

- Positions: SNSF Postdoc.mobility Fellow at ETH Zürich (current); Senior Fellow,
  LCLU, University of Cambridge from 09/2026 — the "INCOMING" block in About will
  need flipping to "NOW" around then.
- Sections: hero / About / Approach (3 photo panels + missions strip) / Selected
  Research (linked cards incl. Google Scholar) / Press (2 stories with outlet links) /
  Radio DJ (Teller of Blue: NTS.live, Radio Vilnius) / Contact.
