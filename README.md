# Creative Ireland Impact Report 2025

Static HTML microsite for the Creative Ireland Programme's 2025 Impact Report.

**Live URL:** https://fergal-adams.github.io/creative-ireland-impact-report-2025/  
**Repo:** https://github.com/fergal-adams/creative-ireland-impact-report-2025  
**Deployed via:** GitHub Pages (branch: `main`, root `/`)

---

## Deploying to WordPress

Download the repo as a ZIP (green **Code** button → **Download ZIP**), unzip, and upload the entire folder to a subfolder on the WordPress server (e.g. via FTP/SFTP or the hosting file manager):

```
✅ index.html
✅ .htaccess
✅ assets/*.webp          (images — ~5MB)
✅ assets/*.png           (logos)
✅ assets/*.svg           (ireland map + cursor)
✅ assets/fonts/          (Founders Grotesk + FF Quadraat + Sparose font files)
✅ assets/Videos/         (MP4 video banners — ~18MB)

❌ README.md              (not needed on server)
```

**Total upload size: ~24MB**

The `.htaccess` file ensures the server sends correct MIME types for WebP images, MP4 video, and self-hosted fonts. Without it some hosts will silently drop assets.

Once uploaded, the page is accessible directly at its folder URL — no WordPress page or shortcode needed.

---

## Background

The site was originally built in Readymag but had compatibility issues in Microsoft Edge. This is a standalone static recreation of the same content, built as a single self-contained HTML file with all assets in a local `assets/` folder.

---

## Tech stack

| Concern | Approach |
|---|---|
| Markup | Single-file `index.html` — no framework, no build step |
| Styles | Inline `<style>` block in `<head>` — all CSS in one place |
| Scripts | Inline `<script>` at bottom of `<body>` |
| Fonts | Self-hosted `.otf`/`.ttf` files in `assets/fonts/`, loaded via `@font-face` |
| Images | `.webp` (all converted from original JPGs/PNGs) |
| Videos | Local `.mp4` files (`assets/Videos/`) — autoplay, muted, loop |
| Deployment | GitHub Pages — push to `main` goes live in ~1 minute |

---

## Running locally

Any static file server works. The simplest:

```bash
cd "Creative Ireland Report Site 2025"
python3 -m http.server 8742
# then open http://localhost:8742
```

> **Note:** Do not open `index.html` directly as a `file://` URL — some browsers block local video autoplay over `file://`. Always use a server.

---

## File structure

```
Creative Ireland Report Site 2025/
├── index.html              ← entire site (HTML + CSS + JS)
├── .htaccess               ← MIME types for Apache/WordPress hosting
├── README.md
└── assets/
    ├── *.webp              ← all page images (WebP)
    ├── *.svg               ← ireland-map.svg + cursor.svg
    ├── ci-logo-black.png   ← logo (footer — mix-blend-mode:multiply)
    ├── ci-logo-white.png   ← logo (nav — white on dark background)
    ├── fonts/
    │   ├── founders-regular.otf     ┐
    │   ├── founders-medium.otf      │ Founders Grotesk
    │   ├── founders-semibold.otf    │ (UI / body font)
    │   ├── founders-italic.otf      ┘
    │   ├── quadraat-regular.ttf  ┐ FF Quadraat
    │   ├── quadraat-bold.ttf     ┘ (serif / pull-quote font)
    │   └── sparose.ttf           ← Sparose (script / decorative titles)
    └── Videos/
        ├── OverallImpact.mp4
        ├── CreativeCommunities.mp4
        ├── CreativeYOuth.mp4         ← note: typo in filename, do not rename
        ├── CreativeHealth.mp4
        ├── CreativeClimate.mp4
        └── SharedIsland.mp4
```

---

## Page sections (in order)

| Anchor | Section | Notes |
|---|---|---|
| *(hero)* | Hero photo banner | Full-screen photo with title overlay |
| `#introduction` | Contents + Introduction | Minister's foreword, contents list |
| `#what-is` | What is Creative Ireland? | Stats row, impact cards |
| `#creativity` | What do we mean by creativity? | 2-col text + photo |
| `#operate` | How does Creative Ireland operate? | Pillars table + body text |
| *(no anchor)* | Embedding creativity | Policy timeline (3-col) |
| `#overall-impact` | Overall Impact | Video banner → impact card grid |
| `#communities` | Creative Communities | Video banner → 2-col content + org chart SVG |
| `#youth` | Creative Youth | Video banner → 2-col + ireland-stat → Shared Island dark section |
| `#health` | Creative Health and Wellbeing | Video banner → 2-col content |
| `#climate` | Creative Climate Action | Video banner → 2-col content + pull quote |
| `#industries` | Creative Industries | Photo banner → 2-col content |
| *(final)* | Shared Island | Video banner (outro) |
| *(footer)* | Footer | URL + logo + social icons |

