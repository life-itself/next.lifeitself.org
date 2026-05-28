# Research Site Migration — Phase 1 Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Move all content from research.lifeitself.org (../research/) into lifeitself.org/research/ as-is, with index pages and working internal links.

**Architecture:** Copy files from `../research/` into `research/` subdirectory of this repo. Fix internal wikilinks. Create two index pages (projects, publications). Update the existing hub page (research.md). Add redirects to config.json for when research.lifeitself.org is retired.

**Tech Stack:** Markdown + Flowershow. Wikilinks use `[[slug|label]]` syntax. Redirects via config.json. No build step needed locally — changes are visible after deploy.

**Source repo:** `../research/` (sibling directory)
**Target repo:** this repo (`next.lifeitself.org/`)

**GitHub issue:** https://github.com/life-itself/next.lifeitself.org/issues/28

---

## Task 1: Copy project pages

**Files:**
- Create: `research/projects/` (9 files from `../research/projects/`)

**Step 1: Copy all project files**

```bash
cp ../research/projects/*.md research/projects/
```

**Step 2: Verify**

```bash
ls research/projects/
```

Expected: 9 files — blind-spots.md, building-the-field-developmental-spaces.md, cohere-plus-ecosystem-mapping.md, embodying-collective-transformation.md, environmental-data-sharing-incentives.md, environmental-footprint-database-design.md, making-sense-of-web3.md, polycrisis-mapping.md, web3-for-local-government.md

**Step 3: Commit**

```bash
git add research/projects/
git commit -m "migrate: add research project pages from research.lifeitself.org"
```

---

## Task 2: Copy publication pages

**Files:**
- Create: `research/publications/` (10 files from `../research/publications/`)

**Step 1: Copy all publication files**

```bash
cp ../research/publications/*.md research/publications/
```

**Step 2: Verify**

```bash
ls research/publications/
```

Expected: 10 files — business-models-environmental-footprint-data.md, collective-intelligence.md, cryptocurrency-gfoa.md, embodying-collective-transformation-report.md, emergent-power-report.md, incentivising-environmental-data-sharing.md, polycrisis-mapping-report.md, polycrisis-mapping-stakeholder-needs-analysis.md, polycrisis-to-metacrisis-white-paper.md, real-deal-on-web-3-gfoa.md

**Step 3: Commit**

```bash
git add research/publications/
git commit -m "migrate: add research publication pages from research.lifeitself.org"
```

---

## Task 3: Copy posts and discovery-calls

**Files:**
- Create: `research/posts/` (2 files)
- Create: `research/calls.md`

**Step 1: Copy posts**

```bash
cp ../research/posts/*.md research/posts/
```

**Step 2: Copy discovery-calls as calls.md**

```bash
cp ../research/discovery-calls.md research/calls.md
```

**Step 3: Verify**

```bash
ls research/posts/ && cat research/calls.md | head -5
```

**Step 4: Commit**

```bash
git add research/posts/ research/calls.md
git commit -m "migrate: add research posts and calls page"
```

---

## Task 4: Copy research assets

The research site has images and PDFs referenced in publication/project pages.

**Files:**
- Create: `assets/research/` (copy from `../research/assets/`)

**Step 1: Copy assets**

```bash
cp -r ../research/assets/ assets/research/
```

**Step 2: Verify a few key files exist**

```bash
ls assets/research/ | head -10
```

**Step 3: Commit**

```bash
git add assets/research/
git commit -m "migrate: add research assets (images, PDFs)"
```

---

## Task 5: Fix internal wikilinks in project pages

Project pages use wikilinks like `[[../publications/polycrisis-mapping-report|label]]`. These break when moved because the relative path `../publications/` no longer resolves correctly in the new location.

Flowershow resolves wikilinks by slug (filename without extension), so `[[polycrisis-mapping-report|label]]` (no path prefix) will work once both files are in the same Flowershow instance.

**Files:**
- Modify: all files in `research/projects/*.md`

**Step 1: Check all wikilinks in project files**

```bash
grep -r "\[\[" research/projects/
```

**Step 2: Remove `../publications/` path prefixes from wikilinks**

```bash
sed -i '' 's/\[\[\.\.\/publications\//\[\[/g' research/projects/*.md
sed -i '' 's/\[\[\.\.\/projects\//\[\[/g' research/projects/*.md
```

**Step 3: Check all wikilinks in publication files**

```bash
grep -r "\[\[" research/publications/
```

**Step 4: Remove path prefixes from publication wikilinks**

```bash
sed -i '' 's/\[\[\.\.\/publications\//\[\[/g' research/publications/*.md
sed -i '' 's/\[\[\.\.\/projects\//\[\[/g' research/publications/*.md
```

**Step 5: Fix asset wikilinks (images referenced as `![[../assets/...]]`)**

```bash
grep -r "!\[\[" research/projects/ research/publications/
```

For any `![[../assets/filename.png]]` found, replace with standard markdown: `![](/assets/research/filename.png)`

**Step 6: Verify — no remaining `../` prefixes in wikilinks**

```bash
grep -r "\[\[\.\./" research/projects/ research/publications/ research/posts/
```

Expected: no output.

**Step 7: Commit**

```bash
git add research/projects/ research/publications/
git commit -m "fix: update internal wikilinks after research migration"
```

---

## Task 6: Create projects index page

**Files:**
- Create: `research/projects/index.md` (or `research/projects.md` — see note)

> Note: Flowershow serves `research/projects/index.md` at `/research/projects/`. Alternatively, `research/projects.md` works too since the folder contains the sub-pages. Use `research/projects.md` (alongside `research.md`) to keep top-level research pages flat.

