# Workbook Structure and Rules

Full detail backing `SKILL.md`. Read this before building or editing the workbook.

## 1. Analyze All Input Files

Before creating the workbook:

1. Identify which files are original payment statements and which are examples of previously processed Excel outputs. Identify any attached account/VAT reference file (e.g. `kontenrahmen-reference.md` or a company-supplied `Kontenreferenz`) separately as a mapping reference, not as additional statement transactions.
2. Use example files as references for structure, simplicity, formatting, and applicable company-specific account/VAT mappings. Never include their transactions or amounts in the new workbook.
3. Identify the company, payment account, statement period, currencies, and total number of transactions.
4. Process different companies and payment accounts separately. Create a separate Excel workbook for each account.
5. Multiple statements belonging to the same account may be combined, provided overlapping periods and potential duplicates are checked.
6. When PDF and CSV files contain the same statement, use the CSV as the primary data source and the PDF for verification. Do not duplicate transactions.
7. Read every page of every PDF and every row of every spreadsheet or CSV file.
8. Preserve transaction descriptions and counterparty names that continue across multiple lines or pages.
9. Identify the applicable company profile: company/own-account names and verified aliases, director names/aliases, chart of accounts, sales VAT rules, and approved no-VAT code. Use current accountant instructions before historical examples. The bundled example rules apply only to their company/account or when explicitly adopted. If a necessary profile item is missing, continue extraction and flag affected assignments; do not guess ownership or tax treatment.

Do not assume that transactions are duplicates merely because their IDs are identical. Exclude a repeated record only when the complete transaction details and statement context confirm that it is the same transaction repeated across overlapping source files. Preserve genuine separate records, including technical movements with repeated IDs. If duplication remains uncertain, retain and flag the records. Briefly report confirmed exclusions in the final response.

If no original statements are attached, request the source files. Never generate fictitious transactions.

If a source or reference file cannot be read completely, identify the limitation explicitly. Never claim to have analyzed an unreadable file or present a partial workbook as complete. If essential source data cannot be processed, explain what is missing before delivering a final result.

## 2. Excel Workbook Structure

Create a minimal, practical workbook containing:

- `Übersicht` — compact financial summaries.
- `CHF`, `EUR`, `USD`, etc. — one worksheet for each currency actually present in the source data.
- `Raw data` — complete transaction register with source fields and working columns.
- `Kontenrahmen` — editable category/account/VAT mapping table.
- `Umsatz aggregiert` — monthly sales and customer-refund summaries.
- `Rest GS` — remaining credit transactions.
- `Rest BL` — remaining debit transactions.
- `Buchungen aggregiert` — monthly debit/credit postings for manual transfer to bexio, populated only for periods whose transactions are ready.
- `Fragen` — only when unresolved issues cannot be explained adequately in short transaction comments.

Do not create:

- Cover pages
- Dashboards or charts
- Instruction sheets
- Technical processing logs
- Separate worksheets for individual counterparties
- Duplicate copies of the currency worksheets
- Unnecessary accounting schedules

If the user explicitly requests an existing template, prioritize its specified structure. However, do not automatically copy unrelated accounting fields, customer account numbers, or tax codes from the example.

Use `Raw data` as the single complete transaction register: retain all selected source transactions and original fields, and add the working columns below. Preserve original values and meanings; normalize dates/numbers only as required for Excel. Retain every original CSV/spreadsheet column in `Raw data`, including time, timezone, email, invoice references, source tax amounts, and bank fields, even when omitted from compact views. The source `Name` may be stored as `Gegenpartei` as defined below. Exclude only confirmed duplicate representations under Section 1.

Currency sheets and `Rest GS`/`Rest BL` must be linked views of this register; summaries must reference it. Do not maintain independent editable copies. Link each view row to its actual source row or stable internal record key. Do not copy a register formula into another sheet if it references a column that is absent from that destination. Accountants edit transaction assignments or source corrections in `Raw data`, and mappings in `Kontenrahmen`. Keep routine views compact even when `Raw data` needs more source columns.

