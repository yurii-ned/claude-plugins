# PayPal Statement Accounting

A Claude plugin with one skill, `payment-statement-to-excel`. It turns PayPal and other payment-provider statements (CSV or PDF) into an Excel workbook for accounting.

## What it does

- Reads every row of the attached statements.
- Builds an `.xlsx` with live formulas: `Übersicht`, one sheet per currency, `Raw data`, `Kontenrahmen`, `Umsatz aggregiert`, `Rest GS`, `Rest BL`, `Buchungen aggregiert`.
- Classifies each transaction and maps it to an account (`Kto`) and a VAT code (`MwSt`).
- Leaves expense VAT blank for a human to fill in. It fills sales and fee VAT only where a mapping supports it.
- Runs its own checks (transaction counts, totals, formula consistency) before it hands over the file.
- Writes the summary in German, with Swiss spelling.

It does not connect to bexio or any other accounting system. `Buchungen aggregiert` is a journal you review and enter by hand.

## What's included

- `skills/payment-statement-to-excel/SKILL.md`: the workflow and the rules Claude follows.
- `references/workbook-rules.md`: worksheet structure, columns, formulas, formatting and the checks before delivery.
- `references/kontenrahmen-reference.md`: an anonymized example of an account/VAT reference. All names, accounts and VAT codes in it are invented. It shows the format and the logic.

## Use it for your company

The example reference is not your data. Attach your own account/VAT reference together with the statements, or edit `references/kontenrahmen-reference.md`:

- the PayPal account number,
- the company and director identities,
- your chart of accounts,
- your approved VAT codes.

The workflow and the workbook structure stay the same.

## Requirements

The skill tells Claude to load the `xlsx` skill before it builds the file. Make sure that skill is available where you run the plugin.

## Install

In Claude Code:

```
claude plugin marketplace add yurii-ned/claude-plugins
claude plugin install paypal-statement-accounting@yurii-ned-plugins
```

## Updates

```
claude plugin update paypal-statement-accounting@yurii-ned-plugins
```

You can also turn on auto-update for the marketplace in `/plugin`.
