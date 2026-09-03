# Single-File Apps

Welcome to my personal development lab.

I'm a self-taught developer exploring software development by building real projects and experimenting with technologies I find interesting.

A lot of my projects are built with AI assistance—but the goal isn't to blindly generate code. I'm using AI as a development partner, tutor, debugger, and brainstorming tool while learning how everything actually works.
Four Android apps and two browser tools, each built as one self-contained HTML file.

Every app here is a single `index.html` — markup, styles, and logic in one document. The Android builds wrap that file in a WebView; the `classes.dex` in each APK is 2–7 KB, one `MainActivity`, nothing else. The same file opens directly in a browser.

No frameworks. No build step. No backend. Nothing phones home.

---

## Apps

### Bear Code — `com.bearcode.app`

A code editor that runs on a phone. CodeMirror 5.65.16 with JSHint linting, syntax modes for JavaScript, Python, HTML, CSS, SQL, Rust, Shell, Markdown, XML, and C-like languages.

Five side panels: **Files**, **Search**, **Outline**, **Problems**, and **History**. Find and replace, live preview, a run button, bracket matching and auto-close, code folding, autocomplete hints, and an active-line highlight. Theme, text size, and indent width are all configurable.

Files persist in IndexedDB with settings in localStorage, so work survives closing the app.

|                 |                                                          |
| --------------- | -------------------------------------------------------- |
| APK             | 847 KB — CodeMirror and JSHint bundled in `assets/libs/` |
| Standalone HTML | 227 KB — loads the same libraries from cdnjs             |
| Permissions     | `INTERNET`                                               |

The standalone `BearCode.html` needs a connection on first load to pull CodeMirror from CDN. The APK bundles those libraries and works fully offline. That's the only difference between the two.

### Folio — `com.folio.app`

A document writer with ten starting templates: Blank, Blog Post, Cover Letter, Essay, Journal, Letter, Meeting Notes, Report, Resume, and To-Do List. Each one drops in a real section structure — a Report opens with Summary, Introduction, Findings, Recommendations, Conclusion rather than an empty page.

Exports to `.docx`, `.html`, `.md`, and `.txt`. The DOCX export assembles the OOXML package by hand — `[Content_Types].xml`, `word/document.xml`, `word/styles.xml`, the relationship files — so it produces a genuine Word document with no library doing it.

Warm paper palette with a night theme. Documents save to localStorage; the storage permission is for writing exports to the device.

|             |                                                                               |
| ----------- | ----------------------------------------------------------------------------- |
| APK         | 50 KB                                                                         |
| Permissions | `READ_EXTERNAL_STORAGE`, `WRITE_EXTERNAL_STORAGE` (capped by `maxSdkVersion`) |

### Fantasy Fighter — `com.fantasy.fighter`

A Game Boy–styled 2D fighter. Five playable characters — Kael, Saya, Brom, Lyra, Threx — against boss Valdris. Sprites are drawn in code, piece by piece: boots, body armor, shoulder pads, visor, scarf, beard.

Combat runs a combo counter, blocking with a shield effect, knockback, gravity and ground collision, projectiles for ranged attacks, and active-frame hit windows. Brom has a berserker state that triggers on low health. The AI opponent picks actions by range — chase, mid-range, or close.

Landscape, touch controls, pixelated rendering, canvas scaled to fit the screen.

|             |       |
| ----------- | ----- |
| APK         | 34 KB |
| Permissions | none  |

No storage, no network, no permissions at all. The smallest complete thing in this repo.

---

## Browser tools

### Three Layers

An interactive HTML / CSS / JavaScript cheat sheet. The three layers are color-coded throughout — ember for structure, periwinkle for style, citrine for behavior — and every example is live and editable rather than a static code block.

Includes a specificity calculator, a box model explorer, and flexbox and grid playgrounds. Correct and incorrect patterns are marked in jade and rose so the contrast is visible at a glance.

120 KB, one file, opens in any browser.

### BearCode.html

The browser build of Bear Code. Same editor, libraries pulled from CDN instead of bundled. Use this one on a desktop; use the APK on a phone or when you need it offline.

---

## Installing

**Android:** download the APK, allow installs from unknown sources, tap the file. Minimum SDK is set per app in the manifest.

**Browser:** download the `.html` and open it. That's the whole process.

### A note on data

Bear Code stores files in IndexedDB and Folio stores documents in localStorage, both scoped to the app's WebView. Uninstalling the app or clearing its data erases them. Export anything you care about first.

---

## How these are built

Each app is developed as a single HTML file and tested in a browser. The Android build wraps it:

```
App.apk
├── AndroidManifest.xml
├── classes.dex          # one MainActivity, a WebView, a loadUrl call
├── res/mipmap-*/        # launcher icons
└── assets/
    ├── index.html       # the entire application
    └── libs/            # only where offline libraries are needed
```

The wrapper is deliberately thin. Nothing app-specific lives in the Java layer, which means the browser version and the Android version stay in sync by construction — there is only one implementation to keep correct.

---

## Conventions

- One HTML file per app. Styles in `<style>`, logic in `<script>`, same document.
- CSS custom properties for theming, declared once in `:root`.
- Canvas apps use `getContext('2d')` and draw everything themselves.
- Touch targets sized for thumbs; `user-scalable=no` and `-webkit-tap-highlight-color: transparent` on anything interactive.
- Prefer bundling a library over a CDN call when the app should work offline.

---

## AI-assisted development

These were built with a large language model — primarily Claude — with me directing the architecture, reviewing the output, and testing on real devices.

- **The constraints are my decisions.** Single file, no framework, no backend, offline-capable. Those were specified up front, not generated.
- **Generated code gets read before it ships.** Nothing goes in unreviewed.
- **AI authorship isn't an excuse for an untested app.** Each of these was installed and used before it landed here.

This section is here so anyone reading the code later knows how it was produced.

---

## Status

Personal projects, maintained as time allows. Bear Code is the most actively developed; Fantasy Fighter and Space Explorer are essentially finished.

Issues welcome — include the app name, your Android version or browser, and what you expected to happen.

---

## License

MIT unless a file states otherwise.

CodeMirror and JSHint ship inside the Bear Code APK under their own licenses (MIT and JSON-license respectively).