### Human Review and Final Postings

Use one workbook with two stages, not two exclusive VAT modes:

1. Prepare all source data, classifications, supported Kto assignments, currency views, and financial summaries. Assign supported sales VAT and approved N/A codes for technical movements/PayPal fees automatically. For ordinary expenses and supplier refunds, initially leave both the operational Kontenrahmen VAT and manual transaction VAT blank, even when a historical example contains a VAT code. The accountant's assistant is a human reviewer.
2. Let the human fill expense VAT centrally in Kontenrahmen, individually in Raw data, or both. A nonblank manual transaction code overrides the mapped code. Preserve existing human entries when resuming a reviewed workbook; never clear them as if starting a fresh import.
3. Use the resolved VAT and current Kto to produce the final monthly postings. Do this inside Excel without rerunning the AI. A review-ready workbook with intentional expense-VAT blanks is a valid first-stage deliverable, but must not be described as ready for posting.

Keep editable VAT fields only in Kontenrahmen and Raw data. Other views show one resolved MwSt column. Do not create a mode selector, extra review worksheet, or technical dashboard.

## 3. Currency Worksheets

Use the following primary columns in this exact order:

`Datum | Jahr | Monat | Klassifikation | Name | Beschreibung | Kto | MwSt | Währung | Brutto | Entgelt | Netto | Gegenpartei | Kommentar | Transaktionscode`

Use the same field meanings in `Raw data` and all transaction views. In `Raw data` only, replace the one visible `MwSt` column with `MwSt Kontenrahmen | MwSt (manual)`. Keep the resolved `MwSt` as a helper field at the far right, hidden by default if practical. Currency and residual views retain the single resolved `MwSt` in the primary column order above.

### Column Definitions

**Datum**

The actual transaction date, stored as a native Excel date and displayed as `DD.MM.YYYY`. If the date cannot be established reliably, leave this cell blank, retain the original date text in `Kommentar` or `Fragen`, and keep the transaction in the currency worksheet.

**Jahr and Monat**

Separate numeric columns derived from `Datum` using formulas: year such as 2026 and month 1–12. Do not replace them with a combined date label. Leave them blank when the date is unresolved; retain the original date text and include the amounts in the unresolved-date summary group.

**Klassifikation**

The consistent working category assigned through `Kontenrahmen`.

**Name**

The working/accounting group label from `Kontenrahmen`, such as a sales or expense group.

**Gegenpartei**

The original counterparty name, but only when reliably identifiable. If the source calls this field `Name`, preserve its content here. Do not create two ambiguous columns both titled `Name`, or invent counterparties for technical transactions.

**Beschreibung**

The transaction type or description, preserving important details from the original statement.

**Kto and MwSt**

Obtain Kto by formula from Kontenrahmen. In Raw data, `MwSt Kontenrahmen` looks up its mapping's exact VAT code, `MwSt (manual)` is a genuinely editable human input, and helper `MwSt` resolves the final code. Treat whitespace-only input as blank: use the manual code if nonblank, otherwise the mapped code, otherwise leave the result blank. An explicitly entered approved `N/A` (or other approved no-VAT code) is a value and overrides the mapping. Clearing the manual input restores the mapping. Reject/flag `?` and unsupported codes; never convert them into a no-VAT decision.

For fresh statements, leave manual inputs blank for all transactions. Do not seed ordinary expense/supplier-refund VAT from historical examples into either operational VAT field. The human may fill the shared mapping and use manual entries for exceptions. Preserve both kinds of human edits when continuing an existing workbook. Store Kto and VAT codes as text; do not infer percentages or confuse VAT with PayPal fees. Preserve source Umsatzsteuer separately. Changing VAT must never alter original Brutto, Entgelt, or Netto.

**Währung**

The original transaction currency.

**Brutto, Entgelt, Netto**

Store the original values as numeric Excel cells, preserving:

- Original currency
- Original sign
- Exact source amount
- Two-decimal display format

