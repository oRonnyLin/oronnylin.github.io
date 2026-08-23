# Editing notes

Content lives in `index.html`. Styles I added live in `css/custom.css`.
`css/style.css` is the template's — leave it alone so it stays diffable.

## Adding a project

Copy a `.work--flat` tile in the Selected Work section:

```html
<div class="col-md-3 text-center col-padding animate-box">
	<div class="work work--flat">
		<div class="desc">
			<h3>Project Name</h3>
			<span>Category &middot; Context</span>
		</div>
	</div>
</div>
```

If it links somewhere, swap the outer `<div class="work work--flat">` for
`<a href="..." target="_blank" rel="noopener" class="work work--flat work--link">`.

Tiles are four per row (`col-md-3`); use `col-md-4` for three, `col-md-6` for two.

## Using a screenshot instead of a flat tile

Drop the image in `images/`, then remove `work--flat` and add the background:

```html
<a href="..." class="work" style="background-image: url(images/my-project.jpg);">
```

Note the template hides the caption until hover on desktop for photo tiles —
that's why the no-image tiles use `work--flat`, which keeps the text visible.

## Adding a job or degree

Copy a `<li>` in the timeline. Alternate `timeline-unverted` and
`timeline-inverted` so entries zig-zag. Education entries use the
`icon-graduation-cap` badge instead of `icon-suitcase`.

## Skills

Three `.skill-group` blocks, each a plain `<ul class="skill-tags">`. Add an
`<li>`. These replaced the template's percentage charts, which needed made-up
numbers.

## Resume

`files/resume.pdf` is a copy of `Resume_Web_Public.pdf`. Re-copy it when the
source changes — it does not update itself.
