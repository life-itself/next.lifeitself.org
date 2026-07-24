---
title: Festival Website Migration
date: 2026-07-23
status: approved
---

# Festival Website Migration

## Summary

Migrate the historical 2025 Second Renaissance Festival from WordPress.com into
the main Flowershow site at `/festival`. Preserve the warm pink/brown visual
identity and three-page structure while removing WordPress chrome, stale 2026
material, and inactive booking calls.

## Implementation

- Create `/festival`, `/festival/info`, and `/festival/tickets` as responsive
  Tailwind/HTML pages inside Markdown, with a shared festival-local navigation.
- Use the original pale-pink, blush, dark-brown, and green palette and retain
  the source site's image-led, playful character.
- Normalize all event copy to the inaugural festival on 5–7 September 2025,
  label it as a past event, and remove the inactive Tito booking link.
- Download only the images used by the retained Home, Info, and Tickets pages
  into `assets/images/festival/`; do not retain WordPress asset dependencies.
- Add a festival feature to the main Events page.
- Do not migrate the abandoned 2026 draft, empty Gallery page, WordPress
  comments, account links, or generated WordPress UI.

## URLs and Cutover

The canonical URLs are:

- `https://lifeitself.org/festival`
- `https://lifeitself.org/festival/info`
- `https://lifeitself.org/festival/tickets`

After deployment, configure permanent host-level redirects:

- `festival.lifeitself.org/` → `lifeitself.org/festival`
- `festival.lifeitself.org/events/` → `lifeitself.org/festival/info`
- `festival.lifeitself.org/tickets/` → `lifeitself.org/festival/tickets`
- all other legacy festival paths → `lifeitself.org/festival`

The redirect belongs at the DNS/Cloudflare layer because the source and
destination use different hostnames. Do not cut over DNS until all destination
pages return successfully.

## Validation

- Confirm all three pages render without Markdown/HTML parsing errors.
- Confirm the festival pages contain no `2026`, WordPress asset URLs, stale
  booking language, or Tito links.
- Confirm every local image reference exists and all internal navigation links
  resolve.
- Compare desktop and mobile renders with the source site's section order,
  imagery, palette, and overall character.
- After deployment, verify the canonical URLs return 200; after cutover, verify
  the old hostname returns permanent redirects.

## Assumptions

- This is an archive of the inaugural 2025 festival, not promotion for a future
  event.
- The authoritative dates are 5–7 September 2025 and the audience size was
  approximately 20–30 participants.
- The inactive Tito listing is not linked.
- Flowershow supplies the main Life Itself navbar and footer.