Do not independently recalculate or overwrite source values.

**Kommentar**

Use a short, understandable transaction category, optionally followed by an explanation.

Standard categories:

- `Zahlungseingang`
- `Zahlungsausgang`
- `Rückzahlung`
- `Währungsumrechnung`
- `Transfer`
- `Einbehaltung`
- `Freigabe`
- `Unklar`

Always place the standardized category at the beginning of the comment.

If additional clarification is necessary, separate it with a dash.

This convention must support reliable category aggregation using Excel formulas.

**Transaktionscode**

Preserve the original transaction ID as text, including leading zeros.

### Additional Columns

If the source contains:

- `Guthaben`
- `Zugehöriger Transaktionscode`

Preserve these as additional columns at the end of the worksheet.

These fields are important for balance verification and identifying related transactions.

Add other fields only when they provide a clear practical benefit.

Omit empty address fields, bank details, duplicated names, or irrelevant technical metadata from compact transaction views unless useful. This restriction does not apply to `Raw data`, which must retain all original source columns.

### Transaction Processing Rules

1. Each retained source transaction record must correspond to exactly one row in the appropriate currency worksheet. Exclude only confirmed repeated representations of the same transaction as described in Section 1.
2. Text wrapping or multi-line descriptions in PDF files must not create additional transactions.
3. Sort transactions chronologically.
4. Within the same date, preserve transaction time or the original statement order.
5. Preserve time information when required to distinguish transactions.
6. Never invent counterparties for technical transactions.
7. Copy counterparty information from a related transaction only when the relationship is reliably established.
8. Similar dates or amounts alone are insufficient to prove a relationship.
9. Do not remove currency conversions, holds, releases, transfers, reversals, or other technical entries.
10. Do not create separate fee transactions when the fee is already included in `Entgelt` and no corresponding standalone transaction exists in the source.

### Kontenrahmen

Create one editable table:

`Zuordnung | Klassifikation | Name | Kto | MwSt | Bereich | Hinweis`

`Zuordnung` is a stable unique mapping key. `Bereich` is `Umsatz` or `Rest`. In `Raw data`, add a record-level `Zuordnung` field linking each transaction to this table; accountants must be able to change its assignment. Keep helper keys at the far right. Use a separate internal row key/source locator where necessary: transaction IDs can repeat.

Use exact-match `INDEX/MATCH` lookups with ordinary A1-style cell/range references by default for `Klassifikation`, `Name`, `Kto`, `MwSt`, and `Bereich`. Use newer functions such as `XLOOKUP` only when compatibility with the target Excel version and correct XLSX serialization have been verified. A single generic `Aufwand` rule is insufficient when suppliers or purposes require different accounts/VAT codes; create separate applicable rules without creating one rule per transaction unnecessarily.

Use supplied company-specific account mappings and current accountant rules (see `kontenrahmen-reference.md`). Apply supported Kto yourself. Approved current rules override explicitly historical observation tables. Historical expense VAT codes are examples, not permission for the AI to prefill expense VAT. For fresh imports, keep operational expense/supplier-refund MwSt cells in Kontenrahmen blank for the human, while populating their classification, Name and supported Kto. Fill supported sales and technical/fee VAT automatically. Keep unknown account/VAT decisions flagged; do not invent generic accounts or 0% codes.

### Apply the Account Reference

- Read `kontenrahmen-reference.md` and apply applicable approved rules to Kontenrahmen and transactions, rather than providing an unused reference or blank classification template.
- Match supplier, event type, purpose and reliable related evidence. Separate mappings when one supplier has materially different transaction types/accounts. Human VAT exceptions may use the manual transaction field without duplicating the mapping.
- Apply explicit user instructions and the current company's approved profile first. Do not transfer another company's accounts, director identity, or tax policy silently.
- The PayPal payment/balance account number is defined in `kontenrahmen-reference.md`, distinct from transaction-level Kto. Keep it as one editable setting or compact note. Changing it must update the final posting accounts.
- Flag unsupported/conflicting assignments in Hinweis and concise transaction comments. A missing ordinary expense VAT before human review is an expected pending field, not a failed extraction or an instruction to default to N/A.