**Step 1: Create the file**

Create `research/projects.md` with this content:

```markdown
---
title: Research Projects
description: Research projects by Life Itself, exploring paths to a wiser, weller world.
---

# Research Projects

Past and ongoing research projects from the Life Itself Research Collective.

- [[blind-spots|Blind Spots]] 
- [[building-the-field-developmental-spaces|Building the Field: Developmental Spaces]]
- [[cohere-plus-ecosystem-mapping|Cohere+ Ecosystem Mapping]]
- [[embodying-collective-transformation|Embodying Collective Transformation]]
- [[environmental-data-sharing-incentives|Environmental Data Sharing Incentives]]
- [[environmental-footprint-database-design|Environmental Footprint Database Design]]
- [[making-sense-of-web3|Making Sense of Web3 and Crypto]]
- [[polycrisis-mapping|Mapping Responses to the Polycrisis]]
- [[web3-for-local-government|Web3 for Local Government]]
```

**Step 2: Verify the titles match actual page titles**

```bash
grep "^title:" research/projects/*.md
```

Adjust list above if any titles differ.

**Step 3: Commit**

```bash
git add research/projects.md
git commit -m "add: research projects index page"
```

---

## Task 7: Create publications index page

**Files:**
- Create: `research/publications.md`

**Step 1: Create the file**

```markdown
---
title: Research Publications
description: White papers, reports and publications from the Life Itself Research Collective.
---

# Research Publications

White papers, reports and publications from the Life Itself Research Collective.

- [[business-models-environmental-footprint-data|Business Models for Environmental Footprint Data]]
- [[collective-intelligence|Collective Intelligence: Towards a Conversation]] (2020)
- [[cryptocurrency-gfoa|Cryptocurrency Report — GFOA]]
- [[embodying-collective-transformation-report|Embodying Collective Transformation Report]]
- [[emergent-power-report|Emergent Power Report]]
- [[incentivising-environmental-data-sharing|Incentivising Environmental Data Sharing]]
- [[polycrisis-mapping-report|A Boundary Makes a Map: Polycrisis Mapping Report]] (2023)
- [[polycrisis-mapping-stakeholder-needs-analysis|Polycrisis Mapping: Stakeholder Needs Analysis]] (2023)
- [[polycrisis-to-metacrisis-white-paper|From Polycrisis to Metacrisis — White Paper]]
- [[real-deal-on-web-3-gfoa|The Real Deal on Web3 — GFOA]]
```

**Step 2: Verify titles match actual files**

```bash
grep "^title:" research/publications/*.md
```

Adjust list to match actual titles.

**Step 3: Commit**

```bash
git add research/publications.md
git commit -m "add: research publications index page"
```

---

## Task 8: Update research.md hub page

The existing `research.md` already has good content. Add links to the new sub-sections (projects, publications, calls) without removing existing content.

**Files:**
- Modify: `research.md`

**Step 1: Read the current file**

```bash
cat research.md
```

**Step 2: Add navigation links**

Near the top of the page (after the intro paragraph, before "Research Approach and Outputs"), add:

```markdown
## Research Areas

- [Projects](/research/projects) — past and ongoing research projects
- [Publications](/research/publications) — white papers and reports
- [Discovery Calls](/research/calls) — weekly open calls, Fridays 5pm European
```

**Step 3: Verify the page reads well end-to-end**

Read `research.md` in full to check flow makes sense.

**Step 4: Commit**

```bash
git add research.md
git commit -m "update: add projects/publications/calls links to research hub"
```

---

## Task 9: Add redirects to config.json

When research.lifeitself.org is eventually retired, Flowershow's config.json redirects handle incoming links. For now these are internal redirects within lifeitself.org that future-proof any existing internal links using the old paths.

**Files:**
- Modify: `config.json`

**Step 1: Add redirects**

Open `config.json` and add to the `redirects` array:

```json
{ "from": "/research/discovery-calls", "to": "/research/calls" },
{ "from": "/research/posts/research-community-presentations", "to": "/research/posts/research-community-presentations" },
```

> Note: The external research.lifeitself.org → lifeitself.org redirect is a DNS/hosting-level 301, not a config.json redirect. config.json only handles within-site redirects. The DNS redirect is set up separately when research.lifeitself.org is retired.

**Step 2: Validate JSON is still valid**

```bash
python3 -c "import json; json.load(open('config.json')); print('valid')"
```

**Step 3: Commit**

```bash
git add config.json
git commit -m "config: add redirects for migrated research URLs"
```

---

## Task 10: Smoke test

Quick manual check that pages are accessible and links work after deploy.

**Step 1: Check pages exist in repo**

```bash
ls research/projects/*.md | wc -l   # expect 9
ls research/publications/*.md | wc -l  # expect 10
ls research/posts/*.md | wc -l  # expect 2
cat research/calls.md | head -3
cat research/projects.md | head -3
cat research/publications.md | head -3
```

**Step 2: Check no broken `../` wikilinks remain**

```bash
grep -r "\[\[\.\./" research/
```

Expected: no output.

**Step 3: Check assets copied**

```bash
ls assets/research/ | wc -l  # expect >10
```

**Step 4: Push and verify on live site**

```bash
git push
```

Then check:
- https://lifeitself.org/research
- https://lifeitself.org/research/projects
- https://lifeitself.org/research/publications
- https://lifeitself.org/research/calls
- One project page, one publication page

---

## Out of Scope (Phase 2)

- Improve hub page narrative (secondrenaissance.net integration, framing)
- Review/update stale content in individual pages
- DNS 301 redirect from research.lifeitself.org → lifeitself.org/research
- Surface selected secondrenaissance.net papers on hub
