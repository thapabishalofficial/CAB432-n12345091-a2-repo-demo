# Contributing to HabitRail

Thanks for considering a contribution! HabitRail is a small project, so the
process is intentionally lightweight.

## Reporting bugs

Open an issue and include:

1. Your OS and Python version (`python --version`)
2. The exact command you ran
3. What you expected to happen
4. What actually happened (full error output if there was a crash)

## Suggesting features

Open an issue describing the use case, not just the feature. "I want to
export to JSON as well as CSV, because I'm feeding my habit data into a
personal dashboard" is more useful than "add JSON export."

## Submitting changes

1. Fork the repo and create a branch off `main`.
2. Keep changes focused — one logical change per pull request.
3. Add or update tests in `tests/` for any behavioural change.
4. Run the test suite locally before opening a PR: `pytest`.
5. Update `README.md`'s command reference table if you add or change a
   command's behaviour.

## Code style

- Follow existing formatting (we use `black` with default settings).
- Type hints are expected on new functions.
- Keep the CLI dependency-light — HabitRail intentionally avoids heavy
  frameworks. If a new feature needs a new dependency, explain why in the
  PR description.

## Labels we use

- `bug` — something doesn't work as documented
- `enhancement` — a new feature or improvement
- `question` — a usage question, not a bug or feature request
- `needs-triage` — not yet reviewed by a maintainer
- `duplicate` — already covered by another issue
- `stale` — no activity in a long time, may need to be closed or re-confirmed
- `good-first-issue` — a reasonable entry point for new contributors
- `docs` — documentation-only changes needed

## Code of conduct

Be respectful. Assume good faith. This is a small hobby project maintained
in spare time — response times may vary.
