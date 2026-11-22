# sas-design: agent instructions

Canonical always-on rules for **Claude Code, Codex and Cursor** in this repo. `CLAUDE.md` loads this file.

## The source of truth is `../sas-site`

This repo holds design work for the Suits & Sandals website. The site itself, its CMS, its tokens and its blocks live in the sibling repo **`sas-site`** (`/Users/milesroxas/SITES/sas-site`, the same parent folder as this repo). Every design decision here traces back to it.

**Before you design, name or change anything, read the matching source in `sas-site`.** Never invent a token, block, field or page that the site does not have. If the design should differ from the site, say so and treat the difference as a proposal.

Use the **`sas-site-reference`** skill (`.agents/skills/sas-site-reference/SKILL.md`). It maps each design concern to the file in `sas-site` that owns it.

Short map:

| Design concern | Read in `sas-site` |
|----------------|-------------------|
| Project rules (voice, typography roles, Section and grid contracts) | `AGENTS.md` |
| Color palettes (light, dark, bands) | `src/app/(frontend)/globals.css` (`palette-light`, `palette-dark`, `[data-band]`), `src/styles/shadcn-theme.css` |
| Type scale, spacing, breakpoints, gutters, grid gap | `src/app/(frontend)/globals.css` `@theme` |
| Which element takes which type role | `src/blocks/shared/typography.ts` |
| Section spacing and band themes | `src/blocks/shared/section.tsx`, `src/blocks/shared/band-theme.ts` |
| 8-column block grid | `src/blocks/shared/grid.tsx`, `docs/block-grid-roadmap.md` |
| Blocks (one folder each: `config.ts` = CMS fields, `Component.tsx` = render, `*.stories.tsx` = examples) | `src/blocks/` |
| Heroes | `src/heros/` |
| Header, footer, menu | `src/Header/`, `src/Footer/`, `src/components/SiteChrome/` |
| Singleton pages (Payload globals) | `src/payload.config.ts` `globals`, `src/Home/`, `src/CollectionIndexes/` |
| Collections with public URLs | `src/payload.config.ts` `collections`, `src/collections/` |
| Routes on the live site | `src/app/(frontend)/` |
| House voice for any copy | `docs/editorial/voice.md` |

Live content: the **sas-cms** MCP server reads the production CMS (`findHome`, `findWorksIndex`, `findPosts`, `outlineDocument`, ...). Read only. Never write to the CMS from this repo unless the user asks in the conversation. The live site is `https://www.suits-sandals.com`; its sitemaps (`/pages-sitemap.xml`, `/works-sitemap.xml`, ...) list what is published.

## Non-negotiables

- Never use em dashes in prose, copy or placeholder content (the sas-site rule). Recast with a comma, colon, parentheses or a period.
- Follow the house voice in `sas-site/docs/editorial/voice.md` for any copy you write.
- Keep this repo source-only. See `README.md` for what `.gitignore` and the pre-commit hook keep out.

## The Elyx project (`elyx/`)

Use the **`elyx`** skill before you read or edit any `.elyx` file. The project layout:

| Folder | What lives there | Mirrors in `sas-site` |
|--------|------------------|----------------------|
| `elyx/tokens/` | `colors` (+ `colors-dark`, theme=dark), `type-size` (+ `type-size-mobile`, media=mobile), `type` (families, styles, leading, tracking), `space` (spacing steps, radius), `layout` (+ `layout-mobile`: viewport, gutter, grid spans, section rhythm, chrome heights) | `globals.css` `@theme` and palettes |
| `elyx/foundations/` | `tokens.elyx`: a specimen of every color token and the seven type roles. Render it to check a token change. | |
| `elyx/resources/fonts/` | Geist and Geist Mono (variable TTFs from the `geist` package) | `geist` package |
| `elyx/components/` | Shared pieces: button, eyebrow, media placeholder, cards, header, footer | `src/components/`, `src/Header/`, `src/Footer/` |
| `elyx/heroes/` | Hero bands | `src/heros/` |
| `elyx/blocks/` | One file per CMS block, named for its block slug | `src/blocks/<block>/` |
| `elyx/singletons/` | One screen per Payload global that is live: home, works, insights, lab, expertise, who-we-help | `src/payload.config.ts` `globals` |
| `elyx/collections/<collection>/` | One folder per public collection: `pages`, `posts`, `works`, `lab`, `expertise`, `who-we-help`, `contact` | `src/collections/` |

Rules for the Elyx project:

- A block's Elyx symbol and file name follow its Payload block slug (`featureHeadingOffset` lives in `blocks/feature-heading-offset.elyx`). Keep them in step when the site renames a block.
- Screens compose block and component instances. Never rebuild a block inside a screen.
- Colors, type and lengths come from `tokens/`. Never hard-code a value the tokens hold.
- Context axes: `theme=light|dark` (colors) and `media=desktop|mobile` (type scale and gutters). Desktop values are the 1440px end of the site's fluid clamps; mobile values are the 375px end.
- When the site adds, renames or removes a block, global or collection, update the matching Elyx file and the tables above.
- Every layer file that sets text starts with `use "resources/fonts/Geist-Variable.ttf"` and `use "resources/fonts/GeistMono-Variable.ttf"`. Without them diagnostics report `Unknown Font Family` and renders fall back to another font. `normalize` strips `use` lines from token-only files, so the lines can't live in `tokens/type.elyx`.
- `elyx.json` sets `"color": {"encoding": "oklch"}` so tokens keep the site's oklch values. `normalize` gamut-maps a color outside sRGB (the site's primary and brand) to the nearest sRGB oklch value, so a few chroma values differ slightly from `globals.css`. Read the site's value from `globals.css`, not from the token file.
- Put a file's header comment directly above its first declaration, after the imports and `file {}` block. `normalize` drops comments that sit above the imports.
- In a contextual token file (for example `colors-dark.elyx`), restate every alias member. An inherited alias resolves in the scope where it was written, so it keeps the light value.
- Render with `elyx --workspace . render ...` from `elyx/`, and check with `elyx diagnostics .`.
