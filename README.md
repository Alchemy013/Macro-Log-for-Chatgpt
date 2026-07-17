# Rehyann Macro Log

A calorie & macro tracker that lives inside ChatGPT.

Type `rajma rice, one bowl` → ChatGPT works out the macros → the panel logs it and keeps your running totals. Daily rings, weekly trends, a monthly heatmap. No API key, no account, no ads.

![Rehyann Macro Log panel](assets/screenshot.png)

## Why

I was already asking ChatGPT for macros every time I ate something. This script just closes the loop — it sends the question, reads the answer, and tracks the totals, all on the same tab. A macro tracker without the tracker app.

## Features

- **One-line logging** — food + grams. Leave grams empty and it estimates a standard serving. Tuned to assume Indian preparations by default.
- **Today** — dual-ring dashboard (calories outer, protein inner) with a live *kcal left* countdown, carbs/fat bars, the day's entry list, and quick-add chips when the day is empty.
- **Backfill** — arrow back to yesterday and log meals you forgot.
- **Week** — calorie bars against a dashed goal line, green dots on protein-hit days, weekly averages.
- **Month** — heatmap calendar: shade = calories vs goal, mint ring = protein goal hit. Tap any day to open and edit it.
- **Goals** — set daily targets for calories, protein, carbs, and fat.
- **CSV export** — your full log, one click.
- **Private by default** — everything is stored in your browser's localStorage. Nothing is sent anywhere except the food lookup you're already typing into ChatGPT.

## Install

1. Install the [Tampermonkey](https://www.tampermonkey.net/) extension for Chrome.
2. Tampermonkey icon → **Create a new script** → delete the template → paste in [`macro-log.user.js`](macro-log.user.js) → save (Ctrl+S).
3. Open [chatgpt.com](https://chatgpt.com), open any chat, and click the **Macros** pill at the bottom-right.

## Usage tips

- Keep one pinned **"Macro log"** chat — every add posts a single message in whatever conversation is open.
- Use a fast model in that chat. Thinking models turn a 3-second lookup into a 30-second one.
- Enter in the food or grams field adds the entry. The grams box is optional.

## How it works

There's no API involved. The script types a prompt into ChatGPT's own composer, presses send, waits for the stream to finish, and parses a forced single-line JSON reply:

```json
{"food":"Rajma rice","grams":300,"calories":420,"protein":14,"carbs":68,"fat":9,"fiber":8}
```

Entries live in `localStorage` under `mt_entries_v1`. The whole UI renders inside a Shadow DOM, so ChatGPT's styles can't leak into the panel and the panel's styles can't leak into ChatGPT.

## Troubleshooting

If **Add** suddenly stops working, OpenAI has probably changed their page structure. Every selector the script depends on lives in the `CONFIG` block at the top of `macro-log.user.js` — update the selectors there and reload.

## Accuracy

Macros are language-model estimates. They're consistent enough to track trends and hit daily targets, but they're not lab values — treat them accordingly.

## Roadmap

- Edit entries in place
- Weight log with trend vs intake
- Optional standalone mode via API (no ChatGPT tab needed)

## License

MIT

*Not affiliated with OpenAI.*
