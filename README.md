# ranpaeximagri.com — Optimized Copy (NOT yet deployed)

Optimized static copy of https://ranpaeximagri.com/, built 2026-10-13.
**Nothing was deployed. The live site is untouched. No Hostinger login was used.**
This folder is ready to upload to Hostinger `public_html` (steps below).

## Baseline (live site, PageSpeed Insights, verified 2026-10-13)

|  | Performance | Accessibility | Best Practices | SEO |
|---|---|---|---|---|
| Mobile | 38 | 71 | 92 | 82 |
| Desktop | 47 | 82 | 88 | 82 |

Mobile LCP 60.8s, Speed Index 14.6s, CLS 0.538, total page weight ~22.3 MB.
Biggest PageSpeed complaint: image delivery (~11.4 MB waste).

## What changed

**Images — the entire win.** All 70 raster images converted PNG/JPG → WebP
(Pillow, photos q=78, images with transparency q=85, method 6).
Hero backgrounds capped at 1920w, everything else capped at 1200w (downscale only).
Original PNG/JPG files deleted from `site/`; every reference in HTML updated
to `.webp` (same filenames otherwise, same `&#32;` encoding for spaces).

|  | Before | After | Change |
|---|---|---|---|
| Images total (70 files) | 57,468,730 B (54.8 MB) | 3,290,730 B (3.14 MB) | −94.3% |
| Whole `site/` folder | ~59 MB | ~7.3 MB | −87.6% |

Per-page referenced weight (HTML + CSS/JS + images that page loads; fonts excluded):

| Page | Before | After | Change |
|---|---|---|---|
| index.html | 22,760 KB | 2,376 KB | −89.6% |
| about.html | 6,180 KB | 1,436 KB | −76.8% |
| certificates.html | 2,163 KB | 1,361 KB | −37.1% |
| coconuts.html | 11,791 KB | 1,454 KB | −87.7% |
| contact.html | 2,261 KB | 1,183 KB | −47.7% |
| fruits.html | 6,172 KB | 1,326 KB | −78.5% |
| spices.html | 5,964 KB | 1,547 KB | −74.1% |
| vegetables.html | 11,746 KB | 1,571 KB | −86.6% |

Largest images, before → after:

| File | Before | After | Saved |
|---|---|---|---|
| images/bgc4.png | 3334 KB (2000×1335) | 202 KB (1920×1282) | 93.9% |
| images/aboutusbanner.png | 3149 KB (2000×1347) | 191 KB (1920×1293) | 93.9% |
| images/bgc3.png | 3123 KB (2000×1335) | 168 KB (1920×1282) | 94.6% |
| images/bgc1.png | 3122 KB (2000×1335) | 188 KB (1920×1282) | 94.0% |
| images/bgc2.png | 3100 KB (2000×1335) | 174 KB (1920×1282) | 94.4% |
| images/shipway.png | 2487 KB (1536×1024) | 145 KB (1200×800) | 94.2% |
| images/coconuts/oil.png | 2183 KB (1536×1024) | 56 KB (1200×800) | 97.4% |
| images/vegetables/beans.png | 2050 KB (1536×1024) | 86 KB (1200×800) | 95.8% |

Full per-file inventory: `image-inventory-before.csv` / `image-inventory-after.csv`.

**HTML patches (all 8 pages: index, about, certificates, coconuts, contact,
fruits, spices, vegetables) — minimal:**
- Meta descriptions are now unique per page (see SEO pass below). The
  performance pass had added one generic description to all pages; the SEO
  pass replaced it with page-specific copy.
- Added `width` + `height` (true optimized dimensions) to every `<img>`
  (existing `width="120px" height="80px"` logo attributes kept as-is) —
  fixes layout-shift (CLS) warnings.
- Added `loading="lazy" decoding="async"` to every `<img>` except the logo.
  Hero/LCP images are CSS backgrounds, so no LCP image was lazy-loaded.
- Added `alt` only where missing (logo → "Ranpa Exim", others derived from
  filename, e.g. "Semi Husked Coconut"). Existing alt text untouched.
- Added hero preload: index → `images/bgc1.webp`, about →
  `images/aboutusbanner.webp`, contact → `images/redchillisbanner.webp`,
  each with `fetchpriority="high"`. Logo `<img>` also got
  `fetchpriority="high"`.
- Favicon: new `images/favicon.png` (64×64, 8 KB, made from the logo) replaces
  the 425 KB logo file previously used as favicon. Avoids a WebP-favicon
  compatibility question and saves weight on every page load.

## SEO pass (titles, descriptions, H1s, internal links)

Added after the performance pass. No Hostinger login, no deploy. Machine-readable
report: `seo-report.json` (scripts: `seo.py`, `seo_part2.py`).