---

## CSS architecture

All styles live in the `<style>` block (roughly lines 1–480 of `index.html`).

### CSS custom properties
```css
--body:     'Founders Grotesk'   /* UI / body text */
--serif:    'FF Quadraat'        /* headings, pull quotes */
--script:   'Sparose'            /* decorative section titles */
--ui:       'Founders Grotesk'   /* labels, caps */
--dark:     #1a1a18
--page-bg:  #d8ceb0              /* warm tan page background */
--tan-bg:   #c8bda0              /* slightly darker tan (footer) */
--tan-card: #c4b47a              /* gold accent */
--tan-dark: #9a8a52              /* link hover colour */
--mid:      #4a4a44
--muted:    #7a7a70
--rule:     rgba(0,0,0,0.12)     /* divider lines */
```

### Key layout patterns
- **2-col sections:** CSS Grid `grid-template-columns:1fr 1fr` — used for most content sections
- **Inline style override problem:** Many sections have inline `style="grid-template-columns:1fr 1fr;padding:64px 80px;"` attributes. These override class-level rules. Media queries use `[style*="grid-template-columns:1fr 1fr"]{grid-template-columns:1fr!important;}` to beat inline specificity at mobile widths.
- **Video banners:** `<div class="video-banner">` with autoplay `<video>` + dark overlay + centred text
- **Photo banners:** `<div class="photo-banner">` with `<img>` + dark overlay + text

### Responsive breakpoints
```
@media(max-width:960px)  — tablet: collapse 2-col grids, reduce padding
@media(max-width:600px)  — mobile: further reductions, 3-col → 1-col, font scaling
```

---

## Typography

| CSS variable / class | Font | Size | Usage |
|---|---|---|---|
| `body` | Founders Grotesk | 16px / 1.65lh | Base |
| `.right-col p` | Founders Grotesk | 17px / 1.6lh | Body text in content columns |
| `.right-col h3` | FF Quadraat | 25px | Section sub-headings |
| `.big-pull` | FF Quadraat | 22px | Left-column pull quotes |
| `#communities-content .big-pull` | FF Quadraat | 38px | Communities large pull |
| `.section-script-title` | Sparose | 28px | Decorative pillar title |
| `.contents-list li` | FF Quadraat Bold | 36px (22px mobile) | Contents page items |
| `.partners-box` | Founders Grotesk | 15px / 500wt | Partnership credit cards |
| `.photo-caption` | Founders Grotesk | 14px | Image captions |

All body text sizes are WCAG AA compliant (≥ 14px for secondary, ≥ 16px for informational body).

---

## Links / hyperlinks

All external links use `target="_blank" rel="noopener noreferrer"` and are marked with `<u>` underline only (no bold). Hover state transitions to `--tan-dark` (`#9a8a52`). Links on dark-gold backgrounds (Shared Island section) use `#e8d9b0` text colour with white on hover.

---

## Social icons (footer)

Icons use inline SVG. Instagram and YouTube required special treatment:
- **Instagram:** stroke-based SVG (`fill:none; stroke:#c4b47a`) — inner lens circle would be invisible if fill-based
- **YouTube:** outer path = stroke only; play triangle = `fill:#c4b47a; stroke:none` — two-tone approach

---

## Browser support

Tested in Chrome and Edge. Safari and Firefox need sign-off.

---

## Editing guide for the next developer

**To change any section content:** Edit `index.html` directly. There is no build process — save the file and refresh the browser.

**To add or replace an image:**
1. Convert to `.webp` (use Squoosh or `cwebp`) and place in `assets/`
2. Update the `src` attribute in `index.html`
3. Check `aspect-ratio` inline style on the `<img>` if the new image has a different ratio

**To change section colours:** Each coloured section uses an inline `background` style on the `<section>` element:
- Page background tan: `class="page-bg"` → `var(--page-bg)` = `#d8ceb0`
- Olive/gold accent sections: `background:#AD9F71`
- Dark gold (Shared Island): `background:#7a6535`
- Climate quote section: `background:#CBC1A3`

**To deploy:** Just `git push` — GitHub Pages picks up `main` automatically.