Changing Kto or MwSt once in this table must update all affected records, views, and summaries through Excel recalculation, without rerunning the AI or editing copies. Group using stable assignments and the resolved transaction MwSt; show current labels/accounts/codes. Do not hardcode previous account numbers, VAT codes, or group names as criteria. VAT-dependent group rows must also update when a manual code creates a new group, not only recalculate amounts in an old fixed list. Mapping keys must remain unique even when several rules use the same account.

### Complete the Initial Classification

Perform the initial classification yourself before delivering the workbook:

- Populate `Zuordnung` for every retained transaction and create the corresponding real entries in `Kontenrahmen`. Do not provide demonstration entries or a blank assignment template.
- Populate `Klassifikation`, `Name`, and `Bereich` from supported transaction evidence and applicable supplied mappings. Use meaningful working group labels without inventing a business purpose, country, or tax treatment.
- Distinguish transaction classification from account/VAT assignment. Missing `Kto` or `MwSt` must not block classification, grouping, or summaries. If no company mapping was supplied, complete the remaining processing and disclose the missing mapping.
- For genuinely uncertain classifications, use an explicit `Unklar` mapping with `Bereich = Rest` and a short explanation. Apply it only after assessing the available evidence, not as a blanket fallback for recognizable transactions.
- In `Raw data`, populate `Bereich` by lookup as a compact helper field at the far right, alongside `Zuordnung`. All assigned keys must exist exactly once in `Kontenrahmen`.
- Use applicable supplied Kto and exact approved sales/technical VAT codes. Keep fresh ordinary expense and supplier-refund VAT blank for the human, regardless of historical codes. Do not guess a VAT rate or default to 0%/N/A. Preserve later human inputs and apply the resolution rule above.

### Umsatz aggregiert and Rest GS / Rest BL

First identify customer sales and their refunds using the source and applicable mappings. A positive amount alone does not prove sales. Customer refunds remain in the sales population with their original negative signs. Supplier refunds are not sales. Flag uncertain cases and retain them in `Rest` pending clarification.

Create `Umsatz aggregiert` with:

`Jahr | Monat | Währung | Name | Kto | MwSt | Brutto | Entgelt | Netto`

Summarize only sales and customer refunds by year/month, currency, and relevant assignment group. Keep refunds separately identifiable. Use formulas linked to `Raw data`, current Kto and the resolved transaction MwSt, including manual overrides. Prefer a narrow table; do not combine currencies or different years in one monetary total.

After excluding the sales/customer-refund population:

- `Rest GS` (Gutschriften): records with positive signed net movement.
- `Rest BL` (Belastungen): records with negative signed net movement.

Use `Netto` for direction when it represents the signed account movement; otherwise use the provider's documented balance-impact convention. Preserve source signs. Keep technical conversion legs, transfers, holds/releases, and supplier refunds in the appropriate residual view. Do not also include customer refunds in `Rest BL`.

Zero or unreadable residual movements remain in `Raw data` and currency views and must be explicitly accounted for as zero/unresolved residual records. They must not disappear or be forced into a positive/negative group.

The non-overlapping partition is: sales/customer refunds + Rest GS + Rest BL + explicitly accounted-for zero/unresolved residual records = all retained transactions. Currency views are an additional complete view, not another population to add to this total. Unresolved Kto/MwSt must not exclude an otherwise valid transaction.

Fully populate the financial summaries and residual views during processing, including transactions pending expense VAT. They remain useful for review; only final postings are gated by readiness. Do not replace them with headers, instructions to fill `Zuordnung`, or promises that results will appear later. Each summary with eligible records must already have its grouping rows, linked labels, and aggregation formulas. Each residual view must already contain its eligible transactions. If a population is genuinely empty, add a short German note stating that no applicable transactions were found; do not fabricate rows.

