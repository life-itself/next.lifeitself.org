---
title: Initiatives
description: Projects and initiatives started by Life Itself, past and present, from hubs and residencies to research programmes and podcasts.
syntaxMode: mdx
---

These are the projects and initiatives Life Itself has started or helped to start, past and present.

```base
filters:
  file.inFolder("initiatives")
properties:
  note.title:
    displayName: Title
  note.description:
    displayName: Description
views:
  - type: cards
    name: "Initiatives"
    image: note.image
    order:
      - title
      - description
    cardSize: 190
```
