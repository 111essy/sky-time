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

The clock quantises latitude into klimata rather than into equal degrees, in **30-minute steps of longest day** — the same step as the solar time zones on the longitude axis. That gives 24 bands per hemisphere, 48 worldwide, matching the 48 longitude zones. (This is also Ptolemy's own step in the *Geography*, where he reduced the *Almagest*'s quarter-hour parallels to half-hour intervals.)

The reason for quantising the *output* rather than the input: latitude's effect on sunrise is strongly non-linear. A degree of latitude is nearly irrelevant at the equator and dominant near the polar circle. Defining bands by day length gives each band the same timekeeping error and makes them narrow automatically where they need to:

| longest day | latitude | band width |
|---|---|---|
| 12h00m | 0.00° | — |
| 13h00m | 14.75° | 8.24° |
| 15h00m | 39.86° | 4.99° |
| 17h00m | 53.33° | 2.58° |
| 19h00m | 60.34° | 1.36° |
| 21h00m | 63.97° | 0.68° |
| 23h30m | 65.68° | 0.14° |

Error within a band is ±15 minutes of day length, or about ±7.5 minutes on sunrise.

Reference values used in the implementation (h₀ = −0.833°, δ = 23.44°):

| location | longest day | klima | representative latitude |
|---|---|---|---|
| Rotterdam 51.9225 | 16.72 h | 16h30m | +50.74 |
| Equator 0.0 | 12.12 h | 12h00m | +0.00 |
| Nairobi −1.2921 | 12.20 h | 12h00m | −0.00 |
| Sydney −33.8688 | 14.41 h | 14h30m | −34.87 |
| New York 40.7128 | 15.09 h | 15h00m | +39.86 |
| Reykjavík 64.1466 | 21.15 h | 21h00m | +63.97 |
| Tromsø 69.6492 | 24 h | **polar — no band** | — |

### Where we depart

- **Same step, different reason.** Ptolemy used quarter-hours in the *Almagest* and half-hours in the *Geography*. We use half-hours too, but for symmetry with the 30-minute longitude zones rather than by inheritance — the agreement is a coincidence worth noting, not a derivation.
- **Both hemispheres.** Ptolemy's system covered the *oikoumene*, the known inhabited world, and was northern. We mirror it south by taking the sign of the latitude.
- **Refraction included.** Ptolemy never accounted for atmospheric refraction. We use h₀ = −0.833°, which includes both refraction (~0.57°) and the sun's semidiameter (~0.27°), so our band boundaries sit at slightly different latitudes than his would.
- **Polar saturation is explicit.** Above the polar circle the day-length scale runs out — the longest day saturates at 24 hours and the bands become infinitely narrow. Ptolemy's system simply ended at the limit of the inhabited world; we treat it as a distinct state that hides the location-dependent marks rather than clamping silently.

---

## Solar time zones — 30-minute longitude bands ✅

**What came before zones: local mean time.** Until the mid-19th century every town kept its own meridian and its own clock. Places one degree of longitude apart had times four minutes apart — the same 4 min/° relation this project uses. North America had over 144 local times before 1883. Local *apparent* time (a sundial) gave way to local *mean* time as accurate mechanical clocks spread, and local mean time remained the civil standard until national standard times replaced it. So solar-based civil time is not an invention of this project; it is the older practice, and the hourly grid is the innovation that displaced it.

**Why it was displaced.** Railways. A train at 100 km/h crosses about a degree of longitude in 6–7 minutes; over a few hundred kilometres the accumulated difference reaches 20–30 minutes, which made single-track scheduling dangerous. Deadly collisions in the 1850s were attributed partly to clock confusion. British railways moved to GMT across all stations by 1847 ("Railway Time"); US and Canadian railroads imposed four zones on 18 November 1883, the "Day of Two Noons." The 1884 International Meridian Conference in Washington fixed Greenwich as the prime meridian and set the framework still in use. Note what this history actually shows: zone time was adopted for **coordination**, not accuracy. It made clocks agree with each other by making them disagree with the sun.

