# Auto-video-edit: HTML-native AI video editing workspace with Hyperframes and GSAP

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
![Node 22+](https://img.shields.io/badge/node-22%2B-339933?logo=nodedotjs&logoColor=white)
![GSAP](https://img.shields.io/badge/animation-GSAP-88CE02?logo=greensock&logoColor=black)
![FFmpeg](https://img.shields.io/badge/render-FFmpeg-007808?logo=ffmpeg&logoColor=white)

**Auto-video-edit is an open-source workspace for making videos as plain HTML files, driven by AI coding agents.** Each composition is an HTML page with a paused GSAP timeline registered on `window.__timelines`, and the [Hyperframes](https://hyperframes.heygen.com) CLI lints, previews and renders it to MP4 through FFmpeg. It ships Claude Code skills for Hyperframes and GSAP, a motion-design playbook and agent guides, so an AI agent can author, preview and render motion-graphics videos for you.

---

## Contents

- [Prerequisites](#prerequisites)
- [Setup](#setup)
- [Workflow](#workflow)
- [Project layout](#project-layout)
- [Render contract (must-knows)](#render-contract-must-knows)
- [FAQ](#faq)
- [License](#license)

---

## Prerequisites

- **Node 22+** (`node --version`)
- **FFmpeg** on `PATH`
- **Chrome** (auto-installed via `npx hyperframes browser ensure`)

## Setup

```bash
git clone https://github.com/maahin-1/Auto-video-edit.git
cd Auto-video-edit
npm install
npx hyperframes browser ensure
npx hyperframes doctor
```

Optional env:

```bash
cp .env.example .env   # fill in OPENAI_API_KEY / CLICKUP_API_KEY if needed
```

## Workflow

Each video lives under `video-projects/<slug>/`. Author from inside that folder.

```bash
cd video-projects/my-video
npx hyperframes preview                                           # Studio at http://localhost:3002
npx hyperframes lint                                              # validate composition
npx hyperframes render --quality draft  --output renders/draft.mp4
npx hyperframes render --quality standard --output renders/final.mp4
```

## Project layout

```
Auto-video-edit/
├── CLAUDE.md, AGENTS.md          ← workspace + agent guides
├── MOTION_PHILOSOPHY.md           ← motion-graphics aesthetic playbook
├── DESIGN.ais-example.md          ← brand-spec example
├── assets/                        ← shared brand assets
├── scripts/                       ← preflight scripts
├── .claude/skills/                ← Claude Code skills (/hyperframes, /gsap, …)
└── video-projects/
    └── my-video/
        ├── index.html             ← root composition
        ├── compositions/          ← sub-comps via data-composition-src
        ├── assets/                ← project media
        ├── meta.json              ← id / dimensions / fps
        └── hyperframes.json       ← CLI config
```

## Render contract (must-knows)

1. Root `<div>` needs `id`, `data-composition-id`, `data-start`, `data-width`, `data-height`.
2. Timed elements need `class="clip"` + `data-start`, `data-duration`, `data-track-index` — **except** `<video>`/`<audio>` skip `class="clip"`.
3. Every composition registers one paused GSAP timeline on `window.__timelines["<composition-id>"]`.
4. `<video>` must be `muted`; sibling `<audio>` feeds the mixer.
5. Never animate `width`/`height`/`top`/`left` on `<video>` directly — wrap in a `<div>` and animate the wrapper.
6. Deterministic only — no `Date.now()`, no unseeded `Math.random()`, no render-time fetches.

Full spec: `CLAUDE.md` and `npx hyperframes docs <topic>`.

---

## FAQ

### What is Auto-video-edit?
A video editing workspace where every video is an HTML composition with a GSAP timeline, rendered to MP4 by the Hyperframes CLI. It is set up for AI agents such as Claude Code to author the compositions.

### What do I need to install?
Node.js 22 or newer, FFmpeg on your PATH, and Chrome (installed automatically with `npx hyperframes browser ensure`).

### How do I render a video?
From a folder under `video-projects/`, run `npx hyperframes render --quality standard --output renders/final.mp4`. Use `--quality draft` for a quick preview render.

### Is it a Remotion project?
No. It is a separate pipeline built on Hyperframes, where compositions are HTML plus GSAP rather than React components.

### Where does it come from?
It was converted from the Hyperframes Student Kit by Nate Herk (MIT, copyright 2026 Nate Herk), whose notice is kept in the license.

## License

MIT. See [LICENSE](LICENSE).

This workspace was converted from the Hyperframes Student Kit by Nate Herk, which is MIT licensed, so his copyright notice is kept. The AI Automation Society brand materials that remain as a worked example (`assets/brand-tokens.css`, `DESIGN.ais-example.md`) are the property of AI Automation Society and are not licensed for reuse; replace them with your own brand materials.
