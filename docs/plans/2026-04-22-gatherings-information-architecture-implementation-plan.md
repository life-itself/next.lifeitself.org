# Gatherings Information Architecture Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Create `/gatherings` as the canonical home for the Gathering series, move the 2026 UK Gathering page there, link to it from `/events`, and add redirects plus publishing safeguards.

**Architecture:** Keep `/events` as the broad event hub while introducing `/gatherings` as the series landing page and archive. Move the existing 2026 event page into `gatherings/` rather than duplicating it, then preserve the old event URL with a redirect in `config.json`. Exclude `/docs` from Flowershow publishing before any push so planning documents stay internal.

**Tech Stack:** Flowershow markdown site, `config.json` site config, markdown content pages, redirect configuration, Flowershow `contentExclude`

---

### Task 1: Exclude Internal Docs From Publishing

**Files:**
- Modify: `config.json`

**Step 1: Add the docs exclusion**

Add a top-level `contentExclude` entry in `config.json`:

```json
"contentExclude": [
  "/docs"
]
```

**Step 2: Verify the config contains the exclusion**

Run:

```bash
rg -n '"contentExclude"|"/docs"' config.json
```

Expected:

- one match for `contentExclude`
- one match for `"/docs"`

**Step 3: Commit**

```bash
git add config.json
git commit -m "chore: exclude docs from Flowershow publishing"
```

### Task 2: Create The Gatherings Landing Page

**Files:**
- Create: `gatherings.md`
- Reference: `events.md`
- Reference: `blog/our-2018-gathering.md`
- Reference: `blog/our-2019-gathering.md`
- Reference: `blog/announcing-gathering-2024.md`
- Reference: `events/gathering-2026-uk.md`

**Step 1: Draft the page frontmatter and intro**

Create `gatherings.md` with frontmatter and concise copy that:

- explains what the Gathering is
- frames it as a recurring Life Itself series
- links to current and past editions

Suggested frontmatter:

```md
---
title: Gatherings
description: The Life Itself Gathering is a recurring series of events bringing people together around wise living, community, and cultural renewal.
---
```

**Step 2: Add the editions list**

Include links for:

- 2018
- 2019
- 2022
- 2024
- 2026 UK

Use direct links to canonical edition pages when they exist, and recap/announcement posts where a dedicated edition page does not yet exist yet.

**Step 3: Verify the page exists and references the target years**

Run:

```bash
rg -n '2018|2019|2022|2024|2026' gatherings.md
```

Expected:

- matches for all five year references

**Step 4: Commit**

```bash
git add gatherings.md
git commit -m "feat: add gatherings landing page"
```

### Task 3: Move The 2026 UK Gathering Page To The Canonical Location

**Files:**
- Move: `events/gathering-2026-uk.md` -> `gatherings/2026-uk.md`

**Step 1: Move the file**

Move the existing file into the `gatherings/` directory without changing its content except where links or copy need minor canonical URL alignment.

**Step 2: Verify the old file is gone and the new file exists**

Run:

```bash
test -f gatherings/2026-uk.md && test ! -f events/gathering-2026-uk.md
```

Expected:

- command exits successfully with no output

**Step 3: Verify the moved page still contains the expected title**

Run:

```bash
rg -n '^title: Life Itself UK Gathering' gatherings/2026-uk.md
```

Expected:

- one title match in `gatherings/2026-uk.md`

**Step 4: Commit**

```bash
git add gatherings/2026-uk.md events/gathering-2026-uk.md
git commit -m "feat: move 2026 gathering page into gatherings"
```

### Task 4: Add A Gatherings Section To The Events Page

**Files:**
- Modify: `events.md`

**Step 1: Add a dedicated Gatherings heading**

Add a short section near the top of `events.md` that:

- introduces Gatherings as a recurring Life Itself series
- links to `/gatherings`
- optionally highlights the current 2026 UK Gathering

Suggested shape:

```md
## Gatherings

Life Itself's Gatherings are recurring events for deeper connection, reflection, and shared exploration.

- [Explore all Gatherings](/gatherings)
- [Current edition: UK Gathering 2026](/gatherings/2026-uk)
```

**Step 2: Verify the section exists**

Run:

```bash
rg -n '^## Gatherings|/gatherings|/gatherings/2026-uk' events.md
```

Expected:

- one section heading match
- link match for `/gatherings`
- link match for `/gatherings/2026-uk`

**Step 3: Commit**

```bash
git add events.md
git commit -m "feat: add gatherings section to events page"
```

### Task 5: Create Or Normalize Historical Gathering Edition Pages

