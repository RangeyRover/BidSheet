# BidSheet Roadmap

*Reviewed 2026-07-21. This document is the living roadmap. It consolidates and
supersedes §6 of [`research/bid-management-competitive-analysis.md`](research/bid-management-competitive-analysis.md)
(kept for the full competitive reasoning) and sits alongside the iOS field app
roadmap in `bidsheet-cloud/docs/ios-field-app-roadmap.md`.*

The app has closed most of the table-stakes gaps identified in the June
competitive analysis. The biggest lever now is not another feature — it is
**discoverability**: the product is competitive with $166/mo tools but nobody
can find it, the README still says "Screenshots (coming soon)", and the site
has no visuals. Track 1 below is therefore first.

---

## Track 1 — Discoverability (top priority)

### 1a. Ready now (not blocked on screenshots or video)

- **GitHub repo polish.** Add repository topics
  (`construction`, `estimating`, `takeoff`, `electron`, `heavy-civil`,
  `open-source-alternative` …), a social-preview card (logo + tagline is
  enough — no app screenshot required), and enable Discussions so contractor
  feedback has a home that isn't the issue tracker.
- **Package managers = distribution = discovery.**
  - **winget** (Windows) — manifest submission needs no screenshots and is
    where Windows users increasingly search first.
  - **Chocolatey** and an **AUR** package as follow-ons.
  - (Flathub requires screenshots — moves to 1b.)
- **Directory listings that don't gate on screenshots.** AlternativeTo
  (listed as a free alternative to HeavyBid / SharpeSoft / TCLI — screenshots
  optional), opensourcealternative.to, LibHunt, a SourceForge mirror for its
  download traffic.
