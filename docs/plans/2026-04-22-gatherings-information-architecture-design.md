---
title: Gatherings Information Architecture Design
date: 2026-04-22
status: approved
---

# Gatherings Information Architecture Design

## Summary

Life Itself should treat Gatherings as a flagship series with its own canonical home at `/gatherings`, while keeping `/events` as the broad hub for current and upcoming event activity.

The 2026 UK Gathering page should be moved into the new `gatherings/` collection so that the live announcement page becomes the archival page for that edition in its canonical long-term location.

## Current State

Today, the visible landing page is `/events`, because the main navigation links there from `config.json`.

There is no dedicated gatherings landing page yet:

- no `gatherings.md`
- no `gatherings/index.md`
- no `gathering.md`
- no `gathering/index.md`

Gathering-related content is currently split across multiple locations:

- current live event page: `events/gathering-2026-uk.md`
- older standalone page: `gathering/2019-gathering.md`
- legacy program page: `programs/gathering.md`
- recap / announcement posts in `blog/`

This creates a fragmented URL and editorial model. The site currently has no single place that explains the Gathering as a recurring Life Itself format across years.

## IA Decision

### Canonical Series Home

Use `/gatherings` as the canonical editorial home for the Gathering series.

This page should do four jobs:

- explain what the Gathering is
- show its history across years
- link to current and past editions
- act as the main browse destination for anyone looking for "Life Itself Gatherings"

Plural is the correct form because the page is about a recurring series with multiple editions.

`/gathering` should not be the main destination. It should be used only as a legacy redirect.

### Relationship Between `/events` and `/gatherings`

`/events` and `/gatherings` serve different user intents:

- `/events` answers: what is happening now or soon?
- `/gatherings` answers: what is the Gathering, and how has it evolved over time?

This is a stronger information architecture than nesting Gatherings only under `/events/gatherings`, because Gatherings are not merely a generic event subtype. They behave more like an enduring named series with archival value.

### Edition Page Policy

Going forward, new Gathering edition pages should normally live under `gatherings/`.

Recommended structure:

- `gatherings.md`
- `gatherings/2018.md`
- `gatherings/2019.md`
- `gatherings/2022.md`
- `gatherings/2024.md`
- `gatherings/2027.md`
- `gatherings/2027-uk.md` when location disambiguation is needed

### Existing 2026 Page

The existing 2026 UK Gathering page should be moved from:

- `events/gathering-2026-uk.md`

to:

- `gatherings/2026-uk.md`

Reason:

- it should live in the canonical long-term home for Gathering editions
- the same page can still do both jobs: live event page first, archival page later
- this creates a clean and consistent rule for the team

So the rule is:

- the 2026 page moves into `gatherings/`
- `/events` links to `/gatherings` and highlights the current gathering there
- future editions should be created in `gatherings/` by default

## Content Model

### `/events`

`/events` remains the main site-level events hub.

It should continue to cover:

- current and upcoming events
- calendar and external event tools
- broad discovery across event types

It should also include a dedicated Gatherings section that links prominently to `/gatherings` as the archive and series page for the Gathering.

### `/gatherings`

`/gatherings` should be the editorial landing page for the series.

It should contain:

- a short explanation of what the Gathering is
- a list or timeline of editions
- links to recap posts where a dedicated edition page does not yet exist
- a prominent link to the current or most recent edition

### Individual Editions

Each edition page should ideally become the durable record for that year:

- before event: announcement and logistics
- during event: current event page
- after event: archival page with recap context, media, and next-step links if relevant

This supports the user's preference to avoid multiple pages for the same edition.

## Redirect Strategy

Add redirects in `config.json`, which already contains a `redirects` array.

At minimum:

- `/gathering` -> `/gatherings`
- `/gathering/2019-gathering` -> `/gatherings/2019`
- `/programs/gathering` -> `/gatherings/2022`

Also add:

- `/events/gathering-2026-uk` -> `/gatherings/2026-uk`

This preserves the existing live URL while moving the canonical page into the new Gatherings structure.

## Legacy Content Cleanup

There are older content references to `artearthtech.com/gathering/...` in blog posts and related pages. These should be updated where practical.

Known examples include:

- `blog/our-2018-gathering.md`
- `blog/sketches-of-a-future-society-part-one.md`
- `blog/leap-for-education-innovative-think-do-tank-taiwan.md`

Cleanup goal:

- replace dead or legacy external gathering links with current Life Itself destinations where appropriate
- prefer `/gatherings` or specific edition pages when those exist

## Publishing Safeguard For `docs/`

This repository uses Flowershow. We want `docs/plans` to remain internal and not be published to the live site.

Before committing and pushing any docs-based planning workflow, update `config.json` to exclude `/docs` from publishing.

Per Flowershow official documentation, this should be done with:

- `contentExclude`

Recommended config addition:

```json
{
  "contentExclude": [
    "/docs"
  ]
}
```

Official references:

- Flowershow config docs: https://flowershow.app/docs/config
- Flowershow content filtering docs: https://flowershow.app/docs/content-filtering

## Implementation Notes

Implementation should proceed in this order:

1. Add `contentExclude: ["/docs"]` to `config.json`.
2. Create `gatherings.md` as the series landing page.
3. Move `events/gathering-2026-uk.md` to `gatherings/2026-uk.md`.
4. Add a Gatherings section in `events.md` linking to `/gatherings`.
5. Create or normalize edition pages under `gatherings/` for historical years.
6. Add redirects for legacy URLs, including `/events/gathering-2026-uk`.
7. Update old internal content references that still point to outdated gathering URLs.

## Final Recommendation

Adopt a dual model:

- `/events` for current event discovery and operations
- `/gatherings` for the enduring Gathering series and archive

Move the 2026 UK Gathering page into `gatherings/2026-uk.md` and preserve the old event URL with a redirect.

For Gathering editions going forward, create pages under `gatherings/` by default.