**Files:**
- Create: `gatherings/2018.md`
- Create: `gatherings/2019.md`
- Create: `gatherings/2022.md`
- Create: `gatherings/2024.md`
- Reference: `blog/our-2018-gathering.md`
- Reference: `blog/our-2019-gathering.md`
- Reference: `programs/gathering.md`
- Reference: `blog/announcing-gathering-2024.md`
- Reference: `gathering/2019-gathering.md`

**Step 1: Create the 2018 page**

Build `gatherings/2018.md` as a concise edition page using the existing 2018 recap as source material, with a link back to the full blog post if needed.

**Step 2: Create the 2019 page**

Build `gatherings/2019.md` using the current `gathering/2019-gathering.md` page and relevant recap material from `blog/our-2019-gathering.md`.

**Step 3: Create the 2022 page**

Build `gatherings/2022.md` using `programs/gathering.md` as the primary source.

**Step 4: Create the 2024 page**

Build `gatherings/2024.md` using `blog/announcing-gathering-2024.md` as the primary source.

**Step 5: Verify all four pages exist**

Run:

```bash
test -f gatherings/2018.md && test -f gatherings/2019.md && test -f gatherings/2022.md && test -f gatherings/2024.md
```

Expected:

- command exits successfully with no output

**Step 6: Commit**

```bash
git add gatherings/2018.md gatherings/2019.md gatherings/2022.md gatherings/2024.md
git commit -m "feat: add historical gathering edition pages"
```

### Task 6: Add Redirects For Legacy URLs

**Files:**
- Modify: `config.json`

**Step 1: Add the redirects**

Add these entries to the `redirects` array:

```json
{ "from": "/gathering", "to": "/gatherings" },
{ "from": "/gathering/2019-gathering", "to": "/gatherings/2019" },
{ "from": "/programs/gathering", "to": "/gatherings/2022" },
{ "from": "/events/gathering-2026-uk", "to": "/gatherings/2026-uk" }
```

**Step 2: Verify the redirects exist**

Run:

```bash
rg -n '/gathering"|/gathering/2019-gathering|/programs/gathering|/events/gathering-2026-uk' config.json
```

Expected:

- four redirect matches

**Step 3: Commit**

```bash
git add config.json
git commit -m "feat: add gathering redirects"
```

### Task 7: Update Legacy Gathering References In Content

**Files:**
- Modify: `blog/our-2018-gathering.md`
- Modify: `blog/sketches-of-a-future-society-part-one.md`
- Modify: `blog/leap-for-education-innovative-think-do-tank-taiwan.md`

**Step 1: Replace legacy external gathering links**

Update old `artearthtech.com/gathering` references to point to the new canonical destinations:

- general series references -> `/gatherings`
- year-specific references -> the relevant `gatherings/<year>` page when available

**Step 2: Verify no targeted legacy gathering links remain in those files**

Run:

```bash
rg -n 'artearthtech.com/gathering' blog/our-2018-gathering.md blog/sketches-of-a-future-society-part-one.md blog/leap-for-education-innovative-think-do-tank-taiwan.md
```

Expected:

- no matches

**Step 3: Commit**

```bash
git add blog/our-2018-gathering.md blog/sketches-of-a-future-society-part-one.md blog/leap-for-education-innovative-think-do-tank-taiwan.md
git commit -m "fix: update legacy gathering links"
```

### Task 8: Final Verification

**Files:**
- Verify: `config.json`
- Verify: `events.md`
- Verify: `gatherings.md`
- Verify: `gatherings/2018.md`
- Verify: `gatherings/2019.md`
- Verify: `gatherings/2022.md`
- Verify: `gatherings/2024.md`
- Verify: `gatherings/2026-uk.md`

**Step 1: Verify the target files exist**

Run:

```bash
test -f gatherings.md && test -f gatherings/2018.md && test -f gatherings/2019.md && test -f gatherings/2022.md && test -f gatherings/2024.md && test -f gatherings/2026-uk.md
```

Expected:

- command exits successfully with no output

**Step 2: Verify redirects, docs exclusion, and events linkages**

Run:

```bash
rg -n '"contentExclude"|"/docs"|/events/gathering-2026-uk|/gatherings/2026-uk|/gatherings"' config.json events.md gatherings.md
```

Expected:

- `contentExclude` and `"/docs"` in `config.json`
- redirect entries including `/events/gathering-2026-uk`
- `/gatherings` references in `events.md`

**Step 3: Review git diff**

Run:

```bash
git diff --stat
```

Expected:

- changed files limited to the planned config and content updates

**Step 4: Commit**

```bash
git add config.json events.md gatherings.md gatherings blog/our-2018-gathering.md blog/sketches-of-a-future-society-part-one.md blog/leap-for-education-innovative-think-do-tank-taiwan.md
git commit -m "feat: establish canonical gatherings information architecture"
```
