# Basketball Stat Tracker

A lightweight, phone-first stat-keeping app for youth basketball. Track shooting (2PT/3PT/FT, makes and misses), rebounds, assists, steals, blocks, turnovers and fouls per player, live during a game, then review box scores and season totals afterward.

Roster, live games, and season history are shared across every device that opens this page in real time — start scoring on your phone, hand it to someone else, or have a second person track from their own phone while you watch the score update live from home.

## Stack

Plain HTML/CSS/JS, no build step. Backed by Supabase (Postgres + Realtime) for shared data. One file: `index.html`.

## Running locally

Just open `index.html` in a browser, or serve it with anything static:

```bash
npx serve .
```

## Deploying to Vercel

Zero config needed — this is a static site with an `index.html` at the root. Framework preset: **Other**.

## Data & access model — read this before relying on it

- All data (roster, live game state, season history) lives in a dedicated Supabase project (`torontolordsstats`), not in the browser. Anyone who opens the deployed URL sees the same live roster and season data.
- Stat taps update in real time across every open tab/device via Supabase Realtime — no refresh needed to see someone else's entries.
- **Access is open by design, gated only by knowing the URL.** There's no login. Anyone with the link can add players, score a live game, or edit history. This matches a low-stakes team tool, not a public product — if that ever needs tightening (a shared PIN, a login), it's a small addition on top of this schema, not a rebuild.
- The Supabase project's anon/publishable key is embedded in the page source. This is expected and safe for this kind of key — it's designed to be public, and all access is governed by the database's Row Level Security policies, not by keeping the key secret.
- Only one game can be "live" at a time (enforced at the database level), so two people can't accidentally start two simultaneous games.
- Stat increments (and undo) go through atomic database functions, so two people tapping stats at the same moment can't silently overwrite each other's updates.

## Features

- **Roster** — add/remove players (jersey # + name), shared live across devices. Removing a player never touches past game stats — those are snapshotted per game.
- **Live game** — start a game (opponent + date), tap a player to log makes/misses on 2PT/3PT/FT plus REB/AST/STL/BLK/TO/PF. Running scoreboard and shooting line update live, for everyone watching.
- **Undo** — a "Recent" panel shows the last several actions with a one-tap undo on any of them, not just the most recent.
- **History** — every finished game saves a full box score. A Season tab aggregates totals/averages per player across all games, sortable by column.
- **CSV export** — per-game box score or full season totals.
- **Dark/light aware** — follows the device's system theme.

## Backend

Supabase project: `torontolordsstats` (region: ca-central-1). Schema: `players`, `games`, `game_stats`, `game_log`, `settings`, plus two RPC functions (`increment_game_stat`, `increment_opp_score`) for atomic stat updates.
