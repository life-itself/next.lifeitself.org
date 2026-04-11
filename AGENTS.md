# AGENTS.md

This site is built in markdown and html and published with Flowershow https://flowershow.app.

File have frontmatter. HTML can either be mixed into markdown or we can have a markdown file that just has HTML. In either case the good part is that Flowershow will provide navbar and footer and other features automatically.

If we create a pure html page with html extension then that will be published exactly as it but without header and footer added.

## HTML and Tailwind

Flowershow has built in support for Tailwind so you can use any tailwind features in your html. In general when creating HTML use tailwind for design.

## Custom CSS

If we want custom css e.g. we want a button styling that we can reuse across many pages or want to change the style of the overall site we can create a file `custom.css`.

## HTML in Markdown files

When writing HTML inside markdown (.md) files:

- Use **2-space indentation** (not 4 spaces)
- **No blank lines** between sibling HTML elements — blank lines can cause markdown parsers to treat content as markdown rather than raw HTML

## Configuration

There is a config.json that configures things like the navbar, footer and much more. See https://flowershow.app/docs/config for more.
