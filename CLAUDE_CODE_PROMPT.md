# Spirit Airlines Legacy — Claude Code Handoff

> Paste everything from the line below into Claude Code.

---

## Who this is for

This is a **personal gift** for an **A&P (Airframe & Powerplant) technician** who turned wrenches on Airbus A320s at Spirit Airlines until the airline shut down in **November 2024**. After 40+ years, "Bare Fare yellow" goes dark. He kept those jets flying every day — overnight checks, line maintenance, deferred items, the unglamorous middle-of-the-night stuff that 99% of passengers never see.

This page is a **digital hangar** — equal parts museum, training wall, and quiet thank-you. Treat every design choice through that lens. If a flourish makes it feel more like a love letter and less like a marketing site, lean in.

## The file

**Single self-contained HTML file. No build step. No external assets. Double-clickable from Finder.**

Path: `/Users/earllacey/Documents/Claude/Projects/Spirit Airlines Legacy/spirit_legacy.html`

Source materials (do **not** ship them, but mine them for authenticity if anything is unclear):
`/Users/earllacey/Documents/Claude/Projects/Spirit Airlines Legacy/Spirit Airlines/`
 - `40 Hour Gen Fam Course #108T21/` — the actual ATA chapter curriculum
 - `Airbus World & Airnav Presentation/`
 - `TRAX Presentations/`
 - `Other Helpful Files/`

## Constraints (non-negotiable)

1. **One file.** No CDN, no Google Fonts, no images, no JS frameworks. SVG + CSS + vanilla JS only.
2. **Offline-first.** Must render identically with WiFi off.
3. **System font stack.** `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif` for body; `'Courier New', ui-monospace, monospace` for technical readouts.
4. **Spirit yellow is `#FCD116`.** Background black `#080808`. Don't drift.
5. **Respect `prefers-reduced-motion`.** Disable all non-essential animation when set.
6. **Keyboard-navigable.** Every interactive element reachable by Tab, with visible focus rings.
7. **Mobile-clean.** Single-column collapse below 720px; nothing should horizontally scroll.

---

## Priority 0 — Fix the visibility bug

Sections after the hero use class `fade-up { opacity:0; transform:translateY(28px) }` toggled by an `IntersectionObserver` adding `.vis`. Some never trigger.

**Do this:**
1. Make sections **visible by default**. The fade-in is enhancement, not a gate.
2. Reverse the model: start with `.fade-up.vis` styles applied, then *only if* JS loads and `IntersectionObserver` is supported AND `prefers-reduced-motion` is **not** set, strip `.vis` from off-screen elements and re-add it on intersect.
3. Use `threshold: 0.05` and `rootMargin: '0px 0px -10% 0px'` so things appear slightly before they're fully on-screen.
4. Audit the `#aircraft` section's `.inner` wrapper — unify all sections to the same `.section` shell so spacing/padding is consistent.

**Acceptance:** With JS disabled, every section is fully visible. With JS enabled, sections animate in once and stay.

---

## Priority 1 — Content and structure (must-have)

### Nav

