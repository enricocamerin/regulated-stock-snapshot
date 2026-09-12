# HANDOFF — DG MARE regulated-stock work

> Read this first in every new chat: `https://raw.githubusercontent.com/enricocamerin/regulated-stock-snapshot/main/HANDOFF.md`
> Update it at the end of every session. Last update: 2026-09-12.

## Repos and sites
| what | where | notes |
|---|---|---|
| Presentation (HTML/JS) | `enricocamerin/regulated-stock-snapshot` (public) → Netlify site **regulatedstocksnapshot** | pushes deploy automatically |
| Qlik scripts | `enricocamerin/dgmare-ecr-snapshot-analysis` (private) | `apps/V-FAM.qvs`, `reference/stock_area_geo.csv`, `CHANGELOG.md` |
| GitHub push | needs a personal access token pasted in the chat (repo scope); Claude redacts it in logs |

## Presentation — state at 0222a14
Slides (Enter = next; header menu Home · Species · Map · Families; Explorer exists but is hidden from the menu):
1. **Home** — vessel at the quay; parent codes block (Parent Stock Area 2AC4-C, Parent Species SRX, NLD, 2025); space = family boards, quota clouds over the parent (654.400 t) and the child with a ceiling (*07D2. 72.400 t).
2. **Species** — cards with photos in `img/` (thornback CC0 Ecomare Texel; blonde/spotted from Commons, author+licence still to confirm; ling = 1896 Goode & Bean plate). SRX keeps a drawing.
3. **Map** — parent frame (green) over 2a + 4; `04-C.` blue inset (code only); `*07D2.` yellow with "Adapted quota 72.400 t · uptake 99.17 %". Legend: parent / asterisk code. Space = voyage: start north (2a) → one stop in 4b hauling RJH, RJC, RJM → 7d → fast straight home. Ledger: haul / catch / cumulative, own quota where present, parent total + uptake.
4. **Families** — trees with build animation; every block shows Quota and Uptake. Snapshots: 12 Feb (671.748) and 13 Aug (= portal figures, 647.932, 99.01 %). Deltas vs February shown in red on the August view.
Data is hand-typed in `FAMILIES` (index.html). `STOCK_GEO` (865 codes decoded) and `OUTLINES` (merged division shapes) are embedded.

## Qlik — state
- **V-FAM 2.5.12** validated 2026-09-11, tag `vfam-2.5.12`: discards (DISC) + scientific (SCIENT_NUQ) excluded via L1 hash map; member-level change fields (9b). NLD SRX/2AC4-C 13 Aug = 647.932.
- **V-FAM 2.5.13** committed, pending validation: `quota_adapted_qt` from L1 per quota key; pivot measures Adapted quota / Uptake (see CHANGELOG). Tag after validation.
- **V-BKP 3.7** deployed on the server, script NOT yet in the repo — paste it to add `apps/V-BKP.qvs`.
- Snapshot folder: 15 files 20260212–20260813 + 20260907 (first wide file) + 20260908 (accidental, to delete).

## Rules established (do not re-derive)
- Family = immediate parent (portal semantics); middle nodes get head copies into their own family.
- One counted row per declaration per family per snapshot (`is_total_row`); consumer measure `Sum({<is_total_row={1}>} weight)`.
- Drill measures: Catches = regional = msl on both segments; SC = otherwise.
- Portal rule: discards and scientific catches are not counted against quota.
- Asterisk in a code = the line is written as a special condition; its catches are booked as "special condition catches". NOT a parent/child marker: RJH/RJC/RJM are children without asterisk. Parent/child = `msl_parent_quota_*` (portal "Parent Stock" columns). Adapted quotas of child lines are independent, not shares of the parent.
- No apostrophes in Qlik TRACE text.

## Open items
- Validate 2.5.13 (pivot: parent 654.400 / 99.01 %; *07D2. 72.400 / 99.17 %; species lines 0 / N/A) → tag.
- DB queries to settle the asterisk criterion (is there a special-condition flag? values of `fishing_cat_code`?).
- Pivot label wording for children (last proposal: [own quota · also counted against the parent] / [no own quota · counted against the parent]).
- Confirm author/licence for blonde and spotted ray photos; local copies already in `img/`.
- FAO areas layer download (fifao:FAO_AREAS_CWP) for real polygons; EEZ clip for Union waters.
- Restate the validation set in the V-FAM header after 2.5.13.

## Commit history (presentation repo) — each id is a full copy of index.html at that point