### Titles + descriptions, before → after

Before = live-site titles; live site had **no** meta descriptions at all.
Titles are all ≤ 60 chars, descriptions 120–160 chars.

| Page | Before title | After title (chars) | After description (chars) |
|---|---|---|---|
| index.html | `Ranpa Exim` | Ranpa Exim \| Red Chilli, Spices & Coconut Exporter (50) | Ranpa Exim exports red chilli, spices, coconuts, fruits & vegetables from Madhira, Telangana, India. Bulk export quality for global buyers. (139) |
| spices.html | `RanpaEximAgri - Spices` | Red Chilli & Spices Exporter \| Ranpa Exim (41) | Export-grade red chilli (Teja, S4 Sannam, 334) & spices from Ranpa Exim, Madhira. Bulk packing for importers. Enquire for prices & samples. (139) |
| vegetables.html | `RanpaExim - Vegestables` | Fresh Vegetables Exporter \| Ranpa Exim (38) | Fresh vegetables exported in bulk by Ranpa Exim from Madhira, Telangana — beans, bitter gourd, okra, tomato & more, packed fresh for importers. (143) |
| fruits.html | `RanpaExim - Fruits` | Fresh Fruits Exporter \| Ranpa Exim (34) | Fresh fruits exported in bulk by Ranpa Exim — mango, banana, papaya, pomegranate, grapes & more from Madhira, Telangana, India. (127) |
| coconuts.html | `RanpaExim- Coconuts` | Coconuts & Coconut Products Exporter \| Ranpa Exim (49) | Coconuts & coconut products from Ranpa Exim — tender, semi-husked, fully husked coconuts, copra & coconut oil for bulk export buyers. (133) |
| certificates.html | `RanpaExim -Certificates` | Export Certificates \| Ranpa Exim (32) | Ranpa Exim export credentials: APEDA, IEC, GST, MSME & Coconut Board certificates. Verified exporter from Madhira, Telangana, India. (132) |
| about.html | `Ranpa Exim - About Us` | About Ranpa Exim \| Exporter from Madhira, India (47) | About Ranpa Exim Pvt Ltd — exporter of red chilli, spices, coconuts, fruits & vegetables from Madhira, Telangana, India. Our story & mission. (141) |
| contact.html | `RanpaExim - Contact` | Contact Ranpa Exim \| Export Enquiry, Madhira (44) | Contact Ranpa Exim for bulk export enquiries — red chilli, spices, coconuts, fruits & vegetables. Office in Madhira, Telangana. Call or email us. (145) |

### One H1 per page (verified)

| Page | Before | After |
|---|---|---|
| index.html | 4 × `<h1>` (hero slider) | 1 × `<h1>` (first slide kept); other 3 slides demoted to `<h2>` |
| spices.html | 1 × `<h1>` reading “Coconuts” (copy-paste bug) | 1 × `<h1>` reading “Spices” |
| certificates.html | 0 × `<h1>` | 1 × `<h1 class="sr-only">` (screen-reader only, zero visual change) |
| about / coconuts / contact / fruits / vegetables | 1 × `<h1>` each | Unchanged |

To keep the demoted slider headings rendering pixel-identically, the two
slider rules in `css/style.css` (`.owl-carousel.home-slider .slider-item
.slider-text h1`, incl. the mobile media query) had their selectors extended
to `h1, h2`. That is the **only** CSS change; JS and fonts are byte-identical
to the live site.

### Internal linking (footer “Useful Links”)

Every page’s footer now links to all 8 pages with descriptive anchors
(verified: no page missing any of the 8). The block now reads:
Home, About Us, Red Chilli & Spices, Fresh Vegetables, Fresh Fruits,
Coconuts & Coconut Products, Export Certificates, Contact Us — plus the
pre-existing Blog and Downloads links, kept as-is.
Two footer bugs fixed: “Products” pointed to `coconuts.html` on the
coconuts/fruits/vegetables pages (now `spices.html` everywhere), and its
anchor is now “Red Chilli & Spices”. Blog/Downloads hrefs kept per-page as
in the original (self-page `#` dead anchors).

### Alt text

No alt changes were needed in this pass: every `<img>` already had alt text
(the hitwebcounter image uses capital-`A` `Alt="Counters"`, which is valid HTML).

### Deliberate SEO boundaries

- Body copy untouched, except the spices-page H1 fix above and footer anchor
  text. The “Vegestables” typo was fixed in the vegetables title only; body
  copy still says “Vegestables” where the original did.
- No new pages, no schema/JSON-LD, no sitemap/robots changes (static site has
  none), no keyword stuffing. Titles/descriptions describe the actual page.

