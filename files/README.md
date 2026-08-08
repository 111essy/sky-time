# Three-Body Calendar

A lunisolar calendar in which every boundary is an observable sky event — months from the Moon, quarters and years from the Sun, cycles from their interference, ages from Earth's shadow. Nothing decreed; everything falsifiable by looking up.

**Goal:** a mobile PWA showing today's date in this system alongside the Gregorian one, with a month grid built from real lunations.

## Contents

- `spec/three-body-calendar.md` — the full specification. Part I is the constitution (the rules), Part II the address format, Part III the current position, Part IV commentary (the physics, ignorable in daily use), Part V open items and build plan.
- `reference/calendar-mechanism.html` — working reference implementation: gear train, seven-tier ledger, syzygy scanner. Standalone, no dependencies, all astronomy (Meeus series) computed live in-page. **Reuse this astronomy code rather than rewriting it.**

## Build plan

Stage 1 — PWA: month grid + today's address, offline, installable. No server, no accounts.
Stage 2 — Google Calendar, read-only: render existing events inside the lunar grid (browser PKCE, direct to Google, IndexedDB cache).
Stage 3 — Native wrap (Expo) only if the beta earns it: widgets, full-moon notifications, stores.

Beta scope is the **daily layer only** (see spec Part V, item 5). Ages and deep time go behind a separate page or a later version.
