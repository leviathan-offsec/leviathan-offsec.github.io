# Brand

One identity for leviathan.ac, the tool READMEs, and the Hermes dashboard.
Tokens live in [`assets/brand.css`](assets/brand.css) and nowhere else. If a
colour or a font needs to change, it changes there and every surface inherits it.

## Positioning

**Leviathan Offsec builds tools that report what they could not evaluate.**

That is the whole claim, and it is the one thing the portfolio has in common that
no competitor does. Every tool here prints a coverage line:

```
database coverage  3/7 plugins matched  4 with no advisory data
```

```
edges built            312
  condition-evaluated  198
  condition-partial     74   unknown condition key, edge unverified
  condition-blocking    40   evaluated, edge excluded
paths to admin           6
  fully verified         4
  unverified             2   show the blocking condition key
```

A scanner that cannot say what it failed to check is asking to be trusted on
faith. Ours print the gap.

## Name and voice

- Organisation: **Leviathan Offsec**, written out. `LEVIATHAN OFFSEC` only where
  the surrounding design is already shouting.
- Tools keep the `LVX` suffix for the Go family: `HostageLVX`, `FenrirLVX`,
  `PrivEscLVX`. Python tools are plain lowercase: `leviathan-core`, `surfacediff`.
- One line, one claim, then the number that backs it. No adjectives doing work a
  counter could do.

## Palette

| Token | Value | Means |
| --- | --- | --- |
| `--accent` | `#00ffcc` | brand, the finding, the live edge |
| `--green` | `#10b981` | evaluated and permitted |
| `--amber` | `#eab308` | could not be evaluated, reported not hidden |
| `--slate` | `#94a3b8` | evaluated and refused, excluded on purpose |
| `--red` | `#f43f5e` | the thing you are looking for |
| `--blue` | `#38bdf8` | structure, graph shape, counts |
| `--orange` | `#fb923c` | elevated, needs a human |

The semantic set is not decoration. `--amber` exists specifically so the
"could not evaluate" count can be coloured differently from a pass, which is the
visual half of the thesis.

## Type

- **JetBrains Mono** for every number, every counter, every path, every ARN.
  Data is monospace so columns line up and a changed digit is visible.
- **Inter** for prose. Nothing else.

## Components

| Class | Use |
| --- | --- |
| `.evidence` | the coverage block, `data-label` sets the header |
| `.evidence .ok` `.warn` `.skip` `.bad` `.info` `.dim` | inline state colouring |
| `.chain` | an escalation path, one hop per line |
| `.pill--live` `.pill--unverified` `.pill--blocked` `.pill--admin` | dashboard status |

## Rules

1. A number appears with the count of things it excludes. No bare totals.
2. Unknown never renders as zero. If it was not evaluated, it says so.
3. Fail closed on permissions. An unreadable Deny is treated as a Deny.
4. Passive by default. Nothing is sent to a host you have not named.
5. Negative results get published. A post-mortem of three real bugs and $0 is
   more useful than another win, because it is checkable.
6. No em-dashes in authored copy. Use a comma, a colon, or a new sentence.
7. No "delve", "leverage", "seamless", "robust", "game-changing".

## Surfaces

| Where | How it loads the tokens |
| --- | --- |
| leviathan.ac | `<link rel="stylesheet" href="/assets/brand.css">` |
| Tool READMEs | raw link to `assets/brand.css` plus a `.evidence` block |
| Hermes dashboard | import the same file, do not re-declare the palette |

Implementation detail, type scale, component anatomy and the provenance of the
base palette are in [`DESIGN.md`](DESIGN.md). If `brand.css` and `DESIGN.md`
disagree, `brand.css` wins.
