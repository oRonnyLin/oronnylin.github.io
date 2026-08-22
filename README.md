# oronnylin.github.io

My portfolio site. Lives at https://oronnylin.github.io.

Built off a free HTML template, with Claude Code doing most of the typing.
Plain HTML/CSS/JS.

## Run it locally

```bash
python3 -m http.server 8000
```

Then http://localhost:8000.

## Publish

```bash
git add -A && git commit -m "Update site" && git push
```

Takes a minute to go live. Hard-refresh (Cmd+Shift+R) if it still looks old.

## Where things are

- `index.html` — the whole site. Unfilled spots are marked `TODO:`.
- `css/custom.css` — my overrides. `css/style.css` is the template's; don't touch it.
- `images/` — `profile.jpg`, `cover.jpg`, `project-1..8.jpg`, `writing-1..3.jpg`.
- `files/` — resume PDF and anything else worth linking.

`grep -n "TODO:" index.html` shows what's left. Editing notes are in CUSTOMIZE.md.

---

Personal site, not open to contributions.

Template by [FreeHTML5.co](http://freehtml5.co). Placeholder photos from [Unsplash](http://unsplash.com).
