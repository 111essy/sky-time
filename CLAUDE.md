# Sky Time

## Naming
- **Sky Time** — the system, and the time it keeps (as against civil time).
- **Sky Calendar** — the calendar: seven tiers, day to age.
- **Sky Watch** — the instrument, `three-body-clock.html` (to be renamed
  `sky-watch.html`).
Never write "three-body" in new code, comments or docs. The old name
survives only in filenames until they are renamed.

## Files
- `three-body-clock.html` — the live instrument. Single self-contained HTML
  file, no dependencies, no build step. This is a hard constraint: do not
  add package.json, a bundler, a src/ tree, or any scaffolding.
- `files/sky-calendar.md` — the specification. Part I is the constitution.
- `files/historical-references.md` — historical systems and where we depart.
- `files/sky-watch-mechanism.html` — gear-train reference implementation.

## Constitutional rules that govern the code
1. **Nothing steps.** Every quantity moves continuously, because the sky
   does. No recompute-on-date-rollover, no reset-on-reload, no discrete
   advancement. If a value could jump, that is a bug.
2. **Sky-falsifiable.** Every mark must correspond to something an observer
   could verify by looking up. Do not fabricate marks or smooth away real
   irregularity.
3. **Astronomy comes from the series.** Use the Meeus series already in the
   file. Never replace them with mean-value arithmetic or interpolation
   between computed events. Real lunations vary 29.27–29.83 days; a mean
   month is wrong by hours.
4. **Local vs universal.** Tiers at or inside the day ring (r ≤ 200) are
   local to the observer. Everything outside it is universal — the same for
   every observer on Earth at the same instant.
5. **One source of truth.** A quantity gets one definition. Do not leave two
   functions computing the same thing differently.

## How to work with me
- One change per session where possible. Show the diff before I accept it.
- Every change that touches astronomy or geometry must end with console
  output I can check against numbers. State the expected values.
- If you cannot run something, say so plainly. Never report an unrun check
  as passing.
- If an instruction of mine conflicts with something in the file, or looks
  wrong, say so instead of complying. I have supplied wrong fixtures before
  and want them caught.
- Verification code must be able to fail. If a test cannot distinguish
  "works" from "never exercised the path", say INCONCLUSIVE rather than
  passing.
- Do not restructure, rename, or "clean up" anything I did not ask about.

## Verification fixtures (must keep passing)
At exact Rotterdam coordinates, 51.9225 N / 4.47917 E:
- sunrise/sunset: 2026-08-09 → 06:17 / 21:18 (UTC+2); 2025-12-21 → 08:48 /
  16:33 (UTC+1); 2026-06-21 → 05:22 / 22:05 (UTC+2)
- elongation fraction at the four lunar quarters → 0.0000 / 0.2500 /
  0.5000 / 0.7500
- solar quarters 2026 → 87.7° / 91.4° / 92.3° / 88.6°, summing to 360
- apparent solar longitude at JD 2461262.00000 → 136.9303°, ring fraction
  0.63036
- noon/midnight marks exactly 180° apart at every instant
- midnight continuity: A (immediate rollover) < 0.01°, B (60s-suppressed
  control) ≈ 0.45°. If A and B match, print INCONCLUSIVE.

## Queued work
1. Add the equation of time to the amber sky-time hands — they currently
   run zone mean time while the day ring runs true solar noon.
2. Use the zone meridian (multiple of 5°) instead of exact longitude
   downstream.
3. Klimata: quantise latitude into 20-minute longest-day bands.
4. Rename `three-body-clock.html` → `sky-watch.html` and the repo to
   `sky-time` (breaks the live Pages URL — do together, once).
