# Slidecast Live Playground — static UI demo

A single self-contained HTML file that reproduces the bootstrapper playground's
interface as a click-through demo. No backend, no build step, no framework, no
bundler — vanilla HTML/CSS/JS in one file.

**File:** `playground-demo.html` (~123 KB)

## Embedding

```html
<iframe src="playground-demo.html"
        style="width:100%;height:760px;border:0"
        title="Slidecast Live Playground demo"></iframe>
```

It also opens directly from `file://`. Nothing is fetched at runtime, so it works
offline and inside a sandboxed iframe.

Give it at least **1100 × 700**. Below that the dual-instance layout gets cramped;
the demo still works but the two panels stop being readable side by side.

## Real vs fake

| Layer | Status |
| --- | --- |
| Layout, spacing, type scale, colours | **Real.** Ported from the app's styling layer. Fluent UI v9 `webLightTheme` token values were read out of the theme package, not eyeballed — `#242424` foreground, `#0f6cbd` brand, `#e0e0e0` stroke, and so on. |
| Step-flow / instance-accent palettes | **Real.** These are literal constants in the source, not theme tokens: done `#e8f5e9`/`#2e7d32`, active `#e3f2fd`/`#1565c0`, presenter `#e65100`, attendee `#00695c`. |
| Interaction model | **Real.** Sign-in → pick file → redeem → relay → boot, the parallel "skip sign-in" path, single/dual instances, presenter→attendee event routing, the parameter drawer, log filtering. |
| Every hostname, endpoint, product name, service name, feature-gate name, file extension, identifier and sample message | **Invented.** See below. |
| The viewer itself | **Invented.** The real app hands a container to a bootstrapper that loads a remote viewer in an iframe. A static file can't do that, so the demo renders a synthetic 12-slide deck as inline SVG with a thumbnail rail. |

### The fictional world

Nothing below refers to a real system.

| Concept | Name used here |
| --- | --- |
| Product | Slidecast Live |
| Host application | Northwind Meet (`NWMeet`) |
| Render host | `render-west.contoso-apps.example.com` |
| Edge / on-demand host | `ondemand.stage.contoso-apps.example.com` |
| Session-URL service | Relay (`relay.contoso-apps.example.com`) |
| File store | Contoso Drive / `fabrikam-my.vault.example.com` |
| Identity | `login.identity.example.com` |
| Deck file extension | `.sdx` |
| Local bootstrapper build | CoreJS |

Accounts (`dana.reeve@fabrikam.example.com`), tenant/session/call identifiers,
feature-gate names (`EnableNewRelayService`, `PlaybackAllowListForFileType`, …)
and host parameters (`presenterViewInHost`, `enableMigrateRelayToRtc`, …) are all
invented with the same *shape* as the originals.

Identifiers come from a seeded PRNG, not `crypto.randomUUID()`, so every visitor
sees the same values and the demo is reproducible.

## What to try

| Do this | To see |
| --- | --- |
| **Sign In** → **Pick File**, then double-click a file | The redeem and relay calls stream into the log, and the step bar walks pending → active → done |
| **Boot ▶** on the instance card | Staged boot with a progress bar, then the viewer, then `FnOnInitializeSuccess` |
| **←/→**, a number + **Go**, or a thumbnail | Slide navigation, each call logged, each move emitting an `OnPositionChanged` event |
| **Count** / **Pos** | Synthetic viewer events in the log |
| **Dual** in the top right, then **Boot Both** | Two instances; navigating the presenter routes `Sync: goToSlide(n)` to the attendee |
| The **Presenter** / **Attendee** badge | Live `switchRole` — the card accent, viewer badge and log badge all follow |
| **Local Bootstrapper · CDN Slideshow ⚙** | Config popover. Choosing the CDN bootstrapper disables the local slideshow option and explains why |
| **Skip sign-in**, paste any URL | The alternative path. Sign In goes muted "optional", the application URL is derived from the URL's edge prefix, and Boot unlocks without an account |
| The **▶** handle on the left edge | The ~50-field parameter drawer, with regenerable identifiers and JSON blocks |
| Set **Host Boot Timeout (ms)** to something under 1500 and Boot | The timeout path: the boot fails with `TimeoutInitialize` before the frame handshake |
| Scroll the log up while a boot streams | It stays where you put it; new lines only pin to the bottom when you're already there |

## Keyboard

| Key | Action |
| --- | --- |
| `←` `→` | Previous / next slide |
| `P` | Toggle the parameters drawer |
| `M` | Switch single / dual instance |
| `B` | Boot (both instances in dual mode) |
| `S` | Stop |
| `R` | Switch role on the active instance |
| `L` | Clear logs |
| `Esc` | Close a popover, or cancel the file picker |

Shortcuts are suppressed while a text field has focus.

## Implementation notes

One `state` object; `renderAll()` batches through `requestAnimationFrame`.
Containers are mounted once and only their slots are swapped, and nodes are
reused while their content signature is unchanged. That combination is load-
bearing rather than an optimisation:

- **Clicks during live updates.** During a boot the app re-renders about five
  times a second. Chrome cancels a click if the pressed element leaves the
  document between mousedown and mouseup — even if the *same node object* is put
  back. Rebuilding, or merely re-parenting, the Stop button swallowed the click.
- **Scroll position.** The log scroller is created once and never re-parented.
  Elements tagged `data-scroll="<key>"` have their offsets saved and restored
  around each render.
- **Focus while typing.** Focus is restored with `focus({ preventScroll: true })`;
  a plain `focus()` scrolls the field into view and undoes the scroll restore on
  every keystroke.
- **Double-click.** Detected by timestamp, not the `dblclick` event: the first
  click re-renders the file list, so the second lands on a new node and
  `dblclick` never fires.
- **Scrollbars.** `::-webkit-scrollbar` is Chromium/WebKit only, so the standard
  `scrollbar-width` / `scrollbar-color` pair is declared alongside it.

`window.__demo` exposes `state`, `DECK` and a few helpers for inspection. Nothing
in the UI depends on it.

## Known differences from the app

- The viewer is a synthetic SVG deck. Real slide rendering, animation timelines,
  ink/annotation, media playback and optical zoom have no counterpart here; the
  event names for those paths exist in the log stream but nothing drives them.
- Presenter→attendee routing is a direct function call. In the app it crosses the
  host's transport, so real ordering and latency are not represented.
- The file picker is invented. The real one is a hosted iframe with its own
  auth and navigation.
- Only one boot failure is reachable (the host-boot timeout). The app's other
  failure modes are not simulated.
