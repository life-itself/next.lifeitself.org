# WordPress to HTML+Tailwind Markdown Conversion Plan

## Goal
Convert a WordPress page (e.g. `https://next.lifeitself.org/hubs`) into a local markdown page that uses embedded HTML + Tailwind classes, consistent with the style used in `index.md`.

## Output
- Replace the target page markdown file (example: `hubs.md`).
- Cache raw source HTML locally for working/debugging (optional, do not commit).
- Download all required image assets locally into a page-specific folder (example: `assets/images/hubs/`).
- Keep video files remote when requested.

## One-Time Repository Hygiene
- Ensure `.cache/` is gitignored.
- Keep working artifacts under `.cache/raw-html/`.

## Step-by-Step Workflow
1. Clarify scope before editing:
- Which target file to replace.
- Whether to keep dynamic WP sections (query loops/events/blog cards) as static snapshots.
- Whether any media should remain remote (e.g. hero video only).

2. Fetch and cache raw HTML:
- `mkdir -p .cache/raw-html`
- `curl -L --fail --silent --show-error '<WP_URL>' -o .cache/raw-html/<slug>.wordpress-page.html`

3. Extract content area only:
- Isolate main content block (typically `entry-content`), excluding global header/footer/nav.
- Save a focused copy: `.cache/raw-html/<slug>.entry-content.html`

4. Collect media URLs from content block:
- Extract canonical `<img src>` values and any `<video poster>` values.
- If asked to keep video remote, do not download `.mp4` files.

5. Download image assets locally:
- Create folder: `assets/images/<slug>/`
- Download all selected images with original filenames.
- Prefer assets actually used by the converted page (not all `srcset` variants).

6. Build/replace markdown page with embedded HTML:
- Use frontmatter pattern consistent with `index.md`:
  - `layout: plain`
  - `showSidebar: false`
  - `showToc: false`
- Reconstruct sections with Tailwind utility classes.
- Use local image paths: `/assets/images/<slug>/<filename>`.
- Keep allowed remote video URLs directly in `<video src="...">`.
- Preserve core copy and CTA links from the WordPress source unless instructed otherwise.
- Apply HTML formatting conventions:
  - Use **2-space indentation** for nested HTML in markdown files.
  - Use blank lines only between **root-level** HTML blocks/tags (lines that start at column 1).
  - Do not keep blank lines between indented HTML siblings/children.

7. Validate before commit:
- Confirm image references in page point to local `assets/images/<slug>/` paths.
- Confirm only explicitly allowed remote media remains remote.
- Check `git status` for unintended files.
- Confirm formatting:
  - indentation is 2 spaces;
  - no blank-line separators exist inside indented HTML regions.

## Suggested Command Snippets
```bash
# Extract media candidates from isolated content
rg -o 'poster="[^"]+"' .cache/raw-html/<slug>.entry-content.html | sed -E 's/^poster="(.*)"$/\1/'
rg -o '<img[^>]+src="[^"]+"' .cache/raw-html/<slug>.entry-content.html | sed -E 's/.*src="([^"]+)"$/\1/'

# Normalize protocol-relative URLs
sed -E 's#^//#https://#'
```

## Commit Guidance (Conventional Commits)
Use a feature commit for page conversions:
- `feat(<slug>): convert wordpress <slug> page to tailwind with local assets`

If cleanup is needed:
- Amend to remove accidental cache artifacts from tracking.
- Keep `.cache/` ignored.

## What to Avoid
- Committing `.cache` raw files.
- Downloading video files when instructed to keep them remote.
- Mixing old markdown content with new HTML structure in the same page.
- Pulling every `srcset` derivative unless needed.

## Definition of Done
- Target markdown page is replaced with clean HTML+Tailwind structure.
- All page images are local under `assets/images/<slug>/`.
- Allowed remote media (e.g. hero video) remains remote.
- `.cache/` is ignored and untracked.
- Changes are committed with a conventional commit message.
