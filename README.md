# ranpaeximagri.com — FIX v2 (NOT yet deployed)

Built 2026-10-13 from the deployed v1 optimized copy
(`~/workspace/ranpaeximagri-optimized/site/`, WITH the H1 fixes).
**Nothing was deployed. The live site still runs v1. No Hostinger login was used.**
Deploy: upload the contents of `site/` (or `ranpaeximagri-v2-site.zip`, then extract)
into Hostinger `public_html`, overwriting. Back up `public_html` first.
After upload, re-run PageSpeed + check the new pages live.

## What v2 fixes (audit 2026-10-13: only 4 of 15 points passed)

| # | Point | v1 (live) | v2 |
|---|---|---|---|
| 1 | Sitemap | ❌ 404 | ✅ `sitemap.xml` — 11 URLs, lastmod 2026-10-13 |
| 2 | On-page SEO | ❌ partial (no canonical/OG) | ✅ canonical + OG + Twitter on all 15 pages |
| 3 | Google Search Console | ❌ no verification | ⚠️ See "Owner actions" — no code invented |
| 4 | AI integrated | ❌ nothing | ✅ `llms.txt` + LocalBusiness JSON-LD on every page |
| 5 | Privacy policy | ❌ 404 | ✅ `privacy.html` (new, site design) |
| 6 | Terms & Conditions | ❌ 404 | ✅ `terms.html` (new, site design) |
| 7 | Meta titles/descriptions | ✅ | ✅ kept; cart/checkout descriptions lengthened to 120–160 |
| 8 | Favicon | ✅ | ✅ unchanged (`images/favicon.png`) |
| 9 | robots.txt | ❌ 404 | ✅ `robots.txt` (Allow all + sitemap) |
| 10 | Compress images | ✅ WebP | ✅ unchanged |
| 11 | Mobile friendly | ✅ | ✅ unchanged |
| 12 | Custom 404 | ❌ generic Hostinger | ✅ `404.html` (branded) + `.htaccess` ErrorDocument |
| 13 | Broken links | ❌ 1 + fonts + map icon | ✅ 0 broken (see "Broken-link decisions") |
| 14 | Form validation | ❌ none | ✅ contact form: required/name/minlength + JS |
| 15 | Spam protection | ❌ none | ✅ honeypot + 3s submit-time check (see limits) |

SEO types: on-page ✅ (canonical/OG/JSON-LD/H1s/titles), technical ✅
(sitemap/robots/.htaccess redirects/noindex demo pages), local ✅
(LocalBusiness JSON-LD, Khammam added, address aligned to GBP),
off-page ⚠️ ongoing (cannot be built from the site alone — see below).

## New / changed files

- **NEW:** `sitemap.xml`, `robots.txt`, `llms.txt`, `.htaccess`,
  `privacy.html`, `terms.html`, `404.html`, `blog.html`, `shop.html`,
  `cart.html`, `checkout.html` (the last 4 were live on Hostinger but missing
  from the v1 mirror; they are now included so the package is complete).
- **ALL 15 HTML pages:** canonical + Open Graph + Twitter tags; LocalBusiness
  JSON-LD (homepage also WebSite schema); footer Blog → `blog.html`,
  Downloads → `certificates.html`; Privacy + Terms links in footer;
  address aligned to GBP
  ("Beside Lakshmi Srinivasa Theatre, Madhira, Khammam, Telangana, 507203, India");
  copyright → "© 2026 RANPA EXIM PVT LTD. All rights reserved."
- **contact.html:** form now has `name` attributes, `required`, `minlength`,
  honeypot field (`company_website`, must stay empty), JS validation with inline
  errors, 3-second submit-time check. On valid submit it opens WhatsApp
  (`wa.me/917382926533`) with the enquiry prefilled and shows a success message.
  No backend. True server-side CAPTCHA needs a backend — see "Limits".
- **blog.html:** title/description set; `blog-single.html` links → `blog.html`
  (that page does not exist); template `image_1..6.jpg` → existing `.webp`
  banners; search input gets `name="s"` + `required`.
- **shop.html / cart.html / checkout.html:** `<meta name="robots" content="noindex, follow">`,
  titles/descriptions fixed (they said "Vegefoods"), template product images
  mapped to real category `.webp` images, `bg_1.jpg` hero → `bgc1.webp`.
  These are leftover template demo pages (not a real store) — noindexed so
  they do not dilute search. Recommend deleting them on a later cleanup.
