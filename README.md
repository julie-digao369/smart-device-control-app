# Aurio — Hearing-Aid Companion App

An iOS-style PWA **interaction prototype** for a glasses-form hearing aid: a calm, tactile control surface for volume, noise reduction, and media — built to demonstrate UX/UI and front-end interaction-design craft.

> **Portfolio / design-demo notice.** *Aurio* is a **fictional brand**. This is a personal design-portfolio piece — an anonymized concept demo that is **not affiliated with, endorsed by, or built from the confidential materials of any real company**. It is inspired by real product-design experience, with all brand identity, names, and assets re-created for demonstration purposes only.

🌐 **Live demo:** https://smart-device-control-app.julie-gao369.workers.dev

> Best viewed on iPhone Safari (*Share → Add to Home Screen* runs it as a standalone PWA). On desktop it renders inside a 390×844 iPhone frame.

---

## What this prototype demonstrates

- **Token-driven design system** — a single source of color, type, spacing, radii, shadow, and motion tokens (`aurio-tokens.css` / `tokens.json`) drives the whole UI.
- **Glassmorphism with real depth** — layered frosted surfaces, soft shadows, and an "Eastern-calm" (东方禅意) visual direction rather than flat defaults.
- **Micro-interactions** — draggable radial volume gauge, animated noise-mode backgrounds, scene-following auto program, and smooth state transitions on compositor-friendly properties.
- **iOS PWA patterns** — installable manifest, status-bar styling, safe-area handling, and a phone-frame presentation that adapts to real devices.
- **Hand-written interaction logic** — vanilla-JS state and navigation, no framework, data-driven render rather than in-place DOM mutation.

---

## Screens

**Onboarding**
- Splash → "Get started" → Bluetooth pairing sheet (ready → searching → found → pairing → connected).

**Tab 1 — Volume**
- Radial gauge with a draggable thumb, fine +/− control, and a mute pill.
- Left/right-ear split mode with independent sliders.
- Device status chips (connection state / battery) and a collapsible EQ tone card with a per-ear curve.

**Tab 2 — Noise reduction**
- Four modes — Off / Comfort / Strong / Ultimate — each with its own animated backdrop and theme accent.
- Scene detection plus a **smart auto program** that follows the surrounding environment, and editable custom scenes.

**Tab 3 — Media**
- Streaming and phone-call panels with playback controls and media-specific EQ.

**Connectivity states**
- Bluetooth connect / disconnect overlay with reconnect flow and live status bar.

---

## Tech stack

- **Single-file app** — all HTML, CSS, and JS live in `index.html` (zero build, zero runtime dependencies). Simple to deploy and share; navigation conventions are documented in [CLAUDE.md](./CLAUDE.md).
- **PWA** — `manifest.json` + Apple touch icon.
- **Design tokens** — `aurio-tokens.css` (CSS custom properties) with a `tokens.json` export.
- **Fonts** — self-hosted Inter + Playfair Display (`fonts/`).
- **Hosting** — Cloudflare Workers static assets (SPA fallback), deployed from CI.

---

## Project structure

```
.
├── index.html              # The prototype — HTML + CSS + JS, single source of truth
├── aurio-tokens.css        # Design tokens (color / spacing / radii / shadow / motion)
├── aurio-components.html   # Component showcase (dev reference)
├── design-system.md        # Component inventory + type/motion spec
├── tokens.json             # JSON export of the design tokens
├── CLAUDE.md               # Navigation map, CSS conventions, design-sync workflow
├── manifest.json           # PWA metadata
├── wrangler.toml           # Cloudflare Workers config
├── .assetsignore           # Files excluded from deploy
├── .github/workflows/      # CI auto-deploy
├── fonts/                  # Inter / Playfair Display (woff2)
└── *.png / *.webp / *.svg  # Product, icon, and background imagery
```

---

## Local development

Zero build, zero dependencies.

```bash
# open directly
open index.html

# or serve (needed for PWA / service-worker behavior)
python3 -m http.server 8000   # → http://localhost:8000
# or: npx serve .
```

Target device for all screenshots/debugging: **iPhone 390×844**.

---

## Deployment

CI deploys to Cloudflare Workers on push to `main` (`wrangler deploy`). `.assetsignore` keeps internal docs, the component showcase, and redundant image sources off the edge — only `index.html` and the assets it references ship.

```bash
npx wrangler deploy          # manual deploy (needs CLOUDFLARE_API_TOKEN)
```

---

## Design system

See [design-system.md](./design-system.md) for the component inventory, typography scale, spacing/radii, glassmorphism recipe, and motion timings. Colors are always referenced through `aurio-tokens.css` variables (e.g. `var(--blue-main)`) — never raw hex.
