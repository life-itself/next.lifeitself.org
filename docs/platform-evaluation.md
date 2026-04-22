---
created: 2026-04-11
authors:
  - rufuspollock
---

# Platform Evaluation for Website Publishing

What technological platform should we use to publish this website?

## Options

1. **WordPress**
2. **Astro**
3. **Flowershow** (current platform)

## Notes

- Backend content is Markdown, HTML, etc. regardless of platform choice
- Astro and Next.js are similar in what they offer for this use case
- Flowershow currently uses Next.js under the hood
- Key question: what are the jobs to be done that should drive this decision?

## Initial Analysis (2026-04-11)

### Current state of the codebase

~594 markdown files, no custom JS/TS, a simple `config.json`, minimal custom CSS (two button classes). This is a pure content site with essentially zero application logic. Flowershow provides nav, footer, Tailwind, ToC, sidebar, YouTube embeds, redirects — all from config, no code.

### Against WordPress

- Moving ~600 markdown files into a database-backed CMS is a regression for a team that thinks in markdown/git.
- Lose git-based version control, diffing, editing in any text editor.
- WordPress makes sense when non-technical editors need a GUI or you need a plugin ecosystem (e-commerce, membership, forms).
- **Verdict: ruled out.** Too far from what we want. Comments can be handled with Giscus. We have search.

### Flowershow vs. Astro — the real question

Flowershow is currently *carrying* us — lots of value for zero maintenance. Moving to bare Astro means building/maintaining all that scaffolding ourselves. We gain full control but take on more work.

### Key questions

1. **What's frustrating about the current setup?** If there's no acute pain, switching is cost with no payoff.
2. **Who needs to edit?** If just developers/markdown-comfortable people, Flowershow or Astro is fine. Non-technical contributors would point to a CMS layer.
3. **Dynamic features needed?** Membership, auth, e-commerce — WordPress has plugins, Astro/Flowershow would need integrations.
4. **Is Flowershow actively maintained?** If development has slowed, depending on it is a risk vs. community-backed Astro.
5. **Performance/SEO?** Static generators win by default. Not a differentiator between Flowershow and Astro.
6. **12-month vision?** Content-focused = stay. Interactive features = Astro islands. Lots of non-technical editors = WordPress.

### The actual pain point (Rufus, 2026-04-11)

The main frustration is **creating lists/cards from markdown catalogues**. For example:

- The **team page** requires hand-writing card HTML for each person, duplicating data that already lives in individual people markdown files. Every update needs to happen in two places.
- **Podcasts** will have the same pattern — a catalogue of episodes that need to render as a list with cards.
- This is a recurring need: data lives in markdown files with frontmatter, and we need templated list views that render cards from that data automatically.

Flowershow could improve here but **doesn't currently support custom templates or collection-based layouts** — you can't say "render all files in `/people/` as cards using this template."

### Response: this is a real and specific pain, and it points toward Astro

This is exactly the kind of thing that tips the balance. The pattern you're describing — "I have a folder of markdown files with frontmatter, render them as a templated list" — is a **first-class feature in Astro** via content collections and `Astro.glob()`. In Astro you would:

1. Define a collection (e.g. `people/`) with a schema for the frontmatter fields
2. Write a single card component
3. Query the collection, loop, done — one source of truth, no duplication

In Flowershow/Next.js, this requires either custom code in the framework itself (which you'd need to maintain as a fork or upstream) or the hand-written HTML approach you're doing now.

The key consideration: **how many of these collection patterns do you have?** If it's just team + podcasts, the migration cost may not be worth it yet — you could manage with a build script that generates the HTML. But if you see this pattern recurring (blog cards, research projects, events, hubs), that's a strong signal that Astro's content model is what you actually need.

The migration itself wouldn't be terrible: your content is already clean markdown with frontmatter, no custom JS. The main work would be recreating the layout/nav/footer in Astro components — a one-time cost.
