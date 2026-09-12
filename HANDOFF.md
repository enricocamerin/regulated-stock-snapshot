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
