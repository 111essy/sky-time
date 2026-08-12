# Sky Time — Historical References

Working notes on the historical systems Sky Time borrows from. One section per system: what it was, how it worked, what we take from it, and — importantly — where we depart from it.

The purpose is partly honesty. This project has been careful to distinguish real historical practice from modern invention dressed as tradition (see the Celtic Tree Calendar and Dreamspell notes at the end). Anything claimed here should be checkable against a source.

**Status key:** ✅ implemented · 🔶 specified, not built · 📋 noted, not yet decided

---

## Klimata — latitude bands by length of the longest day ✅

**What it was.** In classical Greco-Roman geography, the *klimata* (Greek κλίματα, "inclinations" or "slopes") were divisions of the inhabited Earth by latitude. The system was not defined in degrees but by an observable: **the length of the longest day**. Ptolemy (c. 150 CE) gave the canonical form in the *Almagest* (2.12) — a list of parallels beginning at the equator and stepping north so that the longest day increases in quarter-hour steps from 12 hours at the equator upward. For his geographical tables he reduced this to eleven parallels at half-hour intervals from 12 to 17 hours, and further to the famous **seven climes**, from 16°27′ N (13 hours) to 48°32′ N (16 hours).

Earlier versions existed — Hipparchus had a table of climata, and Aristotle's five-zone scheme (two frigid, one torrid, two temperate) preceded both. Ptolemy's seven-clime system became canonical through late antiquity, was adopted in medieval Islamic astronomy by al-Bīrūnī and al-Idrīsī, and survives in the Persian *haft iqlīm* ("seven climes"). Ptolemy's map legends carried three things for each place: the length of the longest day, the latitude in degrees, and the clime number.

**Why it was defined that way.** Latitude in degrees is an abstraction requiring a model of the Earth's size. Longest day is something you can *measure* with a gnomon and a water clock, from a single location, with no assumptions. It's an observable, not a coordinate. Ptolemy derived latitudes *from* it: Meroë's 16°27′ came from an observed longest day of 13 hours.

**How accurate was it?** Remarkably. Checking his seven climes against geometry using his own obliquity of 23°51′ and ignoring refraction (which he never accounted for), his latitudes agree to within **0.06°**:

| clime | longest day | Ptolemy | geometric | diff |
|---|---|---|---|---|
| Meroë | 13h | 16.45° | 16.45° | +0.00° |
| Syene | 13h30m | 23.85° | 23.81° | +0.04° |
| Alexandria | 14h | 30.37° | 30.34° | +0.03° |
| Rhodes | 14h30m | 36.00° | 36.01° | −0.01° |
| Hellespont | 15h | 40.93° | 40.87° | +0.06° |
| Mid-Pontus | 15h30m | 45.02° | 45.01° | +0.01° |
| Borysthenes | 16h | 48.53° | 48.51° | +0.02° |

**The astrolabe connection — this is the direct ancestor of our use.** An astrolabe needs a *tympan* (plate) engraved with the horizon and altitude curves for one specific latitude. Plates were therefore cut not for individual cities but **for klimata**, and astrolabes were sold as sets. The Hispano-Moorish astrolabe of c. 1260 in the Museum of the History of Science, Oxford, carries seven plates matching Ptolemy's seven climes, one inscribed `CLIA 5 LAT 41`. Severus Sebokht's 7th-century description of the astrolabe specifies that each tablet is labelled with the clime by name, its latitude in degrees, *and the hours of its longest day*.

### What we take

The clock quantises latitude into klimata rather than into equal degrees, in **20-minute steps of longest day** — chosen to rhyme with the 20-minute solar time zones on the longitude axis. That gives 36 bands per hemisphere, 72 worldwide, matching the 72 longitude zones.

The reason for quantising the *output* rather than the input: latitude's effect on sunrise is strongly non-linear. A degree of latitude is nearly irrelevant at the equator and dominant near the polar circle. Defining bands by day length gives each band the same timekeeping error and makes them narrow automatically where they need to:

| longest day | latitude | band width |
|---|---|---|
| 12h00m | 0.00° | — |
| 14h00m | 29.04° | 4.38° |
| 16h40m | 51.65° | 1.87° |
| 19h00m | 60.34° | 0.88° |
| 21h00m | 63.97° | 0.44° |
| 23h20m | 65.64° | 0.10° |

Error within a band is ±10 minutes of day length, or about ±5 minutes on sunrise.

Reference values used in the implementation (h₀ = −0.833°, δ = 23.44°):

