# LedgerMerge

A local-first browser tool for consolidating UK bank statement exports into one Excel workbook. Owner: GOPESH V VYAS.

## Run

Open `index.html` in a browser, or serve this folder with any static web server. The workbook reader, PDF text extractor, and Excel writer are loaded from public CDNs, so those features require an internet connection. Statement contents are processed in the browser and are not sent to an application server.

## Deploy to Vercel

This is a static site; no build command, framework preset, environment variables, or server functions are required. Import the repository in Vercel with the project root set to this directory and the **Other** preset, leaving the build and output directory fields empty. Vercel serves `index.html` from the project root. The included `vercel.json` enables clean URLs and basic security headers.

Alternatively, deploy from this directory with the Vercel CLI using `npx vercel` and follow its prompts. The deployed page still needs internet access to load the public CDN libraries.

## Import and review

- CSV (comma, semicolon, tab, or pipe delimited) and Excel (`.xlsx`, `.xls`) files are supported. The first worksheet is read from Excel workbooks. A per-file mapping panel lets you select the header row and map unfamiliar columns; header recognition accepts common bank-specific suffixes and units.
- Transaction types use concise codes such as `DD`, `DT`, `CR`, and `AC`. For a generic signed `Amount` column, direction is inferred only from the sign or clear transaction wording; otherwise the value stays in `Unassigned` and is flagged instead of being counted as money in/out.
- Text-based PDFs use table-header and column-position detection, statement-period date inference, and continuation-line handling. If yearless dates have no statement year to use, the importer warns that it assigned the current year. Extracted PDF rows remain flagged for review, and their source text is retained in the original-row sheet.
- Where balances or statement summary totals are available, the app checks running-balance continuity and compares parsed money-in/out totals. Discrepancies are flagged for review.
- Scanned/image-only and password-protected PDFs are not supported. PDF layouts vary, so this parser cannot guarantee every bank's format; CSV/Excel exports remain more reliable when available.
- Common date, description, type, debit, credit, amount, and balance headings are detected. Bank formats differ, so detection can be incomplete or wrong.
- Correct cells directly in the ledger and remove rows that should not be included. Amber status means a row is incomplete or needs attention.
- Export includes a `Combined ledger` sheet and an `Original rows` sheet for source traceability.

No automated converter can guarantee 100% accuracy across every bank, account, export setting, and PDF layout. Verify the workbook against the source statements before using it for accounting or filing.