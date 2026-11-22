---
name: sas-site-reference
description: Ground every sas-design task in the sas-site repo (../sas-site), the code and CMS behind the live Suits & Sandals website. Use before designing, naming or changing tokens, components, blocks, heroes or page screens in sas-design, including the Elyx project, prototypes and studio films. Triggers on: token, color, palette, type scale, block, section, hero, header, footer, page, screen, singleton, global, collection, Payload, CMS, live site, suits-sandals.com.
---

# sas-site reference

`sas-design` holds design work. `sas-site` holds the truth: the Next.js and Payload code that renders `https://www.suits-sandals.com`. Find it at `../sas-site` from this repo's root (`/Users/milesroxas/SITES/sas-site`).

A design that drifts from the site is a design nobody can ship. So every token, block, field and page in this repo traces to a file in `sas-site`, and you read that file before you design.

## Process

1. Name the concern (a color, a block, a page).
2. Open the owning file in `sas-site` from the map below. Read it, not your memory of it: the site changes often (`git -C ../sas-site log --oneline -10`).
3. For content, read the live CMS through the **sas-cms** MCP server (read tools only: `find*`, `outlineDocument`, `getBlock`). Never call a `create*`, `update*` or `delete*` tool from this repo unless the user asks for that write in the conversation.
4. Design with the site's names. A block keeps its Payload slug, a field keeps its field name, a page keeps its route.
5. If the design should differ from the site, say so and label it a proposal. Do not change `sas-site` from this repo unless the user asks.

## Map

### Foundations

| Concern | File in `sas-site` |
|---------|-------------------|
| Project rules: voice, type roles, Section, grid | `AGENTS.md` |
| Light and dark palettes, band surfaces | `src/app/(frontend)/globals.css`: `palette-light`, `palette-dark`, `[data-band="dark"]`, `[data-band="neutral"]` |
| Token map (Tailwind color names to palette vars), polarity model, press and ease tokens | `src/styles/shadcn-theme.css` |
| Type scale, breakpoints, gutter, grid gap, content widths | `src/app/(frontend)/globals.css` first `@theme` block (`--text-display` ... `--text-code`) |
| Which element takes which type role | `src/blocks/shared/typography.ts` |
| Section spacing scale (`none`, `tight`, `normal`, `loose`) | `src/blocks/shared/section.tsx` |
| Band themes (`default`, `inverted`, `neutral`, `brand`) | `src/blocks/shared/band-theme.ts` |
| 8-column grid | `src/blocks/shared/grid.tsx`, `docs/block-grid-roadmap.md` |
| UI primitives (button, badge, tabs, ...) | `src/components/ui/` |
| Fonts | Geist Sans and Geist Mono (`geist` package), base weight 380 |

### Blocks and heroes

Each block is a folder in `src/blocks/`: `config.ts` holds the CMS fields and the slug, `Component.tsx` renders it, and `*.stories.tsx` shows real examples with fixtures from `src/blocks/fixtures.ts`.

- Top-level page blocks: `src/blocks/RenderBlocks.tsx` (`archive`, `audienceTabs`, `cta`, `dynamicAudience`, `featureStatementGrid`, `featureStatementLinks`, `featuredWork`, `formBlock`, `industryWork`, `newsletterSignup`, `testimonialsMarquee`, plus `section`).
- Blocks a Section nests: `src/blocks/shared/content-block-renderer.tsx` (`sectionChildComponents`).
- Work page blocks: `src/blocks/case-study/`. Lab page blocks: `src/blocks/lab/`.
- Heroes: `src/heros/` (`HighImpact`, `MediumImpact`, `LowImpact`, `PostHero`, `SegmentHero`, `CaseStudyHero*`, `LabHero`).
- Taxonomy and phases of the block set: `docs/blocks-reorg-roadmap.md`.

### Pages

| Kind | Where |
|------|-------|
| Singletons (Payload globals): Home, Works index, Insights index, Lab index, Expertise index, Who we help index | `src/payload.config.ts` `globals`; `src/Home/`, `src/CollectionIndexes/` |
| Chrome globals: Header, Footer | `src/Header/`, `src/Footer/` |
| Public collections: Pages, Posts, Work pages, Lab pages, Expertise pages, Audience pages (who we help), Contact pages | `src/payload.config.ts` `collections`; `src/collections/<Name>/` |
| Routes | `src/app/(frontend)/` (`[slug]`, `posts/[slug]`, `works/[slug]`, `lab/[slug]`, `expertise/[slug]`, `who-we-help/[slug]`, `contact/[slug]`) |
| What is published | live sitemaps: `/pages-sitemap.xml`, `/posts-sitemap.xml`, `/works-sitemap.xml`, `/lab-sitemap.xml`, `/expertise-sitemap.xml`, `/who-we-help-sitemap.xml` |

### Copy

- House voice: `docs/editorial/voice.md`. Never an em dash.
- CMS naming (tabs, groups, block labels): `docs/cms-naming.md`.

## Keeping sas-design in step

When `sas-site` adds, renames or removes a token, block, global or collection, update the matching file in `sas-design` (for Elyx: `elyx/tokens/`, `elyx/blocks/`, `elyx/singletons/`, `elyx/collections/`) and the tables in `AGENTS.md`.
