# Example Account and VAT Reference (anonymized)

This is an anonymized example of a company-specific account and VAT reference. It shows the format and the classification logic that `SKILL.md` and `workbook-rules.md` expect. All company names, person names, account numbers and VAT codes below are illustrative. Do not apply them to a real company unless the user says to adopt them. When the user attaches their own reference, use that one instead.

Use this together with `workbook-rules.md`. The current rules below supersede the older example observations. They implement an accountant's own process notes and an agreed hybrid VAT review. Historical rows are reference data, never additional transactions.

## Current operating rules

- Apply only to the same company/account or when explicitly adopted. Explicit current client/accountant instructions and the client's approved chart take priority. Obtain the company/own-account names and reliable aliases before matching own transfers; never invent identities from a filename. Max Muster / Muster Max is the named director in this example, not a director default for other clients.
- In this example the PayPal account is **1011**. This is the balance/payment account, not the counterpart Kto. Keep it as one editable setting.
- The AI must classify every record and assign each supported Kto. It must fill supported sales and technical/PayPal-fee VAT itself. For fresh ordinary expenses and supplier refunds, **leave operational VAT in Kontenrahmen and MwSt (manual) blank**, even when the historical tables show a code. The human fills VAT centrally or individually. Preserve previously supplied human edits when continuing the workbook.
- Resolve VAT per transaction: nonblank MwSt (manual) first, otherwise MwSt Kontenrahmen, otherwise blank/pending. Explicit N/A overrides the mapping; whitespace-only input is blank; ? is never usable. Use the final resolved value for all views, grouping, readiness and postings. Do not create exclusive supplier/transaction modes.
- Names in the source Name field belong in Gegenpartei; the editable working Name is the mapping's supplier/customer/group label. Preserve source values and original amounts. Keep raw transaction IDs as text, including leading zeros.

### Classification priority and approved example rules

Assess verified own-company/director exceptions and technical movements before ordinary payments. Use these configured account numbers only in the applicable project context.

| Event / evidence | Klassifikation / working Name | Kto | Initial operational MwSt |
| --- | --- | --- | --- |
| Verified own-company/own-account counterparty | Kontoübertrag | 1090 | N/A |
| Verified director Max Muster / Muster Max | Max Muster | 2119 | N/A |
| Allgemeine Währungsumrechnung / Währungsumrechnung durch Nutzer | Retain the appropriate conversion label in Klassifikation and working Name | 1093 if the approved chart has this separate account; otherwise 1090 | N/A |
| Guthabeneinbehaltung für Konfliktlösung; Einbehaltung für offene Autorisierung; Aufhebung der Guthabeneinbehaltung für Konfliktlösung; Von Nutzer eingeleitete Abbuchung; Rückbuchung allgemeiner Einbehaltung | Relevant technical label | 1090 | N/A |
| Actual standalone PayPal fee, e.g. Konfliktgebühr | Relevant fee label | 6941 | N/A |
| Named customer payment: positive Brutto and negative Entgelt, after applicable exceptions; CHF under this client's approved sales rule | PayPal Umsatz / Umsatz CH | 3200 | UN81 |
| Same confirmed sales pattern, any non-CHF currency under this client's approved sales rule | PayPal Umsatz / Umsatz Ausland | 3210 | UEX |
| Named customer Rückzahlung with negative Brutto, after exceptions/relationship checks; CHF | PayPal Umsatz / Umsatz CH / Rückzahlung | 3200 | UN81 |
| Same confirmed customer refund, non-CHF | PayPal Umsatz / Umsatz Ausland / Rückzahlung | 3210 | UEX |
| Named outgoing ordinary payment with negative Brutto, excluding own/director/technical exceptions | Aufwand / supplier name | Applicable supplier-purpose account from approved chart/history | Blank for human |
| Reliably matched positive supplier Rückzahlung | Aufwand / original supplier | Same supported account as the original expense | Blank for human; review consistently with original expense |
| Allgemeine Gutschrift auf Kreditkarte with reliable original-expense linkage | Aufwand / original supplier | Same supported account as the original expense | Blank for human |
| Bare Rückbuchung | Investigate source/related record; use original account/type only when relationship is reliable | Supported original account; otherwise blank and flag | Sales/technical rule if established, otherwise human/pending |
| Positive Handyzahlung or Allgemeine Zahlung without verified own/director exception | Unklar; request clarification | Blank pending clarification | Blank pending clarification |

The CHF/non-CHF split is an explicit working convention in this accountant's example, not independent evidence of geography or a tax rule for other companies. Positive amounts alone do not establish sales. If a refund or credit has several possible originals, flag it instead of linking by amount alone. Do not create fake source records for fee booking.

## What a real reference contains

A reference for a real company usually holds:

