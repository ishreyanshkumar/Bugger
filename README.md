# 🔥 Code Roast Machine

> Paste a GitHub repo URL. Watch it burn. Download the ashes.

A single-file, zero-dependency tool that performs a ruthless, AI-powered code review of any public GitHub repository — directly in your browser. No backend, no signup, no data ever leaves your machine except directly to Google's API.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Try%20It-ff4444?style=for-the-badge)](https://ishreyanshkumar.github.io/Bugger/)

---

## What It Does

1. **Maps the codebase** — Fetches the full file tree from GitHub and asks the LLM to identify the 100 most important files to review, understanding the architecture first.
2. **Roasts each file** — Sends each file to the model with a 12-point bug-hunting checklist. The model is instructed to be **merciless**: logic bugs, race conditions, missing error handling, security holes, resource leaks, bad patterns.
3. **Delivers a verdict** — Generates an overall score, severity rating, and a final architectural verdict on the entire codebase.
4. **Downloads the report** — Everything packaged as a ZIP: full JSON results + a human-readable markdown report.

---

## Features

- 🧠 **LLM-driven file selection** — Not random sampling. The model reads the directory tree, understands the architecture, and selects the files that actually matter.
- 🔍 **Deep bug analysis** — 12-point mandatory checklist: logic bugs, state machine errors, edge cases, race conditions, security, resource leaks, error handling, API misuse, data integrity, dead code, naming, architecture.
- ⚡ **Rate-limit aware** — Dynamic delays per model, TPM/RPM/RPD budgets respected. No silent quota hammering.
- 🛡️ **Robust JSON recovery** — 6-stage fallback parser handles truncated/malformed model responses including surgical mid-string repair.
- 🔒 **Zero backend** — Your API key and repo contents never touch any server other than Google's. Everything runs in a single HTML file.

---

## Usage

### Option 1: Use the live demo
Open [ishreyanshkumar.github.io/Bugger](https://ishreyanshkumar.github.io/Bugger/) in your browser.

### Option 2: Run locally
```bash
# Just open the file — no build step, no npm install
open index.html   # macOS
start index.html  # Windows
```

### Steps
1. Get a free Gemini API key from [aistudio.google.com](https://aistudio.google.com/app/apikey)
2. Paste the key into the API key field
3. Paste any public GitHub repo URL
4. Click **ROAST IT** and watch the logs

---

## Rate Limits (Gemini 3.1 Flash Lite — Free Tier)

| Metric | Value |
|--------|-------|
| RPM (requests/min) | 15 |
| TPM (tokens/min) | 250K |
| RPD (requests/day) | **500** |
| Lines analyzed/file | ~800 |
| Time for 50 files | ~4 min |
| Repos/day | ~9 |

The 500 RPD limit is the real ceiling: each file = 1 API call, plus 2 overhead calls (codebase map + final verdict). A 50-file repo = 52 calls → leaves room for ~9 full roasts per day on the free tier.

---

## Scoring

| Score | Meaning |
|-------|---------|
| 1–3 | Catastrophic — data loss, security breach, crashes in prod |
| 4–5 | Significant negligence — bugs that surface under normal use |
| 6–7 | Mediocre — works today, breaks tomorrow |
| 8–9 | Competent — minor nits |
| 10 | Exceptional — genuinely rare |

---

## Tech Stack

- Vanilla HTML + CSS + JavaScript (zero dependencies, zero build step)
- [GitHub Contents API](https://docs.github.com/en/rest/repos/contents) for fetching repo trees and file content
- [Google Gemini API](https://ai.google.dev/) (`v1beta`) for all LLM calls
- [JSZip](https://stuk.github.io/jszip/) (loaded from CDN) for report packaging

---

## Caveats

- Works on **public** GitHub repositories only
- Files larger than 200KB are capped at 200KB
- The model may still hallucinate line numbers — treat them as approximate
- Free-tier API keys have daily limits; the tool respects them but cannot exceed them

---

## License

MIT — do whatever you want with it.