- `3f3c8d6` 2026-09-12 — HANDOFF.md: session state for continuity across chats
- `0222a14` 2026-09-12 — Voyage ledger: haul / catch / cumulative per member, own quota and uptake where a line has one, parent total with adapted quota and uptake; lookup by member not by code
- `fed234c` 2026-09-11 — Map panel: descriptions only; voyage table with catch, adapted quota and uptake per code; no narrative text
- `7c101b4` 2026-09-11 — Map: no 'special condition' wording anywhere; codes, quota and uptake only; 7d division label moved clear
- `f3ae0b0` 2026-09-11 — Map: condition areas back on the static map - 04-C. blue with bare code, *07D2. yellow with special condition, adapted quota and uptake
- `f345098` 2026-09-11 — Voyage: the condition's area fades in as the vessel arrives - yellow with adapted quota and uptake for asterisk codes, blue otherwise
- `66f7034` 2026-09-11 — Map: parent outline only; conditions appear only as the vessel fishes them, card marked special condition; panel role on the left
- `51ecd61` 2026-09-11 — Map: bare code on non-asterisk conditions; quota and uptake only during play, on the catch card when the vessel is on an asterisk area
- `328624c` 2026-09-11 — Asterisk areas: adapted quota and uptake shown in the yellow box and in the panel
- `5f68c91` 2026-09-11 — Map panel: code, full area description and species only; asterisk areas in yellow on the map and in the panel
- `b6aab70` 2026-09-11 — Map: parent label on two lines (Parent Stock Area / Parent Species), FAO 2a label moved clear
- `1269d93` 2026-09-11 — Map labels: Parent Area · Parent Species on the parent, special condition on the rest; fast straight return home
- `0129767` 2026-09-11 — Map: parent label large; 7d drawn to the real Channel shape; voyage stops once in the North Sea and hauls three species without moving
- `ec1a692` 2026-09-11 — Two snapshots only: portal figures assigned to 13 Aug 2026 (647.932 t, 99.01 %), compared with 12 Feb 2026
- `203e70e` 2026-09-11 — Third snapshot 11 Sep 2026 from the portal (647.932 t, 99.01 %); detailed two-masted vessel with rigging; cover title removed
- `2db8886` 2026-09-11 — Cover: Regulated Stock header with parent area/species codes, member state, year; quota clouds over the parent and the child with a ceiling when they board
- `a98ca5f` 2026-09-10 — Every block shows its quota and uptake: 654.400 / 72.400 with uptake, 0.000 / N/A on the species conditions
- `3c80018` 2026-09-10 — Menu without Explorer, no Next button; cover states parent species, area and quota; trees show adapted quota and uptake
- `34f3e65` 2026-09-10 — Slide menu in the header: Home / Species / Map / Families / Explorer, jump anywhere; Next button steps through
- `1ba5543` 2026-09-10 — Restore c0ba71a (vessel cover, species, map with voyage, families, explorer) with the pinned footer
- `28aaf78` 2026-09-10 — Restore the two-slide version (00fd6a5): the vessel cover and the family trees
- `01010e6` 2026-09-10 — Layout: stage capped to the viewport, footer with navigation pinned to the bottom
- `feced3e` 2026-09-10 — Cover: the net is the parent quota - mesh enclosure with corks and a float carrying the code; three vessels fish inside it
- `3520966` 2026-09-10 — Cover: three vessels leave the parent quota to fish three species in three waters; each catch pulses back to the parent counter
- `bf0e301` 2026-09-10 — Cover: the vessel is the group of boats under one parent quota (species code + area code); labels no longer collide; text clear of the mast
- `c0ba71a` 2026-09-10 — One vessel stands for the fleet: moored boats at the start point, wording on cover, map and voyage end; fleet count field
- `26020ef` 2026-09-10 — Revert to the single-vessel voyage (7225a8b): two boats made the map unreadable
- `b9ad912` 2026-09-10 — Two vessels sailing at once: northern and southern grounds, both crediting the same parent; split ledger
- `7225a8b` 2026-09-10 — Voyage starts and ends in the north; species appear only as the boat catches them; small card icons
- `400b036` 2026-09-10 — Ling on the map: smaller, no frame, ink multiplied onto the water, slight tilt
- `6ba4268` 2026-09-10 — Ling: 1896 Oceanic Ichthyology plate (public domain), shown whole in a wide frame
- `54e64b9` 2026-09-10 — Species photos as local files in img/ (no runtime dependency on Commons)
- `a58ecd7` 2026-09-10 — Species photos from Wikimedia Commons for RJC, RJH, RJM, LIN (drawing kept for the SRX group); photos shown on the map in round frames with credits
- `d4641a1` 2026-09-10 — Species card: thornback ray photo (CC0, Ecomare Texel, Wikimedia Commons) with credit
- `b1f22f9` 2026-09-10 — Voyage: parent first in the north then south; fish drawn into the hull at each stop; hold counter on the boat
- `39474d5` 2026-09-10 — Map: members carrying the parent's code shown at the parent's level
- `7baa923` 2026-09-10 — Map: fish placed by member generation - parent alone in its own strip, species children in the shared water
- `8b5c154` 2026-09-10 — Species cards: species code only
- `86c0b91` 2026-09-10 — Map: each species only in its own water (parent in the strip no child covers); tooltips with exact names on fish and areas
- `98d113e` 2026-09-10 — Map: species drawings placed in each code's water; fish on the voyage catch cards
- `2e7be7b` 2026-09-10 — Species slide with original drawings (thornback, blonde, spotted ray, skates and rays, ling); thumbnails on the map notes
- `e925ffa` 2026-09-10 — Map: plain-language notes for each stock area code under the diagram
- `d99a9e5` 2026-09-10 — Map: FAO codes only on the divisions
- `5bb4c92` 2026-09-10 — Map: merged outlines per stock area, child inset once inside the parent frame, one label per code
- `5f592df` 2026-09-10 — Map: parent area as container, children inset inside or placed outside; nesting diagram in the panel
- `45fdc68` 2026-09-10 — Map: stock area codes on each area, coloured by generation (parent / child / grandchild)
- `68e7839` 2026-09-10 — Map slide: member -> stock area -> FAO area; only the family areas drawn, ICES and FAO codes on each
- `22f156c` 2026-09-10 — Voyage: the vessel sails through the family areas, catching under each member
- `6a5873a` 2026-09-10 — Map from decoded stock areas: 865 codes -> FAO/EEZ/RFMO/GSA; explorer slide for any stock area
- `23cf9e7` 2026-09-10 — Map slide: ICES divisions over Natural Earth coastline, family areas highlighted
- `00fd6a5` 2026-09-10 — Cover slide: the family boards the vessel (parent, four children, one grandchild)
- `7afa936` 2026-09-10 — Regulated-stock family presentation: NLD SRX/2AC4-C and FRA LIN/6X14., snapshots 12 Feb and 13 Aug 2026