- The company's PayPal balance account.
- The company and director identities, with reliable aliases, for matching own transfers.
- The chart of accounts the company actually uses, with descriptions.
- The approved VAT codes for sales and for technical/fee items.
- Observed account and VAT assignments for counterparties and transaction types, taken from a processed workbook or the accountant's notes.

The tables below show these parts with invented data.

## Account inventory (example)

| Kto | Description / observed use | Role |
| --- | --- | --- |
| 1011 | PayPal | Payment/balance account setting, not a transaction Kto |
| 1021 | Bank CHF | Ledger-only account, not a general transaction Kto rule |
| 1090 | Transferkonto | Own transfers, holds/releases, technical movements |
| 1093 | Allgemeine Währungsumrechnung | Currency conversion |
| 1170 | Vorsteuer Material, Waren, DL - Kl. 4 | Ledger-only account, not a general transaction Kto rule |
| 1171 | Vorsteuer Inv., übr. Betriebsaufwand Kl.1 5-8 | Ledger-only account, not a general transaction Kto rule |
| 2119 | KK Max Muster | Director current account |
| 2200 | Geschuldete Mehrwertsteuer (Umsatzsteuer) | Ledger-only account, not a general transaction Kto rule |
| 3200 | Handelserlös | Sales CH |
| 3210 | Warenverkauf Ausland | Sales abroad |
| 4000 | Materialeinkauf Erzeugnis | Expense |
| 4060 | Fremdarbeiten | Expense |
| 4070 | Transportkosten | Expense |
| 4650 | Aufwand für Verpackungen | Expense |
| 5832 | Spesen, Reise, Auto, Übernachtung | Expense |
| 6500 | Büromaterial | Expense |
| 6510 | Kommunikationskosten | Expense |
| 6513 | Porti | Expense |
| 6559 | Sonstiger Verwaltungsaufwand | Expense |
| 6570 | Informatikaufwand | Expense |
| 6600 | Werbeinserate, elektronische Medien | Expense |
| 6601 | Marketing Aufwand | Expense |
| 6941 | Paypal / Payrexx Gebühren | Payment provider fees |
| 9100 | Saldovortrag | Ledger-only account, not a general transaction Kto rule |

## Example account / VAT observations

The rows below are invented combinations in the shape of a real reference. Use supported supplier-purpose **accounts** as evidence, subject to the current rules above. Do not prefill ordinary expense VAT from this table, including historical n/a. Current rules for director payments, holds/releases, sales and matched supplier refunds override conflicting historical entries. These rows are provenance, not transactions to import.

Columns: Klassifikation, working Name, Kto, MwSt. Rows with different source classifications are deliberately not merged. Historical exceptions and unresolved cases are kept on purpose.

| Klassifikation | Working Name / transaction context | Kto | MwSt |
| --- | --- | --- | --- |
| Kontoübertrag | Kontoübertrag | 1090 | n/a |
| Einbehaltung für offene Autorisierung | Einbehaltung für offene Autorisierung | 1090 | n/a |
| Aufhebung der Guthabeneinbehaltung für Konfliktlösung | Aufhebung der Guthabeneinbehaltung für Konfliktlösung | 1090 | n/a |
| Rückbuchung allgemeiner Einbehaltung | Rückbuchung allgemeiner Einbehaltung | 1090 | n/a |
| Von Nutzer eingeleitete Abbuchung | Von Nutzer eingeleitete Abbuchung | 1090 | n/a |
| Rückbuchung | Muster Handel GmbH | 1090 | n/a |
| Website-Zahlung | Beispiel Webshop GmbH | 1090 | n/a |
| Aufwand | Example Software Ltd | 1090 | n/a |
| Allgemeine Gutschrift auf Kreditkarte | Allgemeine Gutschrift auf Kreditkarte | 1093 | n/a |
| Allgemeine Währungsumrechnung | Allgemeine Währungsumrechnung | 1093 | n/a |
| Währungsumrechnung | Währungsumrechnung | 1093 | n/a |
| Währungsumrechnung durch Nutzer | Währungsumrechnung durch Nutzer | 1093 | n/a |
| Aufwand | Beispiel Akademie | 2119 | n/a |
| Max Muster | Max Muster | 2219 | n/a |
| Paypal Umsatz | Umsatz CH | 3200 | UN81 |
| Paypal Umsatz / Rückzahlung | Umsatz CH / Rückzahlung | 3200 | UN81 |
| Paypal Umsatz | Umsatz Ausland | 3210 | UEX |
| Paypal Umsatz / Rückzahlung | Umsatz Ausland / Rückzahlung | 3210 | UEX |
| Aufwand | Example Marketplace Ltd | 4000 | VIM |
| Rückzahlung | Example Marketplace Ltd | 4000 | VIM |
| Aufwand | Beispiel Baumarkt AG | 4000 | VM81 |
| Rückzahlung | Beispiel Webshop GmbH | 4000 | VM81 |
| Allgemeine Gutschrift auf Kreditkarte | Allgemeine Gutschrift auf Kreditkarte | 4060 | BZM81 |
| Aufwand | Sample Freelance Platform Inc. | 4060 | BZM81 |
| Allgemeine Gutschrift auf Kreditkarte | Sample Freelance Platform Inc. | 4060 | BZM81 |
| Aufwand | Muster Logistik GmbH | 4070 | n/a |
| Aufwand | Muster Verpackung GmbH | 4650 | n/a |
| Aufwand | Example Travel BV | 5832 | n/a |
| Aufwand | Beispiel Büroshop AG | 6500 | VB81 |
| Rückzahlung | Beispiel Büroshop AG | 6500 | VB81 |
| Aufwand | Muster Druck GmbH | 6500 | n/a |
| Aufwand | Muster Telekom AG | 6510 | VB81 |
| Aufwand | Beispiel Post GmbH | 6513 | n/a |
| Aufwand | Example Software Ltd | 6559 | n/a |
| Aufwand | Erika Beispiel | 6559 | n/a |
| Aufwand | Muster Beratung AG | 6559 | VB81 |
| Aufwand | Sample Streaming Inc. | 6570 | BZB81 |
| Allgemeine Gutschrift auf Kreditkarte | Sample Streaming Inc. | 6570 | BZB81 |
| Aufwand | Sample Cloud Corp. | 6570 | BZB81 |
| Rückzahlung | Sample Cloud Corp. | 6570 | BZB81 |
| Aufwand | Example TV AG | 6570 | n/a |
| Aufwand | Example Ads Inc. | 6600 | BZB81 |
| Aufwand | Muster Werbung GmbH | 6600 | VB81 |
| Rückzahlung | Muster Werbung GmbH | 6600 | VB81 |
| Aufwand | Sample Marketing LLC | 6601 | BZB81 |
| Aufwand | Erika Beispiel | 6601 | n/a |
| Konfliktgebühr | Konfliktgebühr | 6941 | n/a |
| Unklar | Beispiel Handel GmbH | ? | ? |
| Aufwand | Example Store SE | ? | ? |

