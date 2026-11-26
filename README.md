# Judit Quiromasaje — website

Static trilingual site for **Judit Quiromasaje y Estética avanzada**, a massage and
advanced aesthetics studio at Rambla de l'Exposició, 103, Vilanova i la Geltrú.

Published at <https://juditquiromasaje.vilanovailageltru.info/>.

## Build

```bash
node scripts/build.mjs      # renders index.html, es/, en/, sitemap, robots, manifest
node scripts/assets.mjs     # favicon, icons, og-image.jpg
node scripts/verify.mjs     # headless checks over the built output
node scripts/card.mjs       # assets/photos/handover.png (10.5 x 14.5 cm @ 300 dpi + QR)
```

`node_modules` is a symlink to the workspace modules and is ignored by git.

## Structure

```
src/template.html      single template for all locales
src/data.json          business facts (build reads it; no absolute URLs)
src/locales.json       ca (default, ""), es ("es/"), en ("en/")
src/i18n/<code>.json   copy per locale
src/photos/            source photo, downscaled and copied to assets/img/ at build
assets/css/styles.css  one stylesheet, palette tokens in :root
assets/js/main.js      progressive enhancement: nav, reveal, cookie consent, map gate
scripts/               build / verify / assets / card
```

## Notes

- **WhatsApp first.** The number is a Spanish mobile, so the booking action is a
  `wa.me` link in the hero, the first-visit block, the header and the contact section.
  A `tel:` link to the same number sits next to it.
- **Reviews.** Five five-star reviews are quoted as extracts, translated into each
  locale and attributed with the author and relative date. The 5.0/167 aggregate comes
  from the Google listing and is surfaced in the hero, the stats grid and the reviews header.
- **Map and analytics are behind consent.** The map iframe lives in a `<template>` and
  is only appended after the visitor accepts; GA4 is loaded on accept only, with consent
  mode `denied` by default. Analytics use the shared `cookie-consent-v1` key.
- **Photo credit.** The hero photo is the studio's own Google listing photo; the
  attribution link is rendered in the caption.
- **No prices or opening hours.** Neither is asserted, because neither is confirmed for
  this business; the treatment lists say the cost is agreed before a session starts.
- **URLs.** Canonical, `og:url`, hreflang, sitemap and the handover QR all derive from
  `slug` + `siteDomain` in `src/data.json`.

## Deploy

`.github/workflows/deploy-aws.yml` builds, then syncs to
`s3://dr-static-websites/vilanovailageltru.info/juditquiromasaje/` (region `eu-south-2`).