- **SEO expansion on bidsheet.co.** The trench-volume calculator is the
  proven template; each new calculator is a landing page for a search intent:
  pipe slope/fall, bedding & backfill tonnage, asphalt/concrete restoration
  quantities, dewatering flow. Add comparison pages ("free HeavyBid
  alternative", "TCLI Estimating Link alternative") mirroring the keywords
  already in the site meta, and short how-to guides ("how to bid a water main
  job") that end in the app.
- **Release cadence with real notes.** Every GitHub Release with readable
  notes feeds watchers, RSS, and the package-manager updaters above.
- **Community seeding.** Answer real estimating questions (r/estimators,
  r/Construction, Eng-Tips, heavy-civil forums) and link only where genuinely
  relevant. Slow but compounding; zero assets required.

### 1b. Blocked on screenshots / video — and how to unblock

Needed for: README screenshots section, `og:image` (currently just the icon),
site hero, Capterra / G2 / Software Advice listings, Flathub, Product Hunt,
and YouTube demos.

**Unblocker: the screenshots can largely be automated.** The app already
ships a seeded sample catalog and starter assemblies, so a scripted demo
dataset + driving the Electron app headed (Playwright supports Electron) can
capture consistent, always-current screenshots of the takeoff screen, bid
grid, trench profiler 3D view, and proposal PDF — regenerated in CI so they
never go stale. Videos can reuse the same scripted walkthrough as a screen
recording with narration added after. This harness is a buildable roadmap
item, not a manual chore.

---

## Track 2 — June roadmap status (what actually shipped)

| # | Item (June analysis §6) | Status | Evidence |
|---|---|---|---|
| 1 | Bid-day pricing: indirect-cost pool | **✅ Shipped** | `IndirectCostsCard.tsx`, spread through unit prices in every output |
| 1 | Bid-day pricing: per-cost-type markup | **❌ Not started** | `bidCalc.ts` still applies one OH%/profit% to total direct cost |
| 1 | Bid-day pricing: interactive unit-price shaping screen | **❌ Not started** | markup folding still happens only at CSV export |
| 2 | Quote leveling + self-perform-vs-sub | **✅ Shipped** | `QuotesTab.tsx` side-by-side with self-perform direct-cost comparison |
| 3 | Bid-schedule import (CSV/Excel) | **✅ Shipped** | `BidItemImportModal.tsx`, `ipc/bids.ts` |
| 4 | Trench Profiler fix / README mismatch | **✅ Shipped** | re-enabled in `manifest.ts`, plus 3D view + depth-band summary |
| 5 | Per-job document storage | **✅ Shipped (local)** | `DocumentsTab.tsx`, managed store, nested folders |
| 5 | E2EE document cloud sync | **❌ Not started** | `sync-engine.ts` has no document awareness yet |
| 6 | Section templates + multi-resource lines | **✅ Shipped** | `SectionTemplatePickerModal.tsx`; assemblies carry labor + equipment and expand from takeoff |
| 7 | Depth-/condition-based production rates | **❌ Not started** | `production_rates.conditions` is still free text; profiler now knows depth bands — the hook exists |
| 8 | Vendor database + RFQ tracking | **🟡 Partial** | vendor autofill from quote history; no vendor entity, no RFQ generation/tracking |
| 9 | Bid-analysis report + win/loss breakdowns | **✅ Shipped** | `BidAnalysisModal.tsx`; dashboard win rate by client and bid size |
| 10 | Bid balancing/unbalancing | ❌ Later (unchanged) | — |
| 11 | Actual-production capture | Moved | now Tier 2 of the iOS field-app roadmap |
| 12 | DOT e-bid submission | ❌ Later (unchanged) | — |
| 13 | Multi-estimator concurrent editing | ❌ Later (unchanged) | — |

Also shipped beyond the roadmap: metric units, client records, auto job
numbers, stale-price warnings, compaction/waste factors (issue #9, trimmed
scope — still open for swell factors), PDF proposal customizer, expanded
walkthrough, and the iOS field app foundation with CI.

---

## Track 3 — Remaining product priorities

Ordered by value ÷ effort for a small utility sub, carrying forward the
unfinished June items:

1. **Per-cost-type markup.** Different margin on labor vs. material vs.
   equipment vs. sub. The smallest remaining cost-engine gap and still
   table-stakes everywhere from TCLI up. Builds directly on `bidCalc.ts`;
   the per-section override pattern already shows how to thread a new knob
   through UI/PDF/CSV.
2. **Bid-day unit-price shaping screen.** The last piece of June item 1: a
   live cost-vs-price/margin view where unit prices can be nudged and the
   spread recomputes — what estimators stare at in the final hour. The math
   (indirect spreading, folded unit prices) all exists; this is a screen, not
   an engine.
3. **E2EE document cloud sync.** Generalize per-job sync from "one plan" to
   "N documents" (the R2 key convention and desktop encrypt/dedupe pipeline
   already support it). Unlocks iOS Tier 3 "documents in the field" nearly
   for free, and encrypted job files at $20/mo is something no competitor
   offers at this price.
4. **Depth-band production rates.** Wire the trench profiler's existing
   depth-band summary to rate selection — the one variable underground
   estimators most want to vary, and a differentiator no cheap tool has.
5. **Vendor records + RFQ tracking.** Promote vendors from autofill strings
   to records (the client-records pattern just shipped is the template), then
   add "who did we ask / who answered" tracking. Full emailed-RFQ generation
   can come later.

---

## Track 4 — New candidates from other programs (added 2026-07-21)

Capabilities observed in the broader field that the June analysis didn't
prioritize, now worth considering:

- **Count takeoff tool.** *(Bluebeam, STACK, PlanSwift — table-stakes in
  every takeoff product.)* Tap to count fittings, valves, services, inlets
  with per-symbol totals and send-to-bid. BidSheet measures lines and areas
  but cannot count discrete items — the most conspicuous takeoff gap, and a
  natural fit for the existing overlay/send-to-bid machinery. **Recommended
  next takeoff feature.**
- **Plan revision / addendum overlay compare.** *(Bluebeam overlay, PlanGrid
  version compare.)* Addendum drops → overlay new sheet on old, differences
  tinted. BidSheet already has the viewer, the Documents tab (addenda
  category), and calibrated pages; a two-layer tinted render is a bounded
  feature with outsized bid-day value. Differentiator at this price point.
- **Formatted Excel estimate export.** *(Everyone — estimators live in
  Excel; TCLI/SharpeSoft trade on Excel-friendliness.)* A styled .xlsx of
  sections/items/markups (beyond bare CSV) so BidSheet output can go to a GC
  or reviewer looking like an estimate, not a data dump.
- **Equipment rate builder.** *(HeavyBid resource rates; blue-book rate
  guides.)* Purchase price, life, fuel, maintenance → derived hourly rate.
  Small subs guess these numbers today; a wizard writing into the existing
  `equipment` table is cheap and sticky.
- **Quote validity tracking.** Quotes carry dates already — add an expiry
  and warn when a bid leans on an expired quote, same pattern as the shipped
  stale-price warnings.
- **Bid calendar / pipeline view.** *(Bid boards: BuildingConnected,
  SmartBid.)* The dashboard already tracks urgency and status; a calendar
  view with .ics export of bid dates is a small step with daily-use value.
- **Watch, don't build yet:** AI auto-takeoff (Togal/Kreo/Beam) — revisit
  when the count tool ships and there's a corpus of real takeoffs to learn
  from; proposal e-signature — needs a third-party service and undermines
  the local-first story; export a signature-ready PDF instead.

---

## Explicitly not building (unchanged)

- A HeavyBid-style activity/codebook rewrite — assemblies + a better bid-day
  screen get ~80% of the value while staying approachable.
- Time-clock/payroll/scheduling/project management (field-app side).
- Electronic DOT bid submission and multi-estimator concurrent editing stay
  parked until user demand shows up.
