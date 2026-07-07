# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A **design-only** landing page for askaustralia.ai — a property in the Noustelos Studio "Ask" network (asksantorini.ai is the flagship; sisters currently include asksingapore.ai, asksydney.ai, asknewyork.ai and askmykonos.ai, with more to come — the `.foot` footer in [index.html](index.html) is the canonical list). The page lives in a single file: [index.html](index.html) — vanilla HTML/CSS/JS, no frameworks, no build step, no backend, no dependencies. Standalone: zero sister-site dependency (sister links only). The repo root also carries a small discovery/SEO layer: [robots.txt](robots.txt), [sitemap.xml](sitemap.xml), [llms.txt](llms.txt), [og-image.png](og-image.png) (the only binary asset) and its source [og-card.html](og-card.html).

There is intentionally **no AI or network logic**. The chat is a **SCRIPTED DEMO**: four hardcoded Q→A pairs matching the chip prefills play a typing-dots + word-by-word reveal; any other input gets a fallback line. All client-side — the real backend later replaces the `SCRIPTED` map + `respond()` in the inline script (the hooks stay put). Treat this repo as the visual source of truth; do not add fetch/API/state-management code unless explicitly asked.

**Honesty rule: the demo must not fake live-AI signals.** The bold `.caption` under the category chips discloses the scripted demo and links to the live concierge at [asksantorini.ai](https://asksantorini.ai) (the only disclosure line — there is no separate `.disclaimer`), and the chat-window status label reads "Live Demo · Ask Australia AI". Keep both honest until the real backend lands.

The commercial goal of the page is to **sell the domain**: the `.acquire` pill above the topbar states availability and links out via `mailto:` to `info@asksantorini.ai` (the only outbound contact — keep it a plain mailto, no forms; switch to `info@askaustralia.ai` once that mailbox is set up). The `.foot` footer links to every sister domain (currently [asksantorini.ai](https://asksantorini.ai), [asksingapore.ai](https://asksingapore.ai), [asksydney.ai](https://asksydney.ai), [asknewyork.ai](https://asknewyork.ai) and [askmykonos.ai](https://askmykonos.ai) — the network grows, so treat this footer as the canonical list and append new sisters there), and the bottom-left `.studio-mark` links to [noustelos.gr](https://noustelos.gr) — all signal the brand network. `<head>` carries the canonical URL, Open Graph/Twitter cards (`summary_large_image` + `og:image` → `https://askaustralia.ai/og-image.png`), an inline SVG favicon, and JSON-LD, so shared links preview well for prospective buyers.

## Discovery / SEO layer

- **[robots.txt](robots.txt)** — allows everything, points to the sitemap.
- **[sitemap.xml](sitemap.xml)** — single URL; bump `<lastmod>` on meaningful page changes.
- **[llms.txt](llms.txt)** — the AI-crawler summary. **Honesty rule applies here too: it must never present the scripted demo as live AI.** Keep its "Sister concierges" list in sync with the `.foot` footer (always the same sisters as the footer, in the same order — never askaustralia itself). When a new sister domain joins the network, update the footer and this list together.
- **[og-image.png](og-image.png)** — 1200×630 social card referenced from `<head>`. Regenerate from [og-card.html](og-card.html) (a standalone Red Centre card mirroring the hero; keep its `:root` tokens in sync with index.html) via headless Chrome + `sips`:

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new \
  --disable-gpu --hide-scrollbars --force-device-scale-factor=2 \
  --window-size=1240,670 --virtual-time-budget=10000 \
  --screenshot=og-big.png og-card.html
sips -c 1260 2400 og-big.png --out og-crop.png
sips -z 630 1200 og-crop.png --out og-image.png
```

The card sits in a 20px paper margin inside a 1240×670 window and is center-cropped because headless Chrome paints rounded window corners black — don't drop the margin/crop. Eyeball the final PNG before committing; delete the `og-big.png`/`og-crop.png` intermediates.

## Deploy

⚠️ **Cloudflare Pages, git-connected: push to `main` = INSTANT LIVE on askaustralia.ai** (no branch previews, like the sister sites). Work on a branch, merge deliberately, verify live after push — the askSingapore webhook once died silently and needed a repo reconnect in the Pages dashboard, so check the deployment actually landed. Rollback = revert commit + push (or dashboard rollback).

## Run

No build. Open [index.html](index.html) directly in a browser, or serve it:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Architecture

The page is one `.hero` section with three layers:

1. **Ambient decoration** (`.deco-*`, `aria-hidden`) — the signature `.deco-uluru` "Red Centre View" (thin ochre line-art, inline SVG, bottom-left: Uluru with its runoff striations, a low desert sun faint behind, desert ripples and spinifex beneath, evening stars above) plus the `.deco-weave` adaptive theme layer (transparent by default, dissolves in a pattern per query theme). Purely cosmetic.
2. **`.inner`** — the `.acquire` domain-for-sale pill, the topbar (brand mark + wordmark + dual-zone Australia clock) and `.main`, which holds the `.hero-row` (the "Just Ask" title on the left, the `.chat` mock window on the right), the `.closing` block, and the `.foot` sister-domains footer.
3. **Floating anchors** — the `.cta-pill` "Ask Australia AI" button (bottom right) and the `.studio-mark` Noustelos Studio attribution (bottom left; joins the flow under the footer on narrow screens).

### Styling system

Design language: **"Red Centre"** (desert-dusk editorial). All design decisions are centralized as CSS custom properties in `:root`. Change colors there, not at call sites:

- `--paper: #FAF5EC` — warm desert-sand off-white page field; `--ink: #2B1D14` — deep earth-brown body text.
- `--ochre: #A6431F` (+ `--ochre-deep`) — primary Uluru ochre/rust accents; `--mist: #F0E7D9` — soft sandstone surfaces/dividers.
- `--eucalypt: #1F5C45` — deep eucalypt green, **CTAs ONLY**: the Send button (`--send-grad`) and the acquisition pill. Nowhere else. This scarcity is the page's conversion logic.
- `--diamond-grad`, `--wordmark-grad` — ochre/ink gradients for the brand mark and wordmark.
- `--font-display` (**Young Serif**, warm earthen display serif, 400 only — hero title roman, with "Ask" in ochre via `.display .accent`; the closing italics are synthesized obliques) and `--font-ui` (Figtree) — loaded from Google Fonts in `<head>`.

Animations are defined as `@keyframes au*` near the top of the `<style>` block (`auMarkShimmer`/`auDiamondPulse` "living" logo, `auSendGlow`/`auSendSpark` Send-button feedback, `auLivePulse`, `auRise` staged page entrance). No continuous full-surface animations or `backdrop-filter` — keep it light for older machines. All motion is gated behind a `prefers-reduced-motion: reduce` guard — extend that rule when adding new animations. Keyboard focus uses a global ochre `:focus-visible` outline (the input row carries the ring via `:focus-within` instead of the field itself). Layout is non-wrapping by design and only stacks below the `max-width: 760px` media query.

### Interactive behaviour (client-side only, no network)

The inline `<script>` drives the scripted demo and presentation feedback — still zero AI/network. Keep additions on this side of that line:

- **Scripted chat** — `#chat-log` is a nested-scroll message list (max-height, thin ochre scrollbar; the greeting is the first bot message). `submitQuery()` appends the user bubble (right-aligned, deeper sandstone); `respond()` shows typing dots (~800ms) then streams the answer word-by-word at 45ms/word (opacity-only spans). Under `prefers-reduced-motion` the **streaming still plays** (30ms/word, instant pop) — only the dots pulse and smooth scrolling are dropped. Copy lives in the `SCRIPTED` array + `FALLBACK` string. The four answers deliberately span the country (Melbourne / Byron Bay / Great Ocean Road / Uluru) so the page reads "all of Australia", not one city.
- **Send button** toggles `.is-ready` (eucalypt glow) when `#ask-input` holds text, and replays a `.spark` sweep; Send/Enter with text runs `submitQuery()`.
- **Adaptive background** — `detectTheme()` matches query keywords and swaps a `theme-outback` / `theme-reef` / `theme-coast` class on `.hero`, which dissolves a faint pattern into `.deco-weave` (spinifex-seed dots / coral scallops / rolling swell lines) via a brief `weave-shift` dissolve. Keyword→theme maps live in the `THEMES` array.
- **Live dual-zone clock** — `#aus-date` + `#syd-clock` + `#per-clock` in the topbar show the current Sydney and Perth times via `Intl.DateTimeFormat` (`Australia/Sydney` / `Australia/Perth` — computed locally, no network), refreshed on a 15s interval. Australia spans three time zones; Sydney (east) and Perth (west) bracket the continent. Editorial lockup, top to bottom: an ochre `.clock-date` line (Sydney's date), the `.clock-zones` pair of light `.clock-time` numerals with dimmed `.cc` colons and per-zone `.zone-label` lines, then a small tracked `.clock-meta` line (live dot · "24/7 Coast-to-Coast Availability"). ⚠️ Sydney observes DST (AEST/AEDT), Perth does not (AWST) — both zone labels (`#syd-tz`, `#per-tz`) are **derived** each tick from the formatter's `timeZoneName: 'short'` part, never hardcoded. `tickClock()` writes the date text, both `HH<span class="cc">:</span>MM` markups, and both zone labels each tick.

### Backend wiring hooks (placeholders only)

These IDs/hooks are where the real backend connects later (replacing the scripted `respond()`) — keep them in place:

- `#ask-input` — chat input field
- `#ask-send` — Send button (runs the scripted `submitQuery()`)
- `#cta-ask` — floating CTA pill
- `.chip[data-prefill]` — category chips: **prefill** `#ask-input`, then auto-send after 300ms (unless a reply is playing)
- `#aus-temp` — local-weather line under the clock; a static mockup (`16°C · Clear Skies`) awaiting live data (real weather needs an API, which is out of the design-only scope)
- `#chat-log` — conversation log the backend will append to
