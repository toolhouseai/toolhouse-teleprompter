# Smart Teleprompter

A browser-based teleprompter that listens to your voice, follows along in your script, and auto-scrolls to keep up with you. No more fighting a fixed-speed scroll — the teleprompter matches *your* pace.

## How It Works

1. Paste your script and click **Start**.
2. The browser requests microphone access and begins transcribing your speech via the [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API).
3. A matching algorithm locates your spoken words in the script and advances a pointer forward.
4. Words you've spoken are dimmed, the current word is highlighted, and the display auto-scrolls to keep your position in view.

## Features

- **Voice-driven scrolling** — follows your natural reading pace
- **Fuzzy matching** — tolerates minor transcription errors (Levenshtein distance ≤ 1)
- **Filler word filtering** — ignores "um", "uh", "like", etc.
- **Look-ahead window** — recovers if you skip a sentence (30-word window)
- **Font size control** — adjustable from 20px to 80px
- **Mirror mode** — flip text horizontally for traditional teleprompter setups
- **Keyboard shortcuts** — Space to pause/resume, Escape to stop
- **Script persistence** — saves your last script to localStorage
- **Responsive** — works on desktop and tablet screens

## Requirements

- **Google Chrome** (or Chromium-based browser) — the Web Speech API is Chrome-specific for reliable use
- **Internet connection** — Chrome sends audio to Google's servers for transcription
- **Microphone access** — the browser will prompt for permission

## Getting Started

### Local Development

```bash
npm install
npm run dev
```

This starts a local Cloudflare Worker dev server via [Wrangler](https://developers.cloudflare.com/workers/wrangler/). Open the URL it prints (usually `http://localhost:8787`) in Chrome.

### Deploy to Cloudflare Workers

```bash
npm run deploy
```

## Architecture

The entire app is a single HTML file (`src/teleprompter.html`) served by a minimal Cloudflare Worker (`src/worker.js`).

```
Browser
├── Microphone → Web Speech API → Transcription
│                                      ↓
│                          Matching Algorithm (greedy forward pointer)
│                                      ↓
│                          Highlight + Auto-Scroll UI
│
└── Served by Cloudflare Worker
```

All processing happens client-side. The Worker only serves the static HTML page — no audio or script data leaves the browser.

## Project Structure

```
├── package.json          # Project config and scripts
├── wrangler.toml         # Cloudflare Worker configuration
└── src/
    ├── worker.js         # Cloudflare Worker entry point
    └── teleprompter.html # Complete single-page app
```

## License

MIT