## VAT codes used in this example

| Code | Treatment in this reference |
| --- | --- |
| ? | Unresolved: leave unsupported assignment blank and flag |
| BZB81 | Preserve exact source code; do not replace with an inferred percentage |
| BZM81 | Preserve exact source code; do not replace with an inferred percentage |
| UEX | Preserve exact source code; do not replace with an inferred percentage |
| UN81 | Preserve exact source code; do not replace with an inferred percentage |
| VB81 | Preserve exact source code; do not replace with an inferred percentage |
| VIM | Preserve exact source code; do not replace with an inferred percentage |
| VM81 | Preserve exact source code; do not replace with an inferred percentage |
| n/a | Preserve exact source code; do not replace with an inferred percentage |

## Exceptions and matching limits

- A historical alternative account for the director (here 2219) is superseded by the accountant's current instruction (2119). The historical 2119 assignment to a supplier ("Beispiel Akademie") does not make that supplier a director; flag it unless its account purpose is independently approved.
- Historic general card credits use several accounts. The current accountant workflow requires identifying a reliable original expense and using its applicable account; do not apply a blanket technical or supplier rule when the relationship is unclear.
- A historical supplier refund may sit on 1090 although the purchase was booked to an expense account such as 6559. Treat 1090 as historical evidence only; a reliably matched supplier refund follows the approved original expense rule. Do not silently treat all unknown refunds as 1090.
- Historical holds/releases and reversals sometimes use sales or suspense accounts. Apply the explicit current technical rules to known holds/releases; investigate bare Rückbuchung separately. Source IDs may repeat; maintain a separate internal row key.
- Several accounts have multiple VAT codes in the example. A human may set a shared expense mapping or override individual records. Never infer a universal VAT code from the account number or supplier name.
- A few counterparties in the example have unresolved accounts/VAT (shown as ?). Apply verified current own-company identity if supplied; otherwise preserve uncertainty rather than guessing.
- 1021, 1170, 1171, 2200 and 9100 are observed ledger accounts, not instructions to generate bank transfers, separate tax entries, or opening-balance records. Final gross postings use supplied VAT codes; never invent separate source tax-split transactions.
- The accountant's operational no-VAT code is N/A in these current example rules. Preserve the exact approved code in human inputs; historical spelling n/a remains visible in the observations table as provenance, not permission to auto-fill ordinary expense VAT.

## General chart of accounts (optional)

BDO-hosted Schweizer Kontenrahmen KMU, official school version:
https://www.bdo.ch/getmedia/39df73b6-5a1c-4d2e-97ef-f8a9f5350608/KMU-Kontenplan-Deutsch-PDF.pdf

Use it as general account-structure context or for clearly labelled proposals when no applicable company mapping exists. It does not define a company's custom accounts, PayPal setting, supplier assignments, or exact VAT codes. Do not override the company mappings with generic chart labels.
