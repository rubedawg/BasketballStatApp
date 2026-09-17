# Basketball Stat Tracker

A lightweight, phone-first stat-keeping app for youth basketball. Track shooting (2PT/3PT/FT, makes and misses), rebounds, assists, steals, blocks, turnovers and fouls per player, live during a game, then review box scores and season totals afterward.

## Stack

Plain HTML/CSS/JS. No build step, no dependencies, no backend. One file: `index.html`.

## Running locally

Just open `index.html` in a browser, or serve it with anything static:

```bash
npx serve .
```

## Deploying to Vercel

Zero config needed — this is a static site with an `index.html` at the root.

1. Push this repo to GitHub.
2. In Vercel, "Add New Project" → import the repo.
3. Framework preset: **Other**. No build command, no output directory override needed.
4. Deploy.

## Data storage — read this before relying on it

All data (roster, games, season stats) is stored in the browser's `localStorage`. **This means:**

- Data lives on one device, in one browser. It does not sync across devices or between people.
- If someone else opens this same deployed URL on their own phone, they see a **blank slate** — no roster, no game history. They are not looking at your data.
- Clearing browser data/cache on the device that has your season history will erase it. There's no cloud backup built in.
- Use each game's **Export CSV** button (or the Season tab's export) periodically if you want an external backup.

If you need multiple people to see and update the *same* live roster/season data from different devices, this app needs a real backend (a small database) added — it isn't just a hosting change. Worth doing if that's actually the use case; ask and it's a straightforward addition (e.g. Vercel + a hosted Postgres/Supabase table).

## Features

- **Roster** — add/remove players (jersey # + name). Removing a player never touches past game stats — those are snapshotted per game.
- **Live game** — start a game (opponent + date), tap a player to log makes/misses on 2PT/3PT/FT plus REB/AST/STL/BLK/TO/PF. Running scoreboard and shooting line update live.
- **Undo** — a "Recent" panel shows the last several actions with a one-tap undo on any of them, not just the most recent.
- **History** — every finished game saves a full box score. A Season tab aggregates totals/averages per player across all games, sortable by column.
- **CSV export** — per-game box score or full season totals.
- **Dark/light aware** — follows the device's system theme.
