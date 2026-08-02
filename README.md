# Adviser Merge Sheet

A self-contained, browser-based tool that combines Main Stats, Gross Calls, and Meaningful Calls into one sheet per adviser, then produces an anonymised version of that sheet for every individual adviser.

Everything runs locally in the browser (via SheetJS and JSZip, loaded from a CDN) — no files are uploaded to a server.

## How to use

1. Open `adviser-merge-tool.html` in a browser.
2. Enter an output file name — used as the prefix for every downloaded file.
3. Upload the three sheets (`.xlsx`, `.xls`, or `.csv` all accepted):
   - **Main Stats**
   - **Gross Calls**
   - **Meaningful Calls**
4. Once all three are recognised, the status line shows how many advisers were matched, plus any warnings.
5. **Download Master Sheet (XLSX)** — one combined workbook, all advisers.
6. **Download Adviser Files (ZIP)** — one workbook per adviser.
7. **Start over** — clears everything to run again.

## Required columns

### Main Stats
- An adviser name column — detected as any header containing "adviser" (prefers one that also contains "name").
- Recognised if present: `Submitted Value`, `Net Banked`, `Gross Converted`, `Net Converted` — used for currency formatting, sorting, and the Close At First Call calculation.
- `Banked (Business)` (case-insensitive, any sheet) is deleted from the output automatically.

### Gross Calls
- `Description` — used to match rows to advisers.
- `Out` and `In` — summed into a new `Gross Calls` column.
- `Tot Tlk` — carried across as `Total Talk Time`, using the cell's displayed value rather than its raw stored number (so an Excel duration like `12:32:08` survives intact rather than becoming a decimal fraction).

### Meaningful Calls
- `Description` — used to match rows to advisers.
- `Out` and `In` — summed into a new `Meaningful Calls` column.

## Name matching

- Case-insensitive and whitespace-tolerant.
- An initial matches a full first name with the same first letter (`J Smith` ~ `John Smith`).
- A built-in list of common English nicknames is treated as equivalent (`David`/`Dave`, `Robert`/`Bob`, etc.) — not exhaustive, so unusual or regional nicknames won't be caught.
- Matching only ever links entries across *different* sheets — two rows within the same sheet are never merged into one adviser.
- An adviser missing from a sheet still appears in the output, with blanks for that sheet's data.

## Calculated columns

| Column | Formula | Notes |
|---|---|---|
| Close At First Call | (Gross Converted − Net Converted) ÷ Gross Converted | Inserted directly after Net Converted. Blank if Gross Converted is 0 or either value is missing. Shown as a percentage. |
| Gross Calls | Out + In (Gross Calls sheet) | A missing Out or In counts as 0; an adviser entirely absent from the sheet is left blank. |
| Meaningful Calls | Out + In (Meaningful Calls sheet) | Same logic as above. |
| Total Talk Time | Tot Tlk (Gross Calls sheet) | Passed through as displayed, no calculation. |

## Formatting and sorting

- `Submitted Value` and `Net Banked` are stored as real numbers and displayed as UK currency (`£#,##0.00`).
- `Close At First Call` is stored as a real fraction and displayed as a percentage (`0.00%`).
- Columns are auto-sized to fit their widest content (character-count based, capped to a sensible max).
- All rows are sorted by `Net Banked`, highest first. If that column isn't found, rows keep upload order and a warning is shown.

## Outputs

- `<output name> Master Sheet.xlsx` — every adviser, one row each.
- `<output name> Adviser Sheets.zip` — one workbook per adviser, named `<output name> <adviser name>.xlsx`. Each contains every row and all data, but every *other* adviser's name is deleted outright from the underlying cell data (not hidden, styled white, or masked).

## Warnings

Shown in the status line without stopping the process, when:
- No `Net Banked` column is found in Main Stats (rows aren't sorted).
- `Close At First Call`, `Gross Calls`, `Meaningful Calls`, or `Total Talk Time` can't be computed because a required source column is missing.
- An uploaded file has no recognisable adviser/`Description` column — that file is rejected until it's fixed.

## Limitations

- Nickname matching is heuristic, not exhaustive.
- Column detection relies on header text (case-insensitive); unexpected header wording won't be recognised.
- Built for one row per adviser per sheet — not designed for multiple rows per adviser within the same sheet.
