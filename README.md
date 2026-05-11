# Auto-video-edit

HTML-native video editing workspace built on [Hyperframes](https://hyperframes.heygen.com). Compositions are plain HTML files with paused GSAP timelines registered on `window.__timelines`. The Hyperframes CLI handles lint, preview, and render.

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

## License

MIT.