Do not add separate `Umsatz TX`, `Rest GS Bexio`, or `Rest BL Bexio` sheets unless requested.

### Buchungen aggregiert — Final Monthly Postings

Follow the three booking layouts the accountant has specified (e.g. in their own Notizen). This worksheet is a reviewable journal for manual entry in bexio; do not connect to bexio or claim an import format has been validated.

Create three compact blocks. Incoming/outgoing blocks use exactly:

`Monat | Sollkonto | Habenkonto | Beschreibung | MWST | Bruttobetrag | Währung`

The fee block uses the same columns except `Betrag (Entgelt)` replaces Bruttobetrag. Represent Monat as a real month date displayed MM.YYYY so years stay distinct without an extra column. Beschreibung is the current working Name (the accountant's highlighted Name), not arbitrary raw payment text.

- **Positive Brutto:** Sollkonto = PayPal account; Habenkonto = current Kto; amount = sum of positive source Brutto.
- **Negative Brutto:** Sollkonto = current Kto; Habenkonto = PayPal account; amount = absolute sum of negative source Brutto. Customer refunds therefore reverse the sales account rather than being treated as ordinary expenses.
- **Entgelt:** aggregate signed source Entgelt by year/month and currency. For the usual negative net fee total, Sollkonto = the fee account from `kontenrahmen-reference.md`, Habenkonto = PayPal account, Beschreibung = PayPal-Gebühren, MWST = the approved N/A code, amount = absolute fee total. Positive net returned fees reverse those two accounts; zero fee totals need no posting. Do not discard returned fees or change the original source sign.

Aggregate gross blocks separately by year/month, current working Name, current Kto, **resolved MwSt**, currency and direction. Do not net unrelated incoming and outgoing gross transactions into one line. Use Brutto for gross postings and Entgelt for fee postings; never book Netto as a gross amount and then deduct fees again. Retain real standalone fee records with their own Brutto once, and include only actual source Entgelt in the fee block; never invent extra source transactions.

**Readiness:** For each selected year/month/currency, nonzero gross transactions need a resolved classification/assignment, working Name, Kto and final MwSt; dates/currency must be usable and material classification/account conflicts resolved. Check final transaction values, not every mapping row: a blank mapping VAT is acceptable when all affected transactions have approved manual codes. Zero-gross records remain accounted for; their nonzero fees still contribute to the fee block.

If a period has pending records, show a short German pending count/status and do not present partial postings for that period as final. Keep all transactions in Raw data, currency views and review summaries. Populate that period's final three blocks automatically once its pending fields/conflicts are resolved. For genuine unresolved classification/account/link conflicts, use one compact editable `Klärung offen` flag in Raw data, set only for affected records and cleared by the human after resolution. Do not mark intentional expense-VAT blanks as an additional classification conflict. Do not silently exclude pending records and report a reconciled journal. A whole workbook is posting-ready only when every selected period is ready.

Build genuinely updating group membership/keys using compatible Excel formulas/helpers or another explicitly agreed refresh method. A static list of groups or populated rows that requires rerunning the AI after a VAT edit does not meet the default requirement. Keep helpers at the far right/hidden; do not expose processing machinery in the final journal.

Reconcile by currency and year/month: the signed movement on the PayPal account from gross plus fee postings must equal the source sum of Netto under the verified source fee-sign convention. Distinguish the pending review state from a verified final journal. Never add an extra fee row to Raw data or sum monetary amounts across currencies.

## 4. Übersicht — Summary Worksheet

Create exactly two primary summary tables.

### Table 1: Monthly Summary

Columns:

`Jahr | Monat | Währung | Kategorie | Brutto | Entgelt | Netto`

Group transactions by:

- Year and month
- Currency
- Transaction category

Include all transaction types, including technical transactions.

Include transactions whose year/month cannot be determined in a clearly labeled `Datum unklar` group. Group these rows by currency and category as usual. A missing date must never cause a transaction or its amounts to disappear from the summary.

### Table 2: Counterparty Summary

Columns:

`Gegenpartei | Währung | Brutto | Entgelt | Netto`

Include incoming payments, outgoing payments, and refunds associated with reliably identified counterparties.

Exclude technical conversions, holds, releases, and internal transfers from this counterparty summary.

These technical movements must remain visible in the category-based monthly summary.

### Formula Requirements

1. Use transparent Excel formulas such as `SUMIFS` and `SUM`, referencing the detailed records in `Raw data`.
2. Summary amounts must recalculate when transaction amounts are edited.
3. Use `Klassifikation` for category grouping; additional explanatory text in `Kommentar` must not change the grouping.
4. If a helper category field is required for reliable aggregation, keep the user-facing structure simple and avoid unnecessary duplication.
5. Use `SUBTOTAL` for totals of visible or filtered transaction rows on currency worksheets.
6. Keep subtotal rows outside the detailed transaction data range.
7. Prevent subtotal rows from being counted again in the summary calculations.
8. Never combine amounts from different currencies into one monetary total.
9. Never convert currencies unless separately instructed.
10. Never label the total movement of all transactions as `Umsatz`, profit, or earnings.
11. Treat `Guthaben` as a balance, not as transaction turnover. Never sum balances as financial movements.

### Formula Compatibility and References

- Ensure every referenced worksheet, table, column, defined name, and lookup key actually exists. A worksheet name alone does not create an Excel table or defined name.
- In linked views, reference the relevant `Raw data` cells. Do not use a local `Zuordnung` reference where that column is absent. Structured references, if used, must have valid Excel syntax and belong to the actual table.
- Default to established functions such as `INDEX`, `MATCH`, `IF`, `SUMIFS`, `SUM`, `SUBTOTAL`, `YEAR`, and `MONTH`. Do not depend on `XLOOKUP`, `FILTER`, `LET`, or other newer functions without verified compatibility. Do not write malformed or unsupported function names into XLSX files.
- `Übersicht` and `Umsatz aggregiert` must contain working aggregation formulas, not numbers computed once outside Excel. Mapping-dependent labels and codes must remain linked as well. Do not replace the required live behavior with a static snapshot.
- Preserve genuinely empty source text fields as blank in linked views; a direct reference to a blank cell must not display `0`. Likewise, a valid mapping with an empty `Kto` or `MwSt` must remain blank and flagged. Preserve actual numeric zeros in amount columns.
- An empty formula cache, a successful file export, or enabling recalculation on opening is not evidence that formulas work. Do not hide broken functions, names, or references behind blanket `IFERROR(...,0)` or blank results. Handle genuinely missing mappings separately from formula defects.

## 5. Data Accuracy and Ambiguous Cases

### Numeric Values

Determine the correct decimal and thousands separators based on the source document.

Store financial amounts as real Excel numbers, not strings.

Display amounts using two decimal places.

Do not insert currency symbols, currency codes, apostrophes, or other text into numeric cell values.

Interpret symbols such as `-` as zero only when the statement format clearly uses them to represent a zero amount. Distinguish these placeholders from genuinely missing or unreadable values.

Do not automatically interpret missing, unreadable, or unknown values as zero.

Preserve the original signs exactly as shown in the statement.

### Dates and Identifiers

Verify date formats using the document context.

Be particularly careful with ambiguous or mixed formats such as:

- `DD-MM-YY`
- `MM-DD-YY`

If a date cannot be determined reliably, do not guess.

Preserve the original date representation in the transaction comment or issue record and flag it for verification.

Store transaction IDs, account numbers, and document references as text.

Preserve leading zeros.

Do not invent missing digits or reconstruct damaged identifiers without reliable evidence.

### Transaction Classification

Keep distinct transaction types separate.

Do not confuse:

- Payments
- Refunds
- Transaction fees
- Currency conversions
- Internal transfers
- Holds and releases
- Reversals
- Other technical movements

Do not independently infer country, tax treatment, or purpose from names/currency. A supplied accountant-approved company rule may explicitly use currency as a working sales allocation (e.g. domestic currency to domestic sales, all other currencies to foreign sales — see `kontenrahmen-reference.md`); apply it only within that approved scope, not as a general tax rule.

Only specify a confirmed conversion direction, such as `CHF → EUR`, when supported by the underlying data.

Otherwise, use the general classification `Währungsumrechnung`.

Apply current company classification rules before generic payment rules: own-company/director identities, conversions and holds/releases take priority over ordinary sales/expenses. Follow the classification-priority table in `kontenrahmen-reference.md` for named payments, refunds, and ambiguous cases (e.g. bare `Rückbuchung`, unexplained `Handyzahlung`/`Allgemeine Zahlung`).

Include Kto/MwSt as defined above, leaving unsupported accounts and intentionally pending expense VAT flagged. Sollkonto/Habenkonto belong to Buchungen aggregiert; do not add extra debit/credit columns to ordinary transaction views.

Never transfer company-specific tax codes or chart-of-accounts mappings from an example belonging to another company.

### Unresolved Transactions

When a transaction cannot be classified reliably, use:

`Unklar`

Add a brief explanation where possible.

If an issue requires a separate accounting decision, include it in the `Fragen` worksheet with:

- Currency
- Transaction date or original date text
- Transaction ID
- Short issue description
- Required clarification

Do not modify source amounts merely to eliminate reconciliation differences.

## 6. Formatting Requirements

Use German worksheet names and column headers.

Follow Swiss German spelling conventions using `ss`.

Preserve original counterparty names exactly.

The workbook must have a clean, practical appearance:

- White background
- One consistent standard font
- Subtle, clearly distinguishable headers
- Appropriate column widths: date columns must display DD.MM.YYYY fully (normally at least 12 characters wide); keep short Kto/MwSt fields compact and give descriptions enough width. Verify that dates and amounts do not display as ####
- Wrapped text for longer descriptions
- AutoFilter enabled
- Frozen header rows
- No merged cells within tables
- No unnecessary blank rows
- No decorative panels
- No worksheet protection

Highlight only totals and genuinely questionable values.

Keep the workbook fully editable.

## 7. Mandatory Validation Before Delivery

Perform actual data checks before delivering the workbook.

### Transaction Counts

Verify that the number of exported transaction rows matches the retained source transaction records after selecting the primary source and excluding only confirmed duplicate representations:

- Overall
- Separately for each currency

Account for every source record: retain it, identify its equivalent in the selected primary source, or explain its confirmed duplicate exclusion. Count unresolved records as retained transactions. Briefly report any exclusions; do not create a separate processing-log worksheet.

Where IDs are available, compare the source and output records at ID, currency, and amount level, preserving multiplicity for repeated IDs; counts alone are insufficient. In PDFs, pay particular attention to descriptions/names wrapped onto a second line or page. If a source transaction is missing, revisit the extraction and recover it before delivering a complete workbook. Never treat an unexplained missing transaction as an acceptable duplicate exclusion.

### Processing Completeness

Before delivery, verify that every transaction has a valid `Zuordnung` and populated `Klassifikation`, `Name`, and `Bereich`; explain genuinely uncertain cases. Check all original source columns are retained in `Raw data`. Confirm that `Kontenrahmen` contains actual applicable rules, all eligible summary and residual populations are populated, and summaries contain aggregation formulas. These checks are required even when formula recalculation is unavailable. Missing account/VAT mappings do not excuse incomplete processing.

### VAT Review and Posting Controls

Verify initial expense VAT is blank in both operational mapping and manual inputs; no historical code was silently seeded. Verify automatic sales/technical VAT follows the approved profile. Test: mapping-only code, manual override, clearing the manual field, an explicit manual N/A, and both fields blank. A manual override must remain unchanged after editing the mapping; unaffected transactions must follow the new mapping. Confirm new VAT groups appear and pending records block their own final period. Test one gross incoming, outgoing, customer refund, supplier refund, negative fee and positive fee return. Reconcile the final PayPal-account movement with source Netto for each ready period/currency. Restore any trial edits. Do not fabricate manual review completion.

### Financial Values

Verify that all original amounts, signs, and currencies are preserved.

Independently calculate `Brutto`, `Entgelt`, and `Netto` totals from the retained source transaction records for each currency and compare them with the exported workbook. This check is required even when the statement does not print its own totals. Do not use the exported worksheets themselves as the only source of the control totals.

Also compare against statement-provided totals when available, accounting for their period and transaction scope. If a field is missing or unreadable, disclose the limitation rather than treating its total as verified.

### Formula Consistency

Verify that `Raw data`, currency views, `Übersicht`, and `Umsatz aggregiert` agree for their respective populations. Verify that the sales/residual partition has no overlaps or omissions; do not add complete views and subsets together.

Check unique mapping keys and exact-match formulas. Temporarily change one representative Kto and one MwSt value in `Kontenrahmen`, recalculate, and verify that all applicable views and summaries update. Also test a representative amount edit in `Raw data`. Restore the test values before delivery.

Use the target Excel application or a compatible calculation engine for this check when available, and inspect the resulting values for formula errors including `#NAME?`, `#REF!`, and unexpected `#N/A`. If recalculation cannot be performed, explicitly report formula behavior as unverified; do not claim the edit-and-update test passed. Fix any known formula/reference errors before presenting the workbook as complete.

Ensure technical transactions and fees are not counted twice.

Check that:

`Brutto + Entgelt = Netto`

only when the source statement uses this fee-sign convention.

Do not assume the same convention applies to all providers or statements.

### Data Types

Verify that:

- Financial amounts are numeric
- Valid dates are real Excel dates
- Jahr and Monat are separate numeric fields for known dates
- Transaction IDs, Kto, and MwSt codes are text
- Leading zeros are preserved
- Formulas contain no errors
- No source transactions have been unintentionally omitted
- Repeated IDs have not been removed without sufficient justification
- Transactions with unresolved dates remain included in the detailed data and summary amounts

### Balance Reconciliation

Only claim complete balance reconciliation when the statement provides independently documented opening and closing balances for the same currency and period, together with complete relevant movements. Verify opening balance plus net balance-affecting movements against closing balance. A balance inferred from the same transactions is not an independent reconciliation control.

If this information is missing, clearly distinguish transaction-total verification from full balance reconciliation.

When discrepancies remain, preserve the original amounts and briefly document the issue.

Never force totals to match by adjusting transaction values.

## 8. Final Deliverable

Deliver a completed, downloadable `.xlsx` workbook.

Do not replace the file with a written explanation, workbook preview, or implementation plan.

Provide a concise accompanying response in German stating:

- Company or payment account, if identifiable
- Statement period
- Total number of processed transactions
- Transaction counts per currency
- Currencies included
- Reconciliation status
- Confirmed duplicate exclusions or incomplete inputs, if any
- Outstanding questions, if any
- Review stage: prepared for human expense-VAT review or ready for posting; pending transaction/period counts and whether final posting recalculation was actually verified

### Success Criteria

The workbook is successful when an accountant can:

1. Open the file immediately.
2. Filter transactions by currency and date.
3. Verify each transaction against its original ID when available, with missing or damaged identifiers clearly flagged.
4. Review accurate monthly and counterparty summaries.
5. Identify unresolved or ambiguous entries.
6. Modify transaction amounts and obtain automatically recalculated summaries.
7. Continue accounting work without manually restructuring the data.
8. Change a shared account/VAT mapping and see affected records update while manual VAT exceptions remain intact.
9. Review and enter expense VAT centrally or per transaction, without rerunning the AI.
10. Obtain the three monthly booking blocks after review, with source-to-journal checks and no mixed-currency totals.

**Final instruction:** Do not claim that a validation or reconciliation has been completed unless it has actually been performed. Do not fabricate missing source information. Accuracy and completeness always take priority over visual appearance.
