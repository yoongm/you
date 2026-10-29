---
name: claude-skill-picks
description: Collect Claude Code skill recommendations from Instagram reels, YouTube videos or posts into one tabbed HTML page (one tab per source, one card per skill with rank, summary, repo and install command). Use when the user shares a reel/video/screenshot that ranks or recommends Claude Code skills and says "수집해", "이것도 추가", "스킬 모음에 넣어", or asks to see the collected skill picks.
---

# Claude skill picks

A running collection of Claude Code skill recommendations. Each source (a reel,
video or post) becomes one tab in `assets/picks.html`; each skill it mentions
becomes a card in that tab.

## Data lives in one place

All content is the JSON inside `<script id="picks-data" type="application/json">`
in `assets/picks.html`. The page renders tabs and cards from it; never hand-edit
the rendered markup.

```json
{
  "sources": [
    {
      "id": "reel-DeqOcQ8ROH5",
      "label": "릴스 1 · TOP 8",
      "url": "https://www.instagram.com/reels/DeqOcQ8ROH5/",
      "note": "one line on what the source is",
      "status": "done",
      "items": [
        {
          "rank": 1,
          "name": "Display name as shown in the source",
          "repo": "owner/repo or empty if unconfirmed",
          "summary": "One or two sentences in your own words",
          "install": "exact command from the repo README, or empty",
          "confidence": "confirmed | likely | unknown"
        }
      ]
    }
  ]
}
```

`status` is `done` or `pending` (a source the user mentioned but whose content
has not arrived yet; render it as a tab with a "waiting for screenshot" notice
and an empty `items` list).

## Adding a source

1. Get the content. Instagram and YouTube usually cannot be read from a session
   (login walls, network policy). Do not spoof a browser or work around bot
   checks. Ask the user for a screenshot and the caption text instead.
2. Identify every skill the source names. Korean transliterations are common
   (e.g. 슈퍼파워스 = Superpowers, 지스택 = gstack, 맷 포콕 = Matt Pocock).
   Find the canonical GitHub repo and take the install command from its README.
   If you cannot confirm a repo, leave `repo` and `install` empty and set
   `confidence` to `unknown` rather than guessing.
3. Write `summary` in your own words. Do not copy the source's narration or
   on-screen text beyond the skill names and ranks.
4. Append the source to `sources` (or flip an existing `pending` one to `done`),
   keep the ranking order the source used, and keep JSON valid.
5. Open the page in a browser (or Playwright screenshot) and check the new tab
   renders, then tell the user what was added and which items are unconfirmed.

## Showing the collection

Point the user at `assets/picks.html` (open it locally, or publish it as an
artifact if they want a link). The tabs are keyboard accessible: arrow keys move
between tabs, Home/End jump to the first/last.
