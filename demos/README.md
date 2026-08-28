# Inspector — static UI demo

`inspector-demo.html` is a self-contained, client-only reproduction of the Inspector desktop app's interface, meant for a portfolio or a design walkthrough. One file, no framework, no build step, no backend.

## Embedding it

Copy `inspector-demo.html` next to your site's other assets and point an `<iframe>` at it:

```html
<iframe
  src="/demos/inspector-demo.html"
  title="Inspector — interactive UI demo"
  style="width: 100%; height: 760px; border: 1px solid #e0e0e0; border-radius: 8px;"
  loading="lazy"></iframe>
```

The iframe is the recommended container: the demo sets `html, body { height: 100% }` and owns its own scrolling, so isolating it keeps the host page's CSS and the demo's CSS from colliding. Give it at least ~700px of height — the app is a four-region desktop layout and gets cramped below that.

It works from `file://` too, so you can double-click the file to try it locally.

## What is real and what is not

Everything the user *touches* is real: the interactions, layout, state transitions, and visual design are ported directly from the app's renderer. Everything the app would *fetch* is fake.

- **No proxy, no capture, no network.** "Start" runs a generator that emits synthetic rows on a timer.
- **All identifiers are invented.** Hosts (`liveapp.contoso.com`, `res-*.cdn.contoso.net`, `*-media.contoso.io`, `*.contoso-drive.com`), the six network classes (`Thumbnail`, `Session`, `Items`, `Asset`, `Media`, `Embed`), ULS tags, categories, sources and log messages are all fictional. No internal endpoint or telemetry appears anywhere in this file.
- **Export JSON** downloads the synthetic rows currently in the tab.
- **Zoom** emulates the Electron window zoom with CSS `zoom`.

## What you can try

| Area | Interactions |
|---|---|
| **Capture** | Start / Stop / Continue (a second run inserts a `Run N · continued` divider), Clear |
| **Capture config** (gear) | Toggle ULS logs; toggle or remove decrypt hosts; add a domain (with or without a Record entry); toggle the six built-in classes; add a custom Record rule (substring or regex); build and arm a chaos fault; Test a URL |
| **Chaos** | Arm the ⚡ switch, attach a fault to a Category, narrow by URL / method / probability / delay, choose a request leg (block or forward, with url / header / body edits) and a response leg (drop or respond, with status / header / body edits). Faulted rows get a ⚡ banner and keep the real upstream response for comparison. |
| **Table** | Click a row to inspect it; sort by clicking a header (disabled while capturing); drag a column edge to resize; show/hide columns; scroll sideways with the dock scrollbar or Shift+wheel; scroll up to pause the live tail and click **↓ Live** to resume |
| **Filtering** | `Logs / Both / Network`; the search box toggles between **Filter** (hides non-matches) and **Find** (highlights, with `n / m` stepping); the eye-off **Mute** popover hides noisy rows by tag or message regex |
| **Markers** | `M` drops a marker, `[` / `]` jump between them, **Span** shows only the rows between the last two, right-click a marker to remove it |
| **Details** | Drag the left edge to resize; **Ctrl+F** searches within the row; click any value to copy it; header and body sections collapse |
| **Tabs** | `+` for a new capture tab, click twice to rename, right-click for Close / Close others / Close tabs to the right, `Ctrl+Tab` and `Ctrl+1–9` to switch |
| **MCP** | Toggle the server on to reveal the connect commands (copy buttons work) |

## Keyboard

| Key | Action |
|---|---|
| `M` | Drop a marker |
| `[` / `]` | Previous / next marker |
| `Ctrl+F` | Find within the open Details panel |
| `Enter` / `Shift+Enter` | Find mode: next / previous match |
| `Ctrl+Tab` / `Ctrl+Shift+Tab` | Next / previous tab |
| `Ctrl+1–9` | Jump to a tab |
| `F2` / `Delete` | Rename / close the focused tab |
| `Ctrl +` / `Ctrl -` / `Ctrl 0` | Zoom in / out / reset (also `Ctrl` + wheel) |
| `Esc` | Close a popover, or the Details find bar |

## Editing it

The file is organised top to bottom as: Fluent `webLightTheme` tokens as CSS variables → component CSS → DOM helpers and icons → formatters and pill palettes → the fictional data model → columns → state → the capture engine → the render loop → widgets → top bar → capture config → table → details panel → bottom dock → keyboard → init. Each region re-renders from state; the table scroller is the one node kept alive across renders so its scroll offset survives.

To change what the demo streams, edit `LOG_RECIPES` and `NET_RECIPES`. To change the fictional service, edit `BUILTIN_HOSTS`, `BUILTIN_CLASSES` and `BUILTIN_CLASS_INFO`.
