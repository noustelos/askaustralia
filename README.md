# askaustralia.ai

Design-only landing page for **askaustralia.ai** — fourth property in the Noustelos Studio "Ask" network
([asksantorini.ai](https://asksantorini.ai) → [asksingapore.ai](https://asksingapore.ai) →
[asksydney.ai](https://asksydney.ai) → askaustralia.ai).

Everything is a single file, [index.html](index.html): vanilla HTML/CSS/JS, no frameworks, no build
step, no backend, no asset files (favicon and decorative art are inline SVG).

## What it does

- **Sells the domain.** The acquisition pill above the topbar is the primary conversion point
  (plain `mailto:`, no forms).
- **Previews the concierge concept.** The chat window is an honest **scripted demo** — four
  hardcoded Q→A pairs behind the category chips (Restaurants / Hotels / Car rental / Directions),
  with a typing-dots + word-by-word reveal. The four answers deliberately span the country
  (Melbourne, Byron Bay, the Great Ocean Road, Uluru). Zero AI, zero network; a disclosure caption
  links to the live concierge at asksantorini.ai.
- **Live dual-zone clock** in the topbar via `Intl.DateTimeFormat` — Sydney and Perth bracket the
  continent's three time zones, with the AEST/AEDT and AWST labels derived from the formatter
  (Sydney observes DST, Perth does not). The weather line is a static mockup awaiting live data.

## Design — "Red Centre"

Desert-dusk editorial: desert-sand paper (`#FAF5EC`), deep earth-brown ink (`#2B1D14`),
Uluru ochre (`#A6431F`) as the accent family, and deep eucalypt green (`#1F5C45`) strictly
reserved for the two conversion points (Send button + acquisition pill). Signature decoration:
thin line-art of Uluru with its runoff striations, a low desert sun faint behind and desert
ripples beneath. Type: **Young Serif** (display serif) + **Figtree** (UI). All tokens live in
`:root` — see [CLAUDE.md](CLAUDE.md) for the full architecture.

## Run

No build. Open `index.html` in a browser, or:

```bash
python3 -m http.server 8000   # http://localhost:8000
```

## Deploy

Cloudflare Pages, git-connected: **push to `main` = instant live** on askaustralia.ai.
Work on branches, merge deliberately, and verify the deployment landed in the Pages dashboard
after every push.