| location | longest day | klima | representative latitude |
|---|---|---|---|
| Rotterdam 51.9225 | 16.72 h | 16h40m | +51.65 |
| Equator 0.0 | 12.12 h | 12h00m | +0.00 |
| Nairobi −1.2921 | 12.20 h | 12h20m | −3.66 |
| Sydney −33.8688 | 14.41 h | 14h20m | −33.02 |
| Reykjavík 64.1466 | 21.15 h | 21h00m | +63.97 |
| Tromsø 69.6492 | 24 h | **polar — no band** | — |

### Where we depart

- **Finer steps.** Ptolemy used quarter-hours in the *Almagest* and half-hours in the *Geography*; we use 20 minutes, for symmetry with the longitude zones rather than for any historical reason.
- **Both hemispheres.** Ptolemy's system covered the *oikoumene*, the known inhabited world, and was northern. We mirror it south by taking the sign of the latitude.
- **Refraction included.** Ptolemy never accounted for atmospheric refraction. We use h₀ = −0.833°, which includes both refraction (~0.57°) and the sun's semidiameter (~0.27°), so our band boundaries sit at slightly different latitudes than his would.
- **Polar saturation is explicit.** Above the polar circle the day-length scale runs out — the longest day saturates at 24 hours and the bands become infinitely narrow. Ptolemy's system simply ended at the limit of the inhabited world; we treat it as a distinct state that hides the location-dependent marks rather than clamping silently.

---

## Solar time zones — 20-minute longitude bands ✅

**Precedent.** Nautical time zones, in use for over a century, are exactly 15° wide and centred on multiples of 15°, with no political boundaries. Local mean time — UTC plus four minutes per degree east — was the civil standard everywhere before railways forced zone time.

**The Dutch case is directly relevant.** The Netherlands ran Amsterdam Time at UTC+00:19:32 from 1909, rounded it to exactly **UTC+00:20** in 1937, and kept it until 1940, when the occupation imposed CET. It stayed on CET after the war. So a 20-minute grid returns Rotterdam to the offset the country had chosen for itself by the same reasoning.

### What we take

72 zones, each 5° of longitude wide, 20 minutes of time apart, named by their offset (`+00:20`, `−05:00`, `+09:20`). Rounding error is at most ±10 minutes — comfortably under the ±16.4 minutes of the equation of time, which the clock *does* carry, so the zone is never the dominant error.

Zones are pure longitude: no political shapes, no negotiation. Crossing 2.5°E puts you in the next zone.

### Where we depart

- **Not aligned to civil offsets.** A 20-minute grid contains the whole-hour offsets but not the half- and quarter-hour ones (India +5:30, Nepal +5:45, Chatham +12:45). This is deliberate: sky time is a separate system and shouldn't be mistaken for a civil one.
- **The machine computes for the zone meridian**, not for the user's exact longitude. This is what makes solar noon land at exactly 12:00 and at the exact top of the day ring for everyone in a zone — the mechanical model, where two people with the same settings see the same instrument.

---

## Metonic cycle — 19 years, 235 lunations 🔶

**What it was.** 235 synodic months ≈ 19 tropical years, to within about 2 hours. Known to the Babylonians, who applied a fixed 19-year intercalation scheme from the late 6th century BC, and named for Meton of Athens (432 BC). Still the basis of the Hebrew calendar's leap-year pattern (years 3, 6, 8, 11, 14, 17, 19) and of the computus that fixes Easter — where a year's position in the cycle is the **Golden Number**, still printed in prayer books.

It is built from two coarser resonances: 99 lunations ≈ 8 years (+1.6 d) and 136 lunations ≈ 11 years (−1.5 d). 8 + 11 = 19, and the errors nearly cancel. This is why the calendar's closing cycles can only be 8 or 11 years.

The Antikythera mechanism (c. 100 BC) carried a Metonic dial and a 223-month Saros eclipse dial in bronze gearing.

### What we take
The 19-year cycle as the calendar's fourth tier, with the epact — the signed offset from solstice to the year-opening full moon — as its observable. See `sky-calendar.md` Part I.

### Where we depart
The Hebrew calendar froze its Metonic arithmetic in the 4th century and has drifted about a day every two centuries since. Ours re-anchors observationally at each age, so error cannot accumulate.

---

## The astrolabe — horizon plate and rotating rete 🔶

**What it was.** A stereographic projection of the celestial sphere: a fixed *tympan* engraved with horizon, meridian and altitude curves for one latitude, and a rotating *rete* carrying the ecliptic and star pointers. Rotating the rete by the hour angle shows the sky at that moment; where a marker crosses the horizon curve **is** sunrise. No computation — the geometry does it.

