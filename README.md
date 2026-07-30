# jaspercurtis.com

Static portfolio site. No build step, no dependencies — plain HTML, CSS, and
self-hosted fonts.

## Pages

| File | |
|---|---|
| `index.html` | Home |
| `work.html` | Work index → Branding / Photo / Fabrication |
| `branding.html` | Branding → Underberg / Band-Aid |
| `photo.html` | Photo — Flowers, Targets, Find the Broken Bone |
| `fabrication.html` | Fabrication landing |
| `fabrication-projects.html` | Fabrication write-ups (Mel Kendrick, Nick Cave, Christopher Wool) |
| `underberg.html` | Underberg marketing project |
| `band-aid.html` | Band-Aid branding project |
| `contact.html` | Contact — click the address to copy it |

`images/` holds the photography, `fonts/` the self-hosted webfonts.

## Deploying to GitHub Pages

1. Create a repository and push these files to the root of the default branch.
2. In the repo, go to **Settings → Pages**.
3. Under **Source**, pick **Deploy from a branch**, choose your branch and the
   `/ (root)` folder, then save.

The site is live at `https://<username>.github.io/<repo>/` after a minute or so.

For a custom domain, add it under Settings → Pages → Custom domain, and point a
CNAME record at `<username>.github.io`.

`.nojekyll` is included so GitHub serves every file as-is. Without it, Jekyll
skips paths beginning with an underscore.

## Viewing locally

Serve over HTTP rather than opening the files directly:

    python3 -m http.server 8000

Then visit <http://localhost:8000>.

Opening a page as `file://` makes browsers treat the `@font-face` files as
cross-origin and block them, so the script wordmarks on Branding, Band-Aid, and
Underberg fall back to a generic face. Over HTTP they load correctly.

## Fonts

Self-hosted so the site has no external dependencies. All are licensed under the
SIL Open Font License 1.1; the license and its copyright notices are bundled in
`fonts/OFL.txt`, which the license requires be distributed with the fonts.

- Monsieur La Doulaise — Branding
- Playfair Display, DM Sans — Band-Aid
- UnifrakturMaguntia, Cormorant Garamond, Courier Prime — Underberg

Everything else uses the system Helvetica Neue stack.
