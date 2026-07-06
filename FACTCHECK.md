# Fact-check ledger — "The city's own land"

Rigorous fact-check performed **2026-07-06**. Every number was re-derived from the raw data (`build/colp_records.json`, the 17,298-record COLP pull) and cross-checked against the live COLP API (`data.cityofnewyork.us/resource/fn4k-qyk2`), the MapPLUTO ArcGIS service, and the NYC Mayor's Office. Findings, and the corrections applied in the same session, are below.

## Errors found and fixed

### ❌ Major — "19,429 acres of vacant land" was ~13× too high and mislabeled
The acreage summed lot area over **no-use records**, not lots. Because COLP is a list of *uses*, one tax lot can appear many times.
- **John F. Kennedy Airport** is a single 214-million-sq-ft tax lot (BBL `4142600001`) recorded 8 times, 3 of them as "NO USE" (EDC, DSBS, DCAS). Counted per-record, JFK alone added ~640M sq ft — the bulk of the 846M sq ft (19,429 acres) that was summed.
- **Fix:** collapse records to lots and apply an active-use precedence rule (below). JFK, Rikers and the marsh now fall out of "no use." Corrected figure: **~1,494 acres** (65M sq ft) across genuinely-unused lots.

### 🔶 Moderate — "17,298 parcels" counted use-records, not parcels
17,298 COLP records sit on **15,188 distinct tax lots** (≈1,600 lots have multiple records). The header, counter, category counts and acreage all treated records as parcels.
- **Fix:** the map now works at the lot level. Header reads **15,188 tax lots**; every count is distinct-lot.

### 🔶 Moderate — active facilities filed under "no current use / clearest opportunity"
A lot with several records could be both "in use" and "no use." **Rikers Island** (BBL `2026050040`) has a CORR "criminal justice facility" record and a DCAS "NO USE" record; JFK likewise.
- **Fix:** precedence rule — a lot is "no current use" **only when every one of its records is no-use**; otherwise it takes its active use (residential › other › parks › no-use). Rikers/JFK now read as "other active use." The "clearest housing opportunity" wording was softened, and popups note when a lot combines multiple recorded uses.

### ⚠️ Minor — knock-on counts
No-use lots **3,132 → 2,569**; category counts moved to distinct lots (other 4,939; parks 6,497; residential 1,183). Under-5,000-sq-ft share **64% → 72%** (now matches Furman's ~70%). No-use manufacturing **580 records → 326 lots**. `lots.geojson` de-duplicated from 16,982 stacked features to **14,940** unique polygons.

## Verified accurate (unchanged)

- Total COLP records **17,298** ✓ (live API); category record-counts as originally computed ✓
- Zoning counts by lot: R 9,928 · M 1,724 · C 931 · PARK 2,456 · BPC 4 · none 145 ✓
- PLUTO join match 15,107/15,188 (99.5%) ✓; lot geometry retrieved for 14,940 lots ✓
- **LIFT task force** ✓ — Mayor Mamdani's Day-1 executive order; goal of **at least 25,000 units over ten years**, sites identified by July 1, 2026 (NYC Mayor's Office)
- Furman shares (parks >40%, no-use ~a fifth, residential <10%) ✓ — consistent with the lot-level shares (43% / 17% / 8%)

## Unverified / disclosed limitations

- **Furman Center's exact figures** (e.g., "417 million sq ft," "70% of vacant lots under 5,000 sq ft"): the Furman page blocks automated access (HTTP 403), so these were confirmed only against secondary summaries, not the primary article. The category *shares* and the 5,000-sq-ft finding are independently reproducible from COLP.
- The popup "≈ N homes" estimate and the 3D extrusion height are disclosed modeling choices (lot area × residential FAR), not measured facts.
- "No current use" means no use *on record*; a lot can be occupied, contaminated, leased or otherwise unavailable in ways COLP does not capture.

---

# Second review — fresh/adversarial pass (2026-07-06)

A cold re-check, skeptical of the corrections above as well as the original. Everything was re-derived from the current deduped data and probed for assumptions not previously tested.

## Held up under fresh scrutiny
- **Totals validate against the primary framing.** Distinct-lot areas sum to **2.18 billion sq ft**, matching the Furman Center's "2.2 billion"; the parks/cultural lot-share (43%) matches Furman's ">40% of lots." Strong end-to-end validation of the dedup and area math.
- **The no-use bucket is clean:** 87% is "vacant land," **0 lots are leased-in**, and ownership is city (`non_city_ownership = C`) on all but ~25 records. JFK/Rikers stay out. BBL integrity: 0 invalid, 0 duplicate lots.
- Every displayed number (15,188 · 2,569 · 1,494 acres · 72% · zoning counts) re-derived and correct.

## New findings
- 🔶 **"Parks & recreation" was imprecise.** COLP's cultural/recreational category's single largest use type is **wetlands/natural areas (2,628 records)**, ahead of parks (1,458), with community gardens (755), playgrounds and a few branch libraries also inside. **Fixed:** relabeled **"Parks & natural areas"** with an accurate description (standalone map and Every Building overlay).
- 🔷 **No-use acreage is a conservative floor, disclosed.** Furman's "417M sq ft / nearly 3,000 lots" counts every lot with any no-use record (incl. the 214M-sq-ft JFK lot); this map's stricter rule yields ~1,500 acres. Neither is "the developable acreage." **Fixed:** methodology now explains the gap explicitly.
- ⚠️ **Precedence distorts land-area shares** (not lot counts): a large natural/park lot carrying an ancillary infrastructure record is assigned to "other active use," so the parks *area* share (41%) sits below Furman's 68%-of-land. Immaterial to the housing-opportunity purpose (both are non-developable); noted as a limitation. The map only claims *lot* shares, which match Furman.
- ⚠️ **Data edition clarified:** COLP is bi-annual; this is the **May 2026** edition (pulled July 2026). Methodology updated.

## Still unverified
- The **Furman Center's exact figures** are now **corroborated by multiple consistent secondary summaries** ("417 million sq ft… nearly 3,000 lots"; "more than 40 percent of lots and 68 percent of the City's land"), and independently reconciled with this data — but the primary page still blocks automated access (Cloudflare 403; Wayback unreachable; the user's Chrome was not running), so they are not confirmed by direct read of the source.

