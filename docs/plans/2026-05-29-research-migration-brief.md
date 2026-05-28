---
title: Research Site Migration Brief
date: 2026-05-29
status: draft
---

# Brief: Merge research.lifeitself.org → lifeitself.org/research

## Goal

Consolidate the standalone research subdomain (`research.lifeitself.org`) into the main site at `lifeitself.org/research`, reducing maintenance overhead and improving visibility of research work.

## Current State

### research.lifeitself.org
Standalone Flowershow site with:
- **posts/** — 2 posts (presentations, 2025 strategy)
- **projects/** — 9 project pages (web3, polycrisis mapping, environmental data, etc.)
- **publications/** — 10 publications (white papers, reports)
- **discovery-calls.md** — sign-up/info page
- Config: sidebar enabled, "lessflowery" theme, navbar links to newsletter/forum/calls

### lifeitself.org/research (current)
- `research.md` — main research page describing collective, approach, recent outputs
- `research/papers.md` — white papers list (partially overlapping with publications/)

## Key Pain Points (from "Where Next for 2025" post)
- Hard to see what research outputs exist on lifeitself.org/research
- Insufficient visibility of outputs, members, activities
- Unclear pathways for participation

## Content Inventory

| Content Type | research.lifeitself.org | lifeitself.org/research |
|---|---|---|
| Home/overview | index.md | research.md |
| Projects | 9 project pages | none |
| Publications | 10 publications | papers.md (3 white papers, partial overlap) |
| Posts/updates | 2 posts | none |
| Discovery calls | discovery-calls.md | links to community calendar |

## Migration Scope

Move all content from research.lifeitself.org into lifeitself.org under `/research/`:
- `/research/` — overview page (merge research.md + research/index.md content)
- `/research/projects/` — 9 project pages
- `/research/publications/` — 10 publications
- `/research/posts/` — 2 posts
- `/research/calls` — discovery calls page

## Decision: Should We Merge?

**Yes. Merge research.lifeitself.org into lifeitself.org/research.**

### Reasons

1. **Content is thin and largely archival.** ~21 pages total; most projects/publications are 2019–2023 (web3, polycrisis, environmental data). Not enough to justify a standalone site.
2. **Active output has migrated.** Ongoing research now publishes as white papers at secondrenaissance.net. research.lifeitself.org is no longer the live output destination.
3. **One less site to maintain.** Separate subdomain = separate deployment, config, and upkeep for minimal gain.
4. **Visibility problem is real.** The community's own "where next for 2025" review explicitly notes outputs are hard to find on lifeitself.org/research. Merging and redesigning the page fixes this directly.
5. **Cleaner story.** Life Itself Research and Second Renaissance are the same community and continuous thread of work. One URL, one story.

### Action
- Shut down research.lifeitself.org
- 301 redirect all research.lifeitself.org/* → lifeitself.org/research (or specific sub-pages)
- Fold content into lifeitself.org/research/

## How to Merge

### Information Architecture

Keep sub-pages — publications and projects have real depth (multiple sections, links, context) that can't be collapsed to a list without losing value.

```
/research/                    ← hub: story, join, link to secondrenaissance.net
/research/projects/           ← index of 9 projects
/research/projects/[slug]     ← individual project pages (moved as-is)
/research/publications/       ← index of 10 publications
/research/publications/[slug] ← individual publication pages (moved as-is)
```

### Phasing

**Phase 1 (migration):** Move content as-is. Minimal changes. Get everything under lifeitself.org/research. Set up 301 redirects from research.lifeitself.org.

**Phase 2 (refinement):** Improve hub page narrative, surface selected secondrenaissance.net papers on /research with link out for full list, review framing of older vs newer publications.

### Notes
- Publications span 2019–2025, not all archival — avoid blanket "past work" framing
- Active research output now at secondrenaissance.net; /research hub should reference and link there

## Redirects

Full DNS control confirmed. Plan:
- Keep research.lifeitself.org live as 301 redirects for ≥6 months after migration
- research.lifeitself.org/* → lifeitself.org/research/* (path-preserving where possible)
- research.lifeitself.org/ → lifeitself.org/research/
- After 6 months: reassess, then optionally let domain go dark
