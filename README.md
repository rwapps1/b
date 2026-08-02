# Report Builder

Upload a spreadsheet, apply a saved report, done.

**Live app:** https://rwapps1.github.io/tools1/

Report Builder has two modes for working with spreadsheet exports (CSV or Excel) — building repeatable, saved reports, and comparing two versions of a file to see exactly what changed.

## Build a report

Three steps: **Upload → Configure → Report**

Configuration options:

- **Filter rows** — one or more conditions, combined with AND/OR
- **Group by** — group by one or more columns, with a running count per group
- **Calculate** — aggregate numeric columns (sum, average, count, minimum, maximum) per group
- **Columns to include** — choose exactly which columns make it into the final report
- **Highlight duplicates** — flag rows where a chosen column repeats a value
- **Add columns** — append blank columns (e.g. "Notes") to fill in by hand after exporting
- **Sort** — sort by one or more columns, with tie-breaking levels (like Excel's Sort by / Then by)

**Presets** — save any configuration as a named, reusable preset. Once saved, applying it to a new file is a single click. Presets can be exported to a file (to share or back up) and imported back in.

## Compare files

Upload an **earlier** and a **later** version of the same type of file, and choose the column that uniquely identifies a row (e.g. a case reference or ID). The tool shows:

- **Added** — rows new in the later file
- **Removed** — rows present earlier but missing later
- **Changed** — same ID, different values, with the differences shown
- **Unchanged** — everything else

## Exporting

Reports can be exported as CSV or Excel (.xlsx).

## Privacy

Everything runs locally in your browser — files are never uploaded to a server. Saved presets are stored in your browser's local storage, so they're private to your device and browser, not shared or synced elsewhere.
