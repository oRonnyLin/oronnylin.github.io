# How to make this site yours

Everything lives in `index.html`. Open it, search for `TODO:`, and work top to
bottom. Nothing here needs a build tool — save the file and refresh the browser.

```bash
grep -n "TODO:" index.html   # a running checklist of what's left
```

---

## 1. The basics

| What | Where |
| --- | --- |
| Browser tab title, search-result blurb, link-preview card | `<head>`, near the top |
| Your name and role | Hero section, `<h1>` and `<h3>` |
| Social links | Two `<ul class="fh5co-social-icons">` blocks (hero + About) |
| Contact details | About section `<ul class="info">` and the Contact section |

Delete any social icon `<li>` you don't use rather than leaving it pointing at
`TODO`. Icon names come from `css/icomoon.css` — open
`fonts/icomoon/icomoon/demo.html` in a browser to see every icon available.

## 2. Images

Drop replacements into `images/` using the same filenames and nothing else has
to change:

| File | Used for | Suggested size |
| --- | --- | --- |
| `profile.jpg` | Round photo in the hero | square, 600×600 |
| `cover.jpg` | Hero background + contact section | wide, 1920×1080 |
| `project-1.jpg` … `project-8.jpg` | Project tiles | 1200×900 |
| `writing-1..3.jpg` | Writing/talks cards | 1200×900 |

Prefer different names? Change the `url(images/…)` path in the matching tag.
Keep files under ~500 KB each so the page stays fast — JPEG at ~80% quality.

## 3. Adding a project

In the **Projects** section, copy the block marked `PROJECT TEMPLATE` and change
three things:

```html
<div class="col-md-3 text-center col-padding animate-box">
	<a href="https://github.com/you/your-repo" target="_blank" rel="noopener"
	   class="work" style="background-image: url(images/project-9.jpg);">
		<div class="desc">
			<h3>Project Name</h3>
			<span>Category</span>
		</div>
	</a>
</div>
```

1. `href` — the repo, live demo, case study, or paper
2. `background-image` — a screenshot in `images/`
3. `<h3>` / `<span>` — the name and a one-word category

Tiles are four per row (`col-md-3`). For three per row, use `col-md-4`; for two,
`col-md-6`. Have fewer than four projects? Use wider tiles so the row fills out.

## 4. Adding a job or degree

In **Experience & Education**, copy the block marked `JOB TEMPLATE`. Alternate
`timeline-unverted` and `timeline-inverted` on successive entries so they
zig-zag down the page. Education entries are identical except the badge icon is
`icon-graduation-cap` instead of `icon-suitcase`.

## 5. Skills

Two blocks, and each number appears twice — update both or the display lies:

- **Circles:** `data-percent="90"` *and* the `90%` text inside the `<span>`.
- **Bars:** `aria-valuenow="90"`, `style="width:90%"`, *and* the `90%` text.

Percentages on a portfolio are a taste call — if you'd rather list skills
plainly, replace the whole `#fh5co-skills` section with a simple list.

## 6. Resume PDF

Drop your PDF at `files/resume.pdf`. The About section already links to it. If
you don't have one yet, delete that `<li>` so the link doesn't 404.

## 7. Contact form (optional)

GitHub Pages serves static files only, so it can't process a form by itself. The
Contact section ships as plain links, which always work.

For a real form, the commented-out block in the Contact section is ready for
[Formspree](https://formspree.io): create a free form, paste its ID over
`YOUR_FORM_ID`, and remove the `<!--` / `-->` markers around the block.

## 8. Sections you don't need

Delete the whole `<div id="...">` for any section that doesn't apply:

`fh5co-about` · `fh5co-resume` · `fh5co-features` (What I Do) · `fh5co-skills` ·
`fh5co-work` (Projects) · `fh5co-blog` (Writing) · `fh5co-started` (CTA) ·
`fh5co-consult` (Contact)

The **Writing & Talks** section is the one most people cut first.

## 9. Colors and styling

The template's orange accent is `#f96d00` (with `rgba(255, 144, 0, 0.9)` on the
hero overlay). To change it, add overrides to `css/custom.css` — that file loads
last and wins. Don't edit `css/style.css`; keeping it untouched means you can
always diff against the original template.

## 10. Ship it

```bash
git add -A && git commit -m "Update site content" && git push
```

Live in about a minute at https://oronnylin.github.io. If something looks stale,
hard-refresh (Cmd+Shift+R) — the browser caches CSS aggressively.