**Pure-longitude zones: nautical time.** The closest precedent for a grid with no political boundaries. Recommended by the Anglo-French Conference on Time-keeping at Sea (London, June 1917) and adopted by all major fleets between 1920 and 1925, nautical time divides the globe into 24 zones of exactly 15° of longitude, centred on Greenwich, with no political deviation. Ships revert to it on leaving territorial waters. What it replaced is closer still to this project: before the 20th century ships kept local apparent time, setting clocks so that noon fell when the sun crossed the ship's own meridian.

**Sub-hourly offsets are ordinary.** India runs +5:30, Iran +3:30, Nepal +5:45, Chatham +12:45. France's 1911 adoption of Greenwich was legally worded as "Paris Mean Time retarded by 9 minutes 21 seconds" — a Greenwich offset described in solar terms, because solar terms were what the law understood.

**The Dutch case.** The Netherlands ran Amsterdam Time at UTC+00:19:32 from 1909, then rounded it to exactly **UTC+00:20** in 1937, keeping it until the 1940 occupation imposed CET (retained after the war). The reasoning was precisely this project's: take local mean time and round it to a convenient shared figure. Note that the Dutch chose a *finer* rounding than we do — their +00:20 was 2 minutes from true local mean time, where our 30-minute grid places Rotterdam at +00:30, 12 minutes from it. The precedent supports the method, not the step.

### What we take

48 zones, each 7.5° of longitude wide, 30 minutes of time apart, named by their offset (`+00:30`, `−05:00`, `+09:30`). Zones are pure longitude, following nautical practice: no political shapes, no negotiation.

**Why 30 minutes.** Two constraints. The zone should stay below the equation of time (±16.4 min), which the clock carries — otherwise the zone becomes the dominant error and the case for correcting the EoT at all weakens. And the grid should be recognisable: 30 minutes contains every existing whole- and half-hour civil offset (India +5:30, Iran +3:30, Adelaide +9:30), so it reads as a familiar kind of thing rather than an invention.

| step | zones | worst error | suffixes | contains civil half-hours? |
|---|---|---|---|---|
| 60 min | 24 | ±30.0 min | 1 | — |
| **30 min** | **48** | **±15.0 min** | **2** | **yes** |
| 20 min | 72 | ±10.0 min | 3 | no |
| 15 min | 96 | ±7.5 min | 4 | yes, and quarter-hours |

This is the one place in the project where familiarity was chosen over precision, and it should be recorded as such. A 20-minute grid was implemented first and gave ±10 min; 30 minutes gives ±15, still under the equation of time but with less headroom. For Rotterdam specifically the cost is concrete and permanent: true offset +17.9 min rounds to +00:30, an error of **12.1 minutes every day of the year**, against 2.1 minutes under the 20-minute grid.

The same 30-minute step is used for the klimata on the latitude axis, so the location of any observer is two numbers of the same kind: 48 zones and 48 klimata.

### Where we depart

- **From nautical time: finer.** 7.5° gores rather than 15°, because a ±30-minute error would swamp everything else in the machine.
- **Quarter-hour offsets are not captured.** Nepal (+5:45) and Chatham (+12:45) fall off the grid; a 15-minute step would have caught them at the cost of four suffixes. Sky time is a separate system and should not be mistaken for a civil one.
- **The machine computes for the zone meridian**, not the user's exact longitude. This is what makes solar noon land at exactly 12:00 and at the exact top of the day ring for everyone in a zone — the mechanical model, where two people with the same settings see the same instrument.
- **No date line quirks.** The 180° zone is normalised to +12:00 rather than split into two 7.5° gores as nautical time does.

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
- Wikipedia, "Local mean time", "Nautical time", "Anglo-French Conference on Time-keeping at Sea", "History of time in the United States"
- Meeus, *Astronomical Algorithms* — ch. 25 (solar position), 27 (equinoxes/solstices), 47 (lunar position), 49 (phases)
- NASA Five Millennium Catalog of Lunar Eclipses
