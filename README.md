# ⚒️ The Forge

**A daily discipline dashboard built on one idea: consistency is a game of averages, not perfection.**

**Live app:** [micah-temple.github.io/the-forge](https://micah-temple.github.io/the-forge/)

The Forge is an offline-first progressive web app for tracking daily habits, scoring consistency over time, and journaling forward — each day you leave a note that tomorrow's page opens with. It's designed to be installed to a phone home screen and used as a daily driver: fast, private, and dependency-free.

## Features

- **Three-state habit logging** — every habit is marked Complete, Incomplete, or N/A. N/A days (rest days, sickness, travel) are excluded from the scoring denominator and don't break streaks, so the metrics stay honest instead of punishing legitimate exceptions.
- **Scoring engine** — each day gets a 0–10 score, each week a rolling average, and each month a two-decimal "locked-in score." The goal isn't a perfect day; it's a rising average.
- **Streak tracking** — per-habit live streak counts with the same N/A-aware logic.
- **12-week heatmap** — GitHub-style consistency grid so slumps and comebacks are visible as shape, not guilt.
- **Note to tomorrow** — a forward-journaling loop: whatever you write today is the first thing tomorrow's page shows you.
- **Six configurable pillars** — habits are grouped under Piety, Physique, Career, Renaissance, Relationships, and Discipline; habits can be added, renamed, and removed in-app without touching code.
- **Local-first data** — everything lives in `localStorage`. Copy/paste backups, JSON file export/import, and a built-in reminder to back up after 30 days.
- **Installable PWA** — web app manifest, service worker with a network-first/cache-fallback strategy, full offline support, iOS home-screen support with custom icons.
- **Ambient polish** — hammer-and-anvil splash animation, drifting ember particles, warm gradient theming, and haptic feedback on supported devices. All animation respects `prefers-reduced-motion`.

## Tech

- **Vanilla HTML/CSS/JS** — zero frameworks, zero dependencies, zero build step. The entire application is a single `index.html`.
- **Service worker** (`sw.js`) — network-first for freshness, cache-fallback for offline; versioned cache invalidation.
- **Web App Manifest** — standalone display mode, themed, with generated icon set.
- **GitHub Pages** — static hosting; the repo is the deployment.

## Data model

```json
{
  "habits": [ { "id": "read", "name": "Read 30 min", "pillar": "Renaissance" } ],
  "days": {
    "2026-07-13": {
      "c": { "read": 1, "lift": 2, "cardio": 3 },
      "m": "Note that tomorrow's page will display."
    }
  },
  "lastBackup": 1783900000000
}
```

Check states: `1` = complete, `2` = N/A (excluded from denominator), `3` = incomplete. A day's score is `10 × complete / (totalHabits − N/A)`. Days with no logged state are treated as unlogged rather than zero, and monthly averages report logged-day coverage alongside the score.

## Design decisions

- **No accounts, no server, no analytics.** Habit data is personal; it never leaves the device. Durability comes from explicit user-owned backups rather than a cloud dependency.
- **N/A as a first-class state.** Most trackers force a binary and users quit when travel or illness "ruins" their record. Modeling legitimate exceptions keeps long-term averages meaningful.
- **Unlogged ≠ failed.** Score math distinguishes "didn't log" from "logged a miss," which keeps historical averages from being polluted by days the app simply wasn't opened.
- **Single-file architecture.** Trivial to audit, fork, and self-host; the app has no supply chain.

## Run locally

```bash
git clone https://github.com/Micah-Temple/the-forge.git
cd the-forge
python3 -m http.server 8000   # any static server works
# open http://localhost:8000
```

To deploy your own: fork, enable GitHub Pages on `main` (root), done.

## Roadmap

- Optional push/local notification reminders
- Cross-device sync via user-supplied storage
- Monthly review report generation

---

*"Let us not become weary in doing good, for at the proper time we will reap a harvest if we do not give up." — Galatians 6:9*
