# craft

**[craft.sankhacooray.com](https://craft.sankhacooray.com)** — the design, engineering & making hub in the
sankhacooray.com network: physical things Sankha designs (often in Illustrator), engineers and builds by hand —
furniture made for home, and paper engineering.

Served by a Cloudflare Worker (`craft-sankhacooray`): static assets from `public/`, plus `src/worker.js`, whose
`/api/og?url=` route fetches a project site's current `og:image` (allowlisted hosts only, cached 1 h).
The VS Code workspace for the hub and its projects is `../craft-sankhacooray-com.code-workspace`.

## Files

| Path | Purpose |
|------|---------|
| `projects.json` | The hub's content: areas (furniture, paper), pieces, roles, years, links |
| `footer.html` | One-line footer, placed in the last section |
| `template.html` | Page template (CSS + JS inline): workshop palette, joinery-drawing hero, tool-glyph particles |
| `build.py` | Renders `public/index.html`; packs small areas side by side in balanced rows |
| `public/thumbs/` | Card images (800×420) — photos of pieces go here |
| `public/og.png` | 1200×630 share image, rendered from `og/og.html` |
| `public/icon*.png`, `icon.svg`, `apple-touch-icon.png`, `manifest.webmanifest` | Icons + PWA manifest |
| `src/worker.js` | Serves assets; `/api/og` live share-image proxy |
| `wrangler.toml` | Worker config + custom domain (creates the DNS record) |

## Adding a piece

The furniture area currently holds three placeholder cards (`"placeholder": true`). Replace one per piece:

```json
{
  "name": "Walnut desk",
  "desc": "One line on what it is and what's clever about it.",
  "plat": [],
  "kind": "object",
  "role": "mine",
  "org": "Designed & built by hand",
  "years": "2024",
  "thumb": "walnut-desk.jpg"
}
```

Put the photo (or the Illustrator drawing exported to JPG/PNG) at `public/thumbs/<name>.jpg`, cropped to 800×420:
`sips -c <h> <w> photo.jpg --out tmp.jpg && sips -z 420 800 tmp.jpg --out public/thumbs/walnut-desk.jpg`.
New areas (lighting, tools, toys…) are new entries in `groups`; `build.py` packs them into rows of up to 5 cards.

## Edit, preview, deploy

```
python3 build.py
python3 -m http.server 4805 --bind 127.0.0.1 -d public
CLOUDFLARE_ACCOUNT_ID=9a1da48a0f61e7cebcba39517856d87e npx wrangler deploy
```

Share image: edit `og/og.html`, then render it and bump `?v=N` on `og:image`/`twitter:image` in `template.html`:

```
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --hide-scrollbars \
  --force-device-scale-factor=1 --window-size=1200,630 --virtual-time-budget=6000 \
  --allow-file-access-from-files --screenshot=public/og.png "file://$PWD/og/og.html"
```

After deploying, curl the page, `og.png` and the icons to confirm 200s and the right content types.