Al-Zarqālī's *saphaea* (11th-century Toledo) and later the Rojas and Gemma Frisius projections produced **universal astrolabes** that work at any latitude with one plate, specifically to solve the plate-set problem.

Prague's Orloj (1410) is a working astronomical clock built on this principle and still running.

### What we take
The conceptual model for the day ring: universal parts (the ecliptic ring at ε = 23.44°, which yields declination and is the same for every observer) plus **settable angles** (polar-axis tilt = latitude, dial rotation = longitude). Nothing needs recutting when you move — the same reason an equatorial telescope mount has a latitude scale rather than interchangeable wedges. Twilight boundaries at −6°, −12° and −18° are simply three more planes parallel to the horizon; on an astrolabe they are the almucantars below it.

---

## Equation of time and the kidney cam 🔶

**What it was.** The difference between apparent solar time (a sundial) and mean solar time (a clock), ranging −14.2 to +16.4 minutes, crossing zero four times a year (approximately 15 Apr, 12 Jun, 1 Sep, 25 Dec). It is the sum of two waves: Earth's orbital eccentricity (annual, ±7.7 min) and axial obliquity (semiannual, ±9.9 min).

Equation clocks carried it as a **kidney cam** — a two-lobed disc turning once a year, cut to the curve — from around 1700; Tompion, Graham, later Breguet. Feeding it through a differential so the clock itself shows solar time (*équation marchante*) is a grand complication; most equation clocks used a simpler separate indicator.

**It is universal.** The equation of time depends only on Earth's orbit and tilt, not on the observer. The same correction applies at Quito and Oslo to the second — which is why the same sundial correction table works everywhere, and why one cam shape fits every machine. It is also nearly immortal: the range shifts by about a minute per five centuries.

### What we take
The clock carries the EoT rather than dropping it, since it is already computed and it is what fixes solar noon to the top of the day ring instead of letting it wander 7.7° across the year.

### Open question 📋
Whether to add a small equation-of-time indicator showing how far the ring is running from mean time. One cam, one hand, no differential.

---

## Systems we deliberately do **not** use

**The Celtic Tree Calendar** (Beth-Luis-Nion, 13 months of 28 days). From Robert Graves' *The White Goddess* (1948), constructed by combining the Ogham alphabet with medieval Irish manuscripts and his own mythological theory. Not attested in any ancient Celtic source; Celticists have never accepted it as historical. Actual pre-Christian Celtic timekeeping is represented by the **Coligny calendar** (Gaul, c. 2nd century CE), which is lunisolar with months of 29 or 30 days and an intercalary month every 2–3 years — the same family as the Babylonian and Hebrew calendars, and structurally much closer to what this project built.

**The 13 Moon / Dreamspell calendar.** José Argüelles, c. 1987–1992. The number 13 is genuinely Maya — the Tzolk'in runs 13 numbers against 20 day-signs, the trecena is a 13-day week, the Long Count measures eras in 13 baktuns — but no Maya calendar has 28-day months. The Haab is eighteen 20-day months plus the 5-day Wayeb. The 13×28+1 arithmetic actually descends from Auguste Comte's Positivist Calendar (1849) and Cotsworth's International Fixed Calendar (1902), which Kodak ran internally from 1928 to 1989. Contemporary Maya daykeepers in Guatemala have objected publicly that Dreamspell is not their calendar; it also skips 29 February, breaking the continuous count that is the sacred element of actual practice.

**Why it matters here.** A 28-day "moon" drifts against the real moon by about a day and a half per month. Both systems market themselves as "natural time" while tracking nothing observable. The working definition this project uses instead: **a calendar is natural to the degree the sky can falsify it.**

---

## Sources

- Ptolemy, *Almagest* 2.12 (klimata); *Geography* Book 8 (longest-day coordinates)
- Wikipedia, "Clime" — full 33-parallel list with Ptolemy's and modern latitudes
- Museum of the History of Science, Oxford — astrolabe exhibition, "The Earth Divided by Climates"
- Severus Sebokht, *Description of the Astrolabe* (7th c.), trans. Gunther, *Astrolabes of the World* (1932)
- D.R. Dicks, "The ΚΛΙΜΑΤΑ in the Greek Geography", *Classical Quarterly* 5 (1955)
- Tupikova & Geus, "Ptolemy's data for the latitudes of Alexandria, Syene and Meroë" (2019)
- Meeus, *Astronomical Algorithms* — ch. 25 (solar position), 27 (equinoxes/solstices), 47 (lunar position), 49 (phases)
- NASA Five Millennium Catalog of Lunar Eclipses
