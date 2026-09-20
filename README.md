# HabitRail

A small, offline-first command-line habit tracker written in Python. Track
daily habits, see streaks, and export your history — no accounts, no cloud
sync, just a local SQLite file.

## Features

- **Track any habit** — `habitrail add "Drink water"` creates a new habit.
- **Daily check-ins** — `habitrail done "Drink water"` marks today complete.
- **Streaks** — HabitRail computes your current and longest streak per habit,
  and shows a simple calendar-style heatmap in the terminal (`habitrail show
  "Drink water"`).
- **Multiple habits, one command** — `habitrail list` shows every habit with
  today's status and current streak at a glance.
- **CSV export** — `habitrail export habits.csv` dumps your full history for
  use in spreadsheets or other tools.
- **Reminders (local only)** — `habitrail remind "Drink water" --at 09:00`
  schedules a local desktop notification using the OS's own notification
  system. This does **not** send anything over the network.
- **No accounts, no telemetry** — everything lives in a single SQLite file at
  `~/.habitrail/habitrail.db`. HabitRail never phones home.

## Installation

```bash
pip install habitrail
```

Requires Python 3.10+. Works on macOS, Linux, and Windows (reminders use the
native notification system on each platform, so behaviour can vary slightly
between OSes — see [Known limitations](#known-limitations)).

## Quick start

```bash
habitrail add "Drink water"
habitrail add "Read 10 pages"
habitrail done "Drink water"
habitrail list
```

```
Habit              Today   Streak   Longest
Drink water          ✅       4        11
Read 10 pages        ⬜       0         6
```

## Data model

HabitRail stores everything in a single SQLite database:

- `habits` table: `id`, `name`, `created_at`, `archived` (boolean)
- `entries` table: `id`, `habit_id`, `date`, `completed` (boolean)

A streak is computed as the number of consecutive calendar days up to and
including today (or yesterday, if today isn't marked yet) where `completed`
is true. Archiving a habit (`habitrail archive "Drink water"`) hides it from
`list` and `show` but keeps its history intact for `export`.

## Command reference

| Command | Description |
|---|---|
| `habitrail add <name>` | Create a new habit |
| `habitrail done <name> [--date YYYY-MM-DD]` | Mark a habit complete (defaults to today) |
| `habitrail undo <name> [--date YYYY-MM-DD]` | Unmark a habit for a given day |
| `habitrail list` | Show all active habits with today's status and streaks |
| `habitrail show <name>` | Show a calendar heatmap and stats for one habit |
| `habitrail archive <name>` | Hide a habit from `list`/`show` without deleting history |
| `habitrail remind <name> --at HH:MM` | Schedule a local daily reminder |
| `habitrail export <path.csv>` | Export full history to CSV |

## Known limitations

- Reminders on Linux require a running notification daemon (most desktop
  environments have one by default; minimal/headless setups won't show
  notifications).
- Timezone handling: HabitRail uses the system's local timezone for all
  date calculations. Traveling across timezones mid-streak can occasionally
  cause a day to be skipped or double-counted — see open issues for the
  current status of this.
- There's currently no way to rename a habit without losing its streak
  history (a rename is implemented internally as archive + re-add).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## Architecture

See [ARCHITECTURE.md](ARCHITECTURE.md) for an overview of the internal
module layout and the streak-calculation algorithm.

## License

MIT

