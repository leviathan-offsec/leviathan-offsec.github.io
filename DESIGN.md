# DESIGN.md: ProjectDiscovery vs Leviathan OffSec

## Source
- URL: https://projectdiscovery.io & https://github.com/projectdiscovery
- Capture date: October 2026
- Evidence: Firecrawl branding tokens, asset tree, full-page screenshots, and GitHub organization scrapes.

## Reference Screenshot
![ProjectDiscovery Reference Screenshot](./.firecrawl/projectdiscovery-screenshot.png)

Use this screenshot as the visual source of truth for layout, hierarchy, density, and feel. Tokens below describe the design system in machine-readable form.

## Design Summary
ProjectDiscovery uses a high-contrast, developer-first cybersecurity aesthetic:
- **Clean Dark Baseline:** Deep near-black backgrounds (`#0A0A0A` / `#07090E`) with cold slate panels (`#0D111A`) and subtle borders (`#1B2333`).
- **Cyber Accents:** Electric neon green (`#22C55E` / `#00FFCC`) and terminal cyan for command lines, status badges, and interactive hovers.
- **Dual-Font System:** High-legibility `Inter` for clean narrative and structure, paired with `JetBrains Mono` for code, flags, and telemetry.
- **Card Rhythm:** Symmetrical 2-column or 3-column grids where every card has identical structural weight: title + pill, concise copy, one-liner CLI snippet, and pinned metadata footer.

---

## Design Tokens

### Colors
| Token | Role | Hex Value | Source |
|---|---|---|---|
| `--bg-canvas` | Deep background | `#07090E` / `#0A0A0A` | Observed |
| `--bg-panel` | Card / surface background | `#0D111A` | Observed |
| `--bg-code` | Terminal / snippet block | `#0B0F19` | Observed |
| `--border-subtle` | Card & container borders | `#1B2333` | Observed |
| `--border-hover` | Hover state border glow | `#26334D` / `rgba(0,255,204,0.3)` | Observed |
| `--text-primary` | Headings & high-contrast text | `#F1F5F9` / `#FFFFFF` | Observed |
| `--text-muted` | Body text & secondary labels | `#8E9BB0` / `#94A3B8` | Observed |
| `--accent` | Primary neon action & flags | `#00FFCC` / `#22C55E` | Observed |
| `--accent-glow` | Button & card hover shadows | `rgba(0,255,204,0.18)` | Observed |
| `--status-error` | Exit 1 / Critical finding | `#F43F5E` | Observed |
| `--status-warn` | Changed diff / High finding | `#F59E0B` | Observed |
| `--status-ok` | Clean / Mitigated finding | `#10B981` | Observed |

### Typography
- **Primary / Body:** `Inter, -apple-system, BlinkMacSystemFont, sans-serif`
  - Body copy: `15px` - `16px`, `line-height: 1.7` - `1.8`, color `#CBD5E1`.
- **Monospace / Code:** `'JetBrains Mono', monospace`
  - Font sizes: `0.75rem` (badges/pills), `0.85rem` (CLI snippets), `0.9rem` (inline code).
- **Headings:**
  - `H1`: `clamp(2rem, 5vw, 2.85rem)`, `font-weight: 800`, `letter-spacing: -1px`.
  - `H2`: `1.6rem`, `font-weight: 700`, `letter-spacing: -0.5px`.
  - `H3`: `1.22rem`, `font-weight: 600`.

### Spacing And Layout
- **Max Width Containers:**
  - Marketing / Landing page: `1140px`
  - Research dossier / Post layout: `840px` (optimized for long-form reading density)
- **Grid Gaps:** `1.5rem` (`24px`)
- **Border Radius:**
  - Cards & code blocks: `8px`
  - Badges & pills: `4px`
  - CTA Buttons: `5px` (Leviathan) to `9999px` (PD pill style)

---

## Components

### 1. Sticky Navigation Bar
- `backdrop-filter: blur(14px)` with 90% opacity background.
- Left: Brand icon (`24px`) + uppercase mono title (`LEVIATHAN OFFSEC`).
- Right: Monospace navigation links (`TOOLS`, `RESEARCH`) + High-contrast accent CTA (`GITHUB`).

### 2. Symmetrical Tool Card
- Header: Tool name (`JetBrains Mono`, `font-weight: 700`) + category badge pill.
- Body: 2-3 sentence technical description (`flex-grow: 1`).
- Install Box: Terminal background with green prompt (`$`), command, and instant one-click `Copy` button.
- Pinned Footer: Baseline-aligned repository link (`→`), license tag (`MIT`), and language pill (`Go | CLI`).

### 3. Pipeline / Terminal Demo
- Simulated macOS / Linux terminal header with red, yellow, green window controls and badge.
- Monospace output showing stdin/stdout chaining (`httpx | surfacediff | hostage`).

### 4. Long-form Research Dossier Layout
- Category pill + publication timestamp + read time.
- Responsive dark-bordered comparison tables (`.table-wrap`).
- Callout boxes with accent left border (`border-left: 3px solid var(--accent)`).
- Author bio dossier box linking directly to personal and organization profiles.

---

## Agent Build Instructions
When generating or refactoring pages for this stack:
1. Always keep CSS scoped and consistent using CSS custom properties (`:root`).
2. Ensure every grid row has equal-height cards using `display: flex; flex-direction: column` and `flex-grow: 1` on descriptions.
3. Every route must support both clean directory URLs (`/path/`) and direct `.html` endpoints using `jekyll-redirect-from`.
4. Ensure dark contrast standards: never use pure black text on dark backgrounds; maintain `#CBD5E1` on `#07090E`.

## Rerun Inputs
```yaml
workflow: firecrawl-website-design-clone
source_url: https://projectdiscovery.io
target_stack: Jekyll / GitHub Pages (Vanilla CSS + HTML5)
output: DESIGN.md
```