**Deliberately NOT changed (performance pass, updated):**
- JS, fonts: byte-identical to the live site. CSS: only the 2 slider selector
  lines above changed (see SEO pass).
  jQuery / Bootstrap / Owl Carousel load order untouched — the hero slider
  depends on them. No JS deferred.
- Visible text: identical to the original mirror except the SEO-pass footer
  anchors and the spices H1 fix (verified by tag-stripped diff).
- Colors, layout, copy, links, contact details: otherwise untouched.
- Google Fonts links untouched (still render-blocking — see Risks).
- Unsplash hero background on coconuts/fruits/spices/vegetables pages
  (`https://images.unsplash.com/photo-1504754524776-8f4f37790ca0`) untouched —
  it is an external hotlink, not a local file.

## Lighthouse (local) — NOT AVAILABLE, read this

Local Lighthouse could **not** be run: in this sandbox, headless Chrome fails
to load even a working localhost server
(`CHROME_INTERSTITIAL_ERROR`, `finalUrl: chrome-error://chromewebdata/`),
tried with `--no-proxy-server`, fresh profile, and background-networking
disabled. The size numbers above are measured, not estimated.
**After deploying, re-run PageSpeed Insights on the live URL to get the real
new scores.** Expect a large Performance jump (page weight −90%), but do not
quote a new score until the live check is done.

## Deploy (Hostinger hPanel File Manager → public_html)

1. **Back up first:** in File Manager, select everything in `public_html` →
   Compress → download the ZIP. Keep it until the new version is verified.
2. Upload the contents of `site/` (the files *inside* `site/`, not the `site/`
   folder itself) into `public_html`, overwriting. This replaces all 8 HTML
   files, all images, `css/style.css`, and adds `images/favicon.png`.
   `css/style.css` MUST be uploaded — it carries the slider `h2` rule that
   keeps the demoted homepage slider headings rendering identically.
   JS/fonts are unchanged from live, so overwriting them is harmless.
3. Confirm no stray files remain: `public_html` should contain **no**
   `.png`/`.jpg`/`.jpeg` under `images/` after upload (except the new
   `favicon.png`). The only `.png` left anywhere is `favicon.png`.
4. Hard-refresh (Ctrl+Shift+R) and open: home, one category page, contact.
   Check hero slider, product images, footer logo, favicon.
5. Re-run PageSpeed Insights for mobile + desktop on
   https://ranpaeximagri.com/ and record the new scores.

## Known issues / risks

- **WebP, no `<picture>` fallback.** References were swapped straight to
  `.webp` (no JPEG/PNG fallback). WebP works in all modern browsers
  (Chrome/Edge/Firefox/Safari 14+, ~97%+ of traffic). Very old browsers
  (IE, Safari <14) will show broken images. If that matters, add
  `<picture>` fallbacks before deploying — the originals are in `original/`.
- **Pre-existing broken references, left as-is (broken on the live site too):**
  `images/category-1.jpg` (referenced in about.html, 404 live) and
  `images/loc.png` (referenced in `js/google-map.js`, 404 live). Neither was
  invented or removed — fixing them needs a real replacement image from the owner.
- **External YouTube thumbnail** (`hqdefault.jpg`) in the HTML is an external
  URL, untouched.
- **Google Fonts** still render-blocking (PageSpeed flagged ~490 ms). Left
  alone on purpose — changing font loading can shift layout. Optional
  follow-up: `display=swap` is already set; `preload` the font CSS or
  self-host fonts.
- **Unsplash hero** on 4 category pages is an external, unoptimized image.
  Optional follow-up: download it once, optimize, serve locally.
- **6 images were missing from the first mirror** (wget skipped them) and were
  fetched directly: `certificates/coconutcerti.png`, `spices/sp5.png`,
  `spices/sp7.png`, `partner/2.png`, `partner/4.png`, `partner/8.png`.
  All verified as real PNGs and included in the optimization.

## Files in this folder

- `site/` — the optimized site, ready to upload (contents → `public_html`).
- `original/ranpaeximagri.com/` — pristine mirror of the live site, for
  diff/rollback. Do NOT upload this.
- `image-inventory-before.csv` / `image-inventory-after.csv` — per-file sizes
  + dimensions, before/after.
- `optimization-report.json` — machine-readable conversion + verification report.
- `optimize.py` — the script that did the conversion + HTML patching
  (re-runnable; it regenerates the inventories/report).
- `seo.py` + `seo_part2.py` — the scripts that did the SEO pass
  (titles/descriptions, H1 fixes, footer links, CSS selector extension).
- `seo-report.json` — machine-readable SEO verification (titles, descriptions,
  H1s, footer links, alt check).
- `README.md` — this file.
