# DESIGN.md

Implementation spec for the Leviathan OffSec surfaces: [leviathan.ac](https://leviathan.ac),
the tool READMEs, and the Hermes dashboard.

**`assets/brand.css` is the source of truth for tokens. This file is the spec for
how they get used.** If the two disagree, `brand.css` wins and this file is wrong.

Positioning, voice and the rules that outrank taste are in [BRAND.md](BRAND.md).

## Provenance

The base palette was sampled from projectdiscovery.io in October 2026 using the
`firecrawl-website-design-clone` workflow. Raw capture is kept in
`.firecrawl/projectdiscovery-branding.json` and the reference screenshot in
`.firecrawl/projectdiscovery-screenshot.png`.

That sample is a starting point, not an identity. What is ours on top of it:

- **`--amber` is not a severity colour here.** It means *could not be evaluated*.
  The `.evidence` component exists so that number never sits next to a pass in
  the same neutral weight.
- **`--slate`** means *evaluated and refused*, which is a third state distinct
  from both a finding and a gap.
- **`--blue`** carries structure and counts, never risk.
- **`.evidence` and `.chain`** are the recurring components. No other tool in this
  space ships a coverage block, so it is the part worth owning.

## Tokens

### Colour

| Token | Value | Means |
| --- | --- | --- |
| `--bg` | `#07090E` | page |
| `--bg-raised` | `#0A0E17` | raised surface |
| `--card-bg` | `#0D111A` | card |
| `--surface-sunk` | `#080B12` | code and evidence blocks |
| `--card-border` | `#1B2333` | border |
| `--card-hover` | `#26334D` | hover border |
| `--text` | `#F1F5F9` | primary |
| `--text-muted` | `#8E9BB0` | secondary |
| `--text-faint` | `#64748B` | labels, table headers |
| `--accent` | `#00FFCC` | brand, the finding, live edge |
| `--accent-cyan` | `#00F0FF` | secondary accent, chain hops |
| `--green` | `#10B981` | evaluated and permitted |
| `--amber` | `#F59E0B` | could not be evaluated |
| `--slate` | `#94A3B8` | evaluated and refused |
| `--red` | `#F43F5E` | critical, admin, the thing sought |
| `--blue` | `#38BDF8` | structure, graph shape, counts |
| `--orange` | `#FB923C` | elevated, needs a human |

### Type

- Body: `Inter`, `15px` to `16px`, `line-height: 1.7`, colour `#CBD5E1`.
- Data: `JetBrains Mono` for every number, path, ARN, counter and CLI snippet.
  Monospace is not a style choice here, it is what makes a changed digit visible
  and a column of counts align.
- `h1` `clamp(2rem, 5vw, 2.85rem)` weight 800, `letter-spacing: -1px`
- `h2` `1.6rem` weight 700 · `h3` `1.22rem` weight 600

### Geometry

- Measure: `1140px` marketing, `840px` long-form research
- Grid gap: `1.5rem`
- Radius: `8px` cards and code, `4px` badges, `9999px` pills

## Components

1. **Sticky nav.** `backdrop-filter: blur(14px)` at 90% opacity. Brand mark plus
   uppercase mono `LEVIATHAN OFFSEC` left, mono links right, accent CTA last.
2. **Tool card.** Name in mono 700 plus a category pill. Two or three sentences
   of technical description on `flex-grow: 1` so cards in a row match height. A
   terminal install block with a copy button. Pinned footer with repo link, MIT
   tag and language pill.
3. **Terminal demo.** Window chrome with the three dots, then real captured output.
   Never simulated numbers.
4. **Evidence block.** `.evidence` with a `data-label` header and `.ok` `.warn`
   `.skip` `.bad` `.info` `.dim` spans. This is the component the whole identity
   is built around.
5. **Chain.** `.chain`, one hop per line, `.kind` in accent-cyan, `.unverified`
   in amber.
6. **Research dossier.** Category pill, date, read time, `.table-wrap` for wide
   tables, callouts with a `3px` accent left border, author box.

## Build rules

1. Tokens via custom properties only. Never a raw hex in page CSS.
2. Equal-height cards: flex column with `flex-grow: 1` on the description.
3. Every route serves both `/path/` and `/path.html` via `jekyll-redirect-from`.
4. Contrast: `#CBD5E1` on `#07090E`, never pure black on dark.
5. A number ships with the count of what it excludes, or it does not ship.
6. Unknown never renders as zero.
