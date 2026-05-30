# 🐾 Pomodoro Meadow

> A cozy Pomodoro timer with a walking cat, a living meadow, and ambient music — built as a single self-contained HTML file.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![No build step](https://img.shields.io/badge/build-none-brightgreen)
![Single file](https://img.shields.io/badge/file-pomodoro--cat.html-blue)

[**▶ Live demo**](https://AmySalami.github.io/cat-walking-me-working/pomodoro-cat.html)

---

## ✨ Why

I wanted a Pomodoro timer that doesn't beep at me like a kitchen appliance.
Something calm — a meadow you can glance at, a cat that walks while you focus, music that pauses when you rest. The whole thing lives in **one HTML file** (no build, no deps), so it's easy to fork, tweak, or drop on any server.

## 🌿 Features

- **Focus ↔ Short break** auto-cycle with audible 3-2-1 cue before each focus ends
- **Walking cat + scrolling meadow + drifting trees** during focus; everything pauses on break
- **4 session presets** — classic 25/5 · quick 15/3 · deep 50/10 · marathon 90/15
- **SoundCloud playlist** with proactive shuffle (next track 3% before the current one ends)
- **Time-of-day moods** — morning, afternoon, evening (sunset behind horizon), night (moon + crater)
- **Document Picture-in-Picture** — pops the whole widget out, floats over any window
- **Collapse mode** — hide the menu, scene fills the screen (foreground stays pixel-pinned)
- **WCAG AA contrast**, ARIA labels, focus-visible tooltips on icon buttons
- **Tab-hidden CPU pause** — animations freeze when the browser tab isn't visible
- **Zero dependencies** beyond Google Fonts + SoundCloud's widget API

## 🛠 Built with

| Layer | What |
|---|---|
| Markup / Styling | HTML5, CSS Grid + Flexbox, `:root` design tokens, `@keyframes` |
| Logic | Vanilla JavaScript (no framework, no build) |
| Audio | Web Audio API (bell chimes), SoundCloud Widget API |
| Browser API | Document Picture-in-Picture, Page Visibility |
| Art | Inline SVG (cat, butterflies, bees) + procedural grass/trees |

## 🏃 Run locally

It's one file. Two options:

**Easiest** — double-click `pomodoro-cat.html`
SoundCloud iframe needs `http://` (not `file://`) to embed properly, so:

```bash
# Python (any version)
python3 -m http.server 8765
# then open http://localhost:8765/pomodoro-cat.html
```

```bash
# Or with Node
npx serve .
```

## 📐 Architecture notes

- **Single source of truth** — one DOM, two layouts (fullscreen vs PiP) handled via `body.*-mode` classes; no element duplication
- **Viewport-anchored positioning** — horizon, trees, cat use `vh` from scene top instead of `%` of scene height, so they don't drift when scene grows/shrinks (collapse/expand)
- **State machine** for the timer encoded as classes on `.widget` (`running`, `paused`, `break`, `counting`) — CSS reacts to each
- **Time-of-day** picks one of four `body.time-*` classes; everything else is CSS

## 🎨 Acknowledgments

- Cat & meadow art — original SVGs in this repo
- Default playlist — [ConcernedApe on SoundCloud](https://soundcloud.com/concernedape) (Stardew Valley OST)
- Fonts — [Fraunces](https://fonts.google.com/specimen/Fraunces) and [DM Mono](https://fonts.google.com/specimen/DM+Mono) via Google Fonts

## 📜 License

[MIT](LICENSE) — use, fork, remix freely. Credit appreciated but not required.
