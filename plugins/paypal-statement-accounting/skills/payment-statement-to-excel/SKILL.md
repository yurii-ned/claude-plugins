---
name: payment-statement-to-excel
description: Converts PayPal and other payment-provider statements (PDF/CSV) into a ready-to-use accounting Excel workbook — currency sheets, a Kontenrahmen account/VAT mapping, a two-stage human VAT review, and monthly bexio posting blocks. Use when payment or PayPal statements are attached and need to become an accounting workbook, or when asked to process a PayPal/payment export into Excel.
---

# Payment Statement to Excel

Expert accounting processing for PayPal and other payment-provider statements. Turn attached statements into a ready-to-use `.xlsx` workbook an accounting team can use to review transactions, reconcile movements, and keep working.

**Priority order:** accuracy of source data > completeness of transactions > correctness of calculations/summaries > ease of use > visual formatting.

Always deliver an actual `.xlsx` file. Never substitute instructions, a proposed structure, or a preview for the real workbook.

## Before building anything

1. Read `references/kontenrahmen-reference.md` in full. It is an anonymized example of a company-specific account/VAT reference. It shows the format and the classification and VAT logic. Its names, account numbers and VAT codes are illustrative: apply them to a real company only if the user says to adopt them. If the user attaches their own account/VAT reference, that one takes priority. Current rules in the file always override historical/example entries in the same file.
2. Read `references/workbook-rules.md` in full. It holds the exact worksheet list, column definitions, formulas, VAT-resolution logic, and formatting the workbook must follow — do not improvise structure that isn't there.
3. Sort the attached files: original payment statements vs. already-processed example workbooks (reference for structure/formatting only — never copy their transactions or amounts) vs. the account/VAT reference doc. Identify company, account, statement period, currencies, and total transaction count. Process each company/account separately — one workbook per account.
4. When a PDF and a CSV cover the same statement, use the CSV as the primary source and the PDF only to verify — never duplicate transactions across them. Read every page of every PDF and every row of every spreadsheet/CSV.
5. If no original statements are attached, ask for them. Never generate fictitious transactions. If a source file can't be read completely, say so explicitly — never claim a file was fully processed when it wasn't, and never present a partial workbook as complete.

## Rules you must not violate

- Don't treat a repeated transaction ID as an automatic duplicate. Exclude a record only when full transaction details and statement context confirm it's the same transaction repeated across overlapping sources. When duplication is uncertain, keep the record and flag it.
- Classify every transaction and give it a `Zuordnung` entry in Kontenrahmen. A missing `Kto` or `MwSt` must never block classification, grouping, or exclude a transaction from a summary.
- For fresh imports, leave ordinary expense and supplier-refund VAT blank (both the Kontenrahmen mapping and the manual override) for a human to fill in — even when a historical example shows a code. Auto-fill only the supported sales VAT and technical/PayPal-fee VAT codes that the applicable account/VAT reference approves.
- When continuing a previously reviewed workbook, preserve existing human edits (manual VAT overrides, Kontenrahmen assignments) exactly — never clear them as a fresh import would.
- Preserve original signs, exact amounts, currencies, and text. Never recalculate or overwrite a source value.
- Every view and summary must be a live Excel formula (SUMIFS, INDEX/MATCH, SUBTOTAL, YEAR/MONTH, etc.), never a static snapshot computed once outside Excel. Default to widely-compatible functions; only use XLOOKUP/FILTER/LET if you've verified compatibility with the target Excel version.
- Never invent counterparties for technical transactions, and never link a refund to an original expense on amount/date similarity alone — require a reliably established relationship.
- Changing an account or VAT code in Kontenrahmen must ripple through every dependent view and summary via recalculation — never by rerunning this process or editing copies by hand.

## Workbook to produce

Build exactly the worksheets specified in `references/workbook-rules.md` — `Übersicht`, one sheet per currency actually present, `Raw data`, `Kontenrahmen`, `Umsatz aggregiert`, `Rest GS`, `Rest BL`, `Buchungen aggregiert` (only for ready periods), and `Fragen` only when needed. Do not add dashboards, charts, cover pages, instruction sheets, or per-counterparty sheets. Follow that file's exact column lists, column order, and formula requirements — they are not optional.

## Before delivering

Actually perform the validation checks in `references/workbook-rules.md` (transaction-count reconciliation, VAT-review test cases, independent total recalculation, formula-consistency checks, data-type checks) — don't report a check as passed without running it. Fix any formula errors before calling the workbook complete.

Then deliver the `.xlsx` file together with a concise response in German covering: company/payment account (if identifiable), statement period, total transactions processed, transaction counts per currency, currencies included, reconciliation status, any confirmed duplicate exclusions or incomplete inputs, outstanding questions, and the review stage (prepared for human expense-VAT review vs. ready for posting, including whether the recalculation test was actually verified).

Use German worksheet names and column headers, and Swiss spelling (`ss`, never `ß`).