- Sticky top bar, blurred black, 1px yellow underline.
- Links: **Aircraft · ATA · Engines · Cockpit · TRAX · Timeline · Tribute**.
- Active link follows scroll position via `IntersectionObserver` on each `<section id>`. Active link gets a yellow underline + white text.
- Add a small **"NK" tail-fin SVG** as the brand mark (Spirit's IATA code).
- Mobile: collapse links into a hamburger that opens a full-screen overlay.

### Hero

Keep the FIDS departure board, runway perspective, and twinkling stars. Improve:

- **FIDS** shows 5 rows. Statuses cycle every 4s with a subtle flip animation: `BOARDING → DEPARTED → ON TIME → DELAYED → CANCELLED → FINAL`. The last row is permanently `FINAL · NOV 2024` and never flips — it's the anchor.
- Below the title, a single line in mono caps: `EST. 1980 · BARE FARE · NK · A320 FAMILY · END OF LINE`.
- Replace the small SVG silhouette in the hero with a **larger top-down A320** rendered cleanly in SVG (single-color outline, yellow accent on the wings).

### Stats bar

Four cells, big yellow numbers, mono labels:
- **205** Aircraft at peak
- **1983** Founded
- **2024** Final flight
- **40 hrs** Gen Fam course

Animate counts from 0 → final on scroll-in.

### Aircraft Zones (new full-width section)

Below the stats bar, a **side-view A320 SVG** spanning full width. Six clickable zones:
- **Cockpit** — ATA 22 (Auto Flight), 23 (Comm), 31 (Indicating), 34 (Nav)
- **Forward Fuselage** — ATA 21 (A/C), 25 (Equipment), 38 (Water/Waste)
- **Wing** — ATA 27 (Flight Controls), 28 (Fuel), 30 (Ice & Rain)
- **Engine** — ATA 71 (Powerplant), 70-series, 36 (Pneumatic), 26 (Fire)
- **Aft Fuselage** — ATA 32 (Landing Gear), 33 (Lights), 35 (Oxygen)
- **Tail** — ATA 49 (APU), 27 (Rudder/Elevator)

Click a zone → the matching ATA chapters highlight in the grid below AND a side panel slides in with a one-paragraph description of what that zone does on the A320. Active zone gets a yellow glow.

### ATA Chapter Grid (21 cards)

Filter bar above the grid — pill buttons:
**All · Day 1 · Day 2 · Day 3 · Day 4 · Day 5 · Powerplant**

Day mapping (this matches the actual 40-hour Gen Fam course):
- **Day 1:** ATA 22, 23, 24, 31, 46
- **Day 2:** ATA 27, 29, 32, 33, 34
- **Day 3:** ATA 25, 28, 35, 38, 47, 52
- **Day 4:** ATA 26, 49, 71
- **Day 5:** ATA 21, 30, 36

Filtering uses CSS class toggles + a fade transition (no DOM rebuild). Cards expand inline on click — verify smooth `max-height` animation and that multiple cards can be open simultaneously. Each card shows ATA #, name, one-line plain-English blurb, and a "tip" line with a memorable detail (e.g., "ATA 49 — APU is the small jet engine in the tail that runs ground power and starts the mains").

### Engines — V2500-A5 vs PW1100G-JM

Two-column comparison. Add for each:
- **Animated thrust gauge** — horizontal bar fills to relative thrust on scroll-in (V2500-A5: ~25,000 lbf · PW1100G-JM: ~27,000 lbf).
- **Fan diameter visualization** — a yellow circle outline that morphs in size between the two engines on hover.
- "Era" tag: V2500-A5 = `CLASSIC NEO`, PW1100G-JM = `NEO GEARED`.

Below the engine cards, the **APU** subsection (Honeywell 131-9A): keep the existing card, add a small "starts mains, provides ground power, bleeds cabin air" bullet trio.

### Cockpit

Grid of cards: **PFD, ND, ECAM E/WD, ECAM SD, MCDU, Sidestick, Throttle Quadrant, Thrust Levers, FCU, Overhead Panel, Pedestal, Rudder Pedals**. Each card: SVG mini-icon (drawn inline, no emojis required), name, two-line description in technician-speak.

### TRAX (the maintenance software)

Keep the desktop-window chrome (red/amber/green dots, title bar). Improve:
- Grid of **21 module tiles**.
- Click a tile → the right side of the board flips to show a **mock TRAX detail pane**: form-style fields with the workflow's typical inputs (e.g., for "Defect Open": flight #, SDR/MRR flag, phase, station, reported-by, defect description). The currently selected tile is highlighted yellow on the left list.
- All 21 module descriptions (verbatim — these are real workflows):

1. **Defect Open** — Log a new aircraft discrepancy: flight number, SDR/MRR flag, phase, station, reported-by, and verbatim logbook description.
2. **Defect Close** — Close out a resolved squawk with corrective action and sign-off.
3. **Oil Used Entry** — Record engine or APU oil added during servicing.
4. **Defect DMI Creation** — Create a Deferred Maintenance Item from an open defect.
5. **Defect MWI Creation** — Create a Maintenance Work Item for scheduled work.
6. **DMI & MWI Closing** — Close out deferred and scheduled items upon completion.
7. **DMI & MWI Troubleshooting Action Entry** — Document troubleshooting steps taken.
8. **Defect Parts Required** — Add parts needed to resolve a defect.
9. **Open MWI Report (229 Report)** — Pull the open maintenance work item report.
10. **Work Order Print** — Print a work order package for the floor.
11. **Work Order Search** — Locate existing work orders by tail, date, or type.
12. **Work Order Task Card Sign Off** — Electronically sign off completed task cards.
13. **Part Number Search & Pick List** — Search IPC for parts and create a pick list.
14. **Tooling Search** — Find required special tools for a given task.
15. **Delay Report Creation** — Document a departure delay with cause code and duration.
16. **Parts Transaction Log Page** — View parts transaction history for a work order.
17. **Parts Transaction Work Order** — Link parts transactions to specific work orders.
18. **Open DMI Report** — Pull the current open deferred maintenance item list.
19. **Reprint Parts Transaction Tag (M-20A)** — Reprint a parts tag for a transaction.
20. **Non-Routine Linking Procedures** — Link non-routine cards to base work orders.
21. **Production Turnover SOP** — Document shift handover status for in-work tasks.

### Timeline — "Runway to Sunset"

Reframe the timeline as a **runway centerline** receding into the distance. Events are **waypoints** along the centerline — yellow runway-marking shapes (rectangles like centerline stripes) instead of dots. A subtle vertical gradient on the runway: bright yellow at the top (early years) fading to dim amber at the bottom (2024).

Events to include — verify and expand:
- **1980** — Charter One founded
- **1992** — Renamed Spirit Airlines
- **2006** — Ultra-low-cost model launches
- **2007** — First A319 delivery (jet era begins)
- **2010** — IPO, NK on NYSE
- **2015** — A320neo orders
- **2019** — Fleet hits 145+ aircraft
- **2022** — JetBlue merger announced
- **2024** — Merger blocked, Chapter 11, **November final flight**

The 2024 entry sits at the visual horizon line, with a small yellow `★ FINAL` flag.

### Tribute

Keep the centered text. Add:
- **Floating embers** behind the text — small yellow dots (8–12) drifting upward at varying speeds, fading at the top, looping. Pure CSS animation. Disabled under `prefers-reduced-motion`.
- A **subtle scan line** overlay on the placard like an old CRT, maybe 2% opacity.
- The signature line should accept a customizable name. Look in `Other Helpful Files/` for the recipient's name — if found, use it; otherwise leave a placeholder `— FOR ___________`.
- End the page with a **runway lights fade**: the last 200px is solid black with two small yellow runway-edge lights that pulse slowly (like a runway closing for the night).

---

## Priority 2 — Polish

- **Scroll-to-top button**: appears after 300px scroll, bottom-right, yellow circle with up-arrow SVG, smooth scroll on click.
- **Custom focus ring**: 2px yellow outline + 2px black offset on every focusable element.
- **Selection color**: yellow background, black text.
- **Print stylesheet**: print-friendly version that strips animations and dark backgrounds, lays out as a clean document with all section content. (He might want to print this for his hangar wall.)
- **Subtle hangar ambience** *(optional, off by default)*: a small speaker icon in the nav. Click → plays a looping low-rumble of distant jet engines via the WebAudio API generating noise (no asset needed). Off by default. Persist preference in `localStorage`.
- **Easter egg**: typing `KCAK` (Spirit's old MX base in Akron) opens a small flight-strip overlay with a one-line "Thanks for keeping the fleet flying."
- **Document title** flips to `★ FINAL · NOV 2024` when the tab loses focus, restores on return.

---

## Voice and tone

- **Technician, not marketer.** Write blurbs the way an experienced A&P would describe a system to a new-hire. Specific, dry, occasionally proud. Never breathless.
- **No emoji in body text.** Use SVG icons. Icons in section labels are okay if they read as instrumentation, not decoration.
- **Mono for anything that should look like a readout** — flight numbers, ATA codes, timestamps, FIDS rows, TRAX fields.

---

## Acceptance checklist

Before declaring done, verify all of these:

- [ ] Open the file by double-clicking from Finder. Every section visible without scrolling triggers.
- [ ] Disable JS in DevTools, reload — every section still readable, content intact.
- [ ] Toggle `prefers-reduced-motion` in DevTools — animations halt, content remains usable.
- [ ] Resize to 375px width — no horizontal scroll, nav collapses, grids stack.
- [ ] Tab through the page — every interactive element reachable, focus ring visible.
- [ ] Filter the ATA grid by Day 1 — only the 5 expected chapters remain, others fade out cleanly.
- [ ] Click 3 different TRAX modules — detail pane updates each time, no flicker.
- [ ] Click each Aircraft Zone — correct ATA cards highlight in the grid below.
- [ ] Print preview — readable B&W document, no clipped content.
- [ ] Lighthouse Performance ≥ 95 (it should — no network calls).
- [ ] No console errors or warnings.

---

## How to work this

1. **Read** `spirit_legacy.html` end-to-end first. Note what already works.
2. **Fix P0** (visibility bug) before touching anything else, and verify in a browser.
3. **Implement P1 sections** one at a time. After each, save and re-verify in a browser — the fail mode here is shipping a giant rewrite that broke three things.
4. **Layer in P2 polish** last.
5. **Run the acceptance checklist** in a real browser. Don't skip the JS-disabled and reduced-motion checks.
6. Save back to the same path. Don't create new files.

This page will outlive the airline it celebrates. Make it worthy of the work it commemorates.
