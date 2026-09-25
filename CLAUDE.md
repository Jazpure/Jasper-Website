# CLAUDE.md

Notes for Claude Code working in this repo. Read this before editing.

## What this is

Jasper Curtis's portfolio site. Static HTML/CSS, **no build step, no dependencies,
no framework**. Live at **jasper-curtis.com** via Cloudflare Pages, which deploys
automatically on every push to `main` of `github.com/Jazpure/Jasper-Website`.

Designs come from a Figma file:
`https://www.figma.com/design/6blhvA9UpOWynOOG4La1jq/Untitled`

Pull real design context with the Figma MCP tools rather than working from
screenshots — the frames carry exact positions, colours and crops.

## Structure

Nine standalone pages, each with its own inline `<style>`. There is no shared
stylesheet: every page is self-contained.

| File | |
|---|---|
| `index.html` | Home |
| `work.html` | Work index → Branding and Design / Photo / Fabrication |
| `branding.html` | Branding and Design → Underberg, plus My Map and Glyphics (their own sites) |
| `photo.html` | Photo — Flowers, Target Series, portraits, Find the Broken Bone |
| `fabrication.html` | Fabrication cover (names over a photo) |
| `fabrication-projects.html` | Fabrication write-ups |
| `underberg.html` | Project page |
| `band-aid.html` | Project page, no longer linked from the site |
| `contact.html` | Contact, click-to-copy email |

`images/` holds photography, `fonts/` self-hosted webfonts.

**Duplication is deliberate but real.** The cow-logo block is ~38 lines repeated
across 8 pages. A change to it means editing all 8. This was a considered choice —
self-contained pages can't break globally — but if you're making the same edit
everywhere, say so rather than silently doing it eight times.

## Conventions

- Vanilla HTML + CSS in an inline `<style>`. Match the surrounding code.
- `* { margin: 0; padding: 0; box-sizing: border-box; }` at the top of every page.
- Font stack: `"Helvetica Neue", Helvetica, Arial, sans-serif`.
- Spacing lives in `:root` custom properties per page (`--item-gap`,
  `--section-gap`, etc.). **Use the existing scale — don't introduce new values.**
- Widths are expressed as a share of the Figma frame, with the source number in a
  comment (e.g. `.w-marshall { width: 75.6%; } /* 1025 */`).
- Every deliberate departure from Figma carries a comment explaining why.

## The cow logo

Present on all pages except `index.html` (it links home, so home doesn't need it).
It adapts per pixel to whatever is behind it — black over the white page, white
over dark imagery, recomputed live while scrolling.

It uses **white** artwork (`cow-logo-white.png`) plus:

1. `mix-blend-mode: difference` on `.home-cow` — the baseline.
2. An `@supports` block that thresholds the backdrop via `backdrop-filter` and a
   mask, giving true black/white instead of a complementary tint over saturated
   colour.

Two things break this if you touch it:

- The blend must sit on the **anchor**, not the `img`. `.home-cow` has `z-index`,
  which makes it a stacking context; a blend on the child isolates and does nothing.
- `.home-cow` must be positioned for the `::after` to anchor. On `underberg.html`
  it needed an explicit `position: relative` because it sits inside the nav.

The `url(x.png)` inside the `@supports` condition is a feature test. It is never
fetched. Don't "fix" it.

## Gotchas

- **Never open pages as `file://`.** Browsers treat the `@font-face` files as
  cross-origin and block them, so the script wordmarks silently fall back to a
  generic face. Serve over HTTP: `python3 -m http.server 8000`.
- **Cloudflare caches hard.** After deploying, check with `?v=2` on the URL or
  purge the cache. A change that looks like it didn't deploy is usually cache.
- **`index.html`'s mobile rules are `(orientation: portrait) and (max-width: 900px)`.**
  A narrow desktop window will not trigger them. The home photo is deliberately
  rotated 90° on mobile — that is the design, not a bug.
- **macOS is case-insensitive; Cloudflare and GitHub Pages are not.** A reference
  to `Images/Foo.jpg` works locally and 404s live. `os.path.exists` cannot catch
  this — compare filenames byte-exactly.
- Don't add a `<link>` to Google Fonts. Fonts are self-hosted so the site has no
  external dependencies; `fonts/OFL.txt` must ship with them (licence requirement).
- `CNAME` holds the custom domain. Don't delete it.

## Deliberate departures from Figma

- **`photo.html` blurbs** are 14px/weight 300; Figma sets 12px Thin, which was
  illegible. Dimension labels likewise 11px/300 rather than 10px/200.
- **`photo.html` dimension labels** ("17x25 in") float loose in the frame, landing
  on top of images. They're rendered as caption rows: name left, dimension right,
  above each image.
- **`photo.html` nav order** follows page order; the frame lists it differently,
  which made the links jump around.
- **Spacing** is standardised where the frame drifts. Photo page: 20 / 100 / 40 /
  200. Fabrication projects: 80 / 100 / 140.
- **Two pages share a heading** — "Fabrication work" (cover) and "Fabrication Work"
  (projects). Left as designed; the `<title>` tags disambiguate instead.

## Working on it

Serve locally and verify visually before saying something works:

    python3 -m http.server 8000

Check both a desktop width and a portrait phone viewport. Measure geometry from
the DOM rather than trusting a screenshot — the preview pane sometimes renders
scaled or in the wrong orientation.

Deploy:

    git add -A && git commit -m "What changed" && git push

Cloudflare rebuilds itself. There is no separate deploy step.
