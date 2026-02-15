---
title: Learn
description: Explore key concepts and topics from Life Itself's work on inner development, cultural evolution, and the Second Renaissance.
syntaxMode: mdx
---

These are our learning resources — key concepts and topics that underpin Life Itself's work. Explore the ideas shaping the emerging Second Renaissance, from the metacrisis to contemplative practice, cultural evolution, and beyond.

```base
filters:
  file.inFolder("learn")
properties:
  note.title:
    displayName: Title
  note.description:
    displayName: Description
views:
  - type: cards
    name: "Learning Resources"
    image: note.image
    order:
      - file.name
      - title
      - description
    cardSize: 190
    imageAspectRatio: 1.6
```
