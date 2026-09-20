# Architecture

This document describes HabitRail's internal structure for contributors.

## Module layout

```
habitrail/
├── cli.py          # argparse-based command dispatch, one function per subcommand
├── db.py           # SQLite connection handling and schema migrations
├── streaks.py      # streak calculation algorithm
├── export.py       # CSV export
├── reminders.py    # OS-native notification scheduling
└── heatmap.py       # terminal calendar heatmap rendering
```

## Streak calculation

`streaks.py` computes a habit's current streak by walking backwards from
today (or yesterday, if today has no entry yet) through the `entries`
table, counting consecutive `completed = true` days until it finds a gap.
The longest streak is computed the same way but scanning the full history
rather than stopping at the first gap.

This is intentionally a simple O(n) scan rather than a windowed SQL query,
since the entries table is expected to stay small (a few thousand rows even
for years of daily tracking across many habits) — there's no indexing
concern here in practice.

## Timezone handling

All dates are computed using `datetime.now()` in the system's local
timezone — there is no UTC normalization anywhere in the codebase. This is
a deliberate simplicity trade-off, but it's also the source of the
timezone-related streak bugs mentioned in the README's known limitations
section. A more correct approach would store entries with an explicit
timezone-aware date and let the user configure their "home" timezone
separately from the system clock, but this hasn't been implemented.

## Database migrations

`db.py` checks a `schema_version` row in a dedicated `meta` table on every
startup and applies any pending migration scripts from `migrations/` in
order. There is currently no rollback mechanism — migrations are expected
to be additive only.

## Reminders

`reminders.py` shells out to the OS's native notification mechanism:
- macOS: `osascript` (AppleScript) via `subprocess`
- Linux: `notify-send` via `subprocess` (requires a running notification
  daemon, typically provided by the desktop environment)
- Windows: `win10toast` (a small optional dependency, only imported when
  reminders are actually used, to keep the base install dependency-light)

Reminders are scheduled using the OS's own scheduler (`cron` on
Linux/macOS via a generated crontab entry, Task Scheduler on Windows) —
HabitRail itself does not run as a background daemon.

## Testing

Tests live in `tests/`, using `pytest` with a temporary SQLite file per
test (via a fixture in `conftest.py`) so tests never touch a real user's
`~/.habitrail/habitrail.db`.