- **`.htaccess`:** `ErrorDocument 404 /404.html`; www → apex https 301;
  http → https 301; 1-week cache headers for webp/png/css/js, 0s for HTML.
- **`js/google-map.js`:** marker icon `images/loc.png` → `images/favicon.png`
  (`loc.png` never existed; the file is dead template code for a New York map).

## Broken-link decisions (all verified)

- `images/category-1.jpg` (about.html): the `<img>` sits inside an HTML
  **comment** (never rendered). Reference updated to
  `images/aboutusbanner.webp` so even the comment is clean.
- **Icon fonts:** the v1 deploy uploaded font files whose names literally
  contained `?` (wget artefacts, e.g. `icomoonccfb.woff?6tt51o`), so clean-URL
  requests 404'd. **Fix: renamed 11 font files to clean names**
  (`fonts/icomoon/*`, `fonts/ionicons/fonts/*`, `fonts/flaticon/font/*`,
  `fonts/open-iconic/*`). No CSS change needed — the CSS already requests the
  clean names (+ query strings, which browsers strip).
- `https://ranpaeximagri.com/Vegestables.html` (about.html, capital V +
  misspelling) → `vegetables.html` (Linux servers are case-sensitive).
- `images/bg_1.jpg` (blog/cart/checkout hero) → `images/bgc1.webp`.
- `images/ranpaeximlogo.png` (blog/shop/cart/checkout) → `images/ranpaeximlogo.webp`
  (the PNG was deleted in the v1 WebP deploy; only `favicon.png` remains as PNG).
- Shop/cart template `product-*.jpg` + the `ChatGPT Image ...png` → real
  category `.webp` images (pages are noindexed demo pages).

## Verification (`verify_v2.py` → `verification-report.json`)

- **0 broken internal refs** across all 15 pages + CSS + JS
  (external Unsplash/YouTube/hitwebcounter/Google URLs excluded as before).
- Every page: canonical ✅, OG 6/6 ✅, Twitter 4/4 ✅, JSON-LD parses with
  LocalBusiness ✅, exactly 1 real H1 (comments stripped) ✅, unique title
  ≤60 chars ✅, description 120–160 chars ✅, footer Privacy/Terms links ✅.
- Sitemap XML parses; all 11 URLs exist. `.htaccess` has 404 + www + https rules.
- Contact form: required ✅, name attrs ✅, honeypot ✅, WhatsApp JS ✅,
  3s check ✅, minlength ✅. Blog search: required ✅.
- Note: title lengths are counted on raw HTML, so `&amp;` counts as 5 chars;
  rendered titles are shorter and still ≤60.
- Local Lighthouse was unavailable in this environment (same as v1);
  re-check PageSpeed on the live URL after deploy.

## Owner actions (cannot be done from the site alone)

1. **Google Search Console:** no verification code was invented.
   Steps: search.google.com/search-console → Add property
   `https://ranpaeximagri.com/` → choose HTML-file verification →
   give the verification filename to whoever deploys (upload it to
   `public_html`) → Verify → Sitemaps → submit
   `https://ranpaeximagri.com/sitemap.xml`.
2. **Privacy / Terms:** both pages are **standard templates, not legal advice**.
   Have them reviewed before relying on them for a dispute.
3. **Off-page SEO (ongoing):** backlinks cannot be built from the site alone.
   Keep the Google Business Profile accurate (it lists this website), keep
   IndiaMART / directory listings consistent (same NAP), and earn mentions
   from buyer/supplier sites.
4. **Deploy note:** after upload, hard-refresh and open home, one category
   page, contact (test the form → WhatsApp), privacy, terms, and a bad URL
   (should show the branded 404). The `.htaccess` redirects need one live
   check: `http://www.ranpaeximagri.com/` → `https://ranpaeximagri.com/`.

## Files in this folder

- `site/` — the v2 site, ready to upload (contents → `public_html`).
- `ranpaeximagri-v2-site.zip` (4.6 MB, 154 files incl. `.htaccess`) — upload +
  extract in File Manager.
- `build_v2.py` — the script that built v2 (new pages are built once from
  about.html; do not re-run blindly — it would re-bake about's canonical
  into the new pages).
- `verify_v2.py` + `verification-report.json` — the verification run.
- `README.md` — this file.
