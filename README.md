# oronnylin.github.io

Personal portfolio site, served by GitHub Pages at **https://oronnylin.github.io**.

Static HTML/CSS/JS — no build step, no dependencies to install. Edit a file,
commit, push, and the live site updates in about a minute.

## Preview locally

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000. (Opening `index.html` straight from Finder
mostly works too, but a local server matches how Pages actually serves it.)

## Publish

```bash
git add -A && git commit -m "Update site" && git push
```

One-time setup, if it isn't already on: repo **Settings → Pages → Build and
deployment → Source: Deploy from a branch → Branch: `main` / `(root)`**.

## Layout

| Path | What's in it |
| --- | --- |
| `index.html` | The entire site — every section lives here, each one marked with a `TODO:` comment |
| `404.html` | Shown for unknown URLs |
| `css/custom.css` | **Your** style overrides — edit this one |
| `css/style.css` | The template's stylesheet — leave it alone |
| `css/`, `js/`, `fonts/`, `sass/` | Template assets (Bootstrap, jQuery, Icomoon icons) |
| `images/` | `profile.jpg`, `cover.jpg`, `project-1..8.jpg`, `writing-1..3.jpg` — replace with your own |
| `files/` | Downloads, e.g. `resume.pdf` |
| `.nojekyll` | Tells Pages to serve files as-is instead of running Jekyll |

## Filling in the content

Every placeholder in `index.html` is prefixed with `TODO:`. To see what's left:

```bash
grep -n "TODO:" index.html
```

Step-by-step instructions are in [CUSTOMIZE.md](CUSTOMIZE.md).

## Credits

Built on the free "Profile" template by [FreeHTML5.co](http://freehtml5.co).
Placeholder photos from [Unsplash](http://unsplash.com) — swap them for your own.
