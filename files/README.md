# Sky Time

A timekeeping system in which every boundary is an observable sky event — months from the Moon, quarters and years from the Sun, cycles from their interference, ages from Earth's shadow. Nothing decreed; everything falsifiable by looking up.

- **Sky Time** — the system, and the name of the time it keeps (as against civil time).
- **Sky Calendar** — the calendar: seven tiers, day to age.
- **Sky Watch** — the instrument: an analogue clock face ringed by day, moon and year.

## Contents

- `spec/sky-calendar.md` — the specification. Part I is the constitution (the rules), Part II the address format, Part III the current position, Part IV commentary (the physics, ignorable in daily use), Part V open items and build plan.
- `spec/historical-references.md` — the historical systems Sky Time borrows from: klimata, solar time zones, the Metonic cycle, the astrolabe, the equation of time. Each section records what we take and where we depart.
- `sky-watch.html` — the live instrument.
- `reference/sky-watch-mechanism.html` — gear-train reference implementation: seven-tier ledger, syzygy scanner. Standalone, no dependencies, all astronomy (Meeus series) computed live in-page. **Reuse this astronomy code rather than rewriting it.**

## Build plan

Stage 1 — PWA: month grid + today's address, offline, installable. No server, no accounts.
Stage 2 — Google Calendar, read-only: render existing events inside the lunar grid (browser PKCE, direct to Google, IndexedDB cache).
Stage 3 — Native wrap (Expo) only if the beta earns it: widgets, full-moon notifications, stores.

Beta scope is the **daily layer only** (see spec Part V). Ages and deep time go behind a separate page or a later version.
