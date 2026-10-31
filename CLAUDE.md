# craft.sankhacooray.com

See README.md for build, preview and deploy steps.

## Lighthouse
<!-- lighthouse-optimized: 2026-10-31 -->
- Baseline (mobile): performance 71, accessibility 100, best-practices 100, SEO 100 (desktop a11y flagged the `a.brand` aria-label).
- Fixed: Google Fonts CSS loads non-blocking (print/onload + noscript), unused DM Sans 700 dropped; brand aria-label now contains the visible text; fold thumb is WebP (44 KB JPG → 18 KB); live `/api/og` images are `loading=lazy`; `public/_headers` gives icons/PNGs/thumbs a 30-day TTL.
- Left: `/api/og` serves the project's full 1200×630 PNG share image (resizing/WebP would need Cloudflare Image Transformations on the zone) with its deliberate 1 h TTL; gtag/analytics/Insights findings are handled centrally.
