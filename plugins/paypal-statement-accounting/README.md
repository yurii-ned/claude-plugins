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

## Install

Pick one way. Most people should use the first.

### Claude chat, Cowork and the desktop app

1. Open **Customize > Plugins** on claude.ai or in the Claude desktop app.
2. Select **Add > Add marketplace**.
3. Enter `yurii-ned/claude-plugins`. The full URL `https://github.com/yurii-ned/claude-plugins` also works.
4. Find **Paypal statement accounting** and select **Add**.

The plugin is saved to your account. It works in chat and in Cowork right away. Claude Code picks it up the next time you start a session signed in to the same account. Run `/reload-plugins` to load it without a restart.

### Claude Code only

```
claude plugin marketplace add yurii-ned/claude-plugins
claude plugin install paypal-statement-accounting@yurii-ned-plugins
```

A plugin installed this way stays on that computer. It is not added to your claude.ai account, so it does not show up in chat or Cowork. If you want it everywhere, use the first way.

## How to use it

1. Start a new chat or Cowork task.
2. Attach the statements. CSV and PDF both work. If you have both for the same period, Claude uses the CSV and checks it against the PDF.
3. Optional: attach your own account/VAT reference (see [Use it for your company](#use-it-for-your-company)). Without one, Claude uses the standard Swiss chart of accounts (Kontenrahmen KMU) and marks the accounts as proposals. You can also attach an earlier workbook as a structure example.
4. Ask in plain words, for example: "Turn these PayPal statements into the accounting workbook."

Claude loads the skill when the task matches. To start it by hand:

- In chat or Cowork: type `/`, then `paypal-statement-accounting`, and select the skill.
- In Claude Code: type `/paypal-statement-accounting:payment-statement-to-excel`.

Claude gives you an `.xlsx` file and a short summary in German: company and account, period, number of transactions per currency, check results, and open questions.

### Review the workbook

The workbook is built in two stages.

1. **First stage.** Claude classifies every transaction and fills the accounts. Expense VAT is blank on purpose.
2. **Your review.** Open `Kontenrahmen` and check the accounts. Fill in the VAT code for expenses there. For a single exception, use `MwSt (manual)` in `Raw data`. A manual code overrides the mapped one.
3. **Final postings.** `Buchungen aggregiert` fills in for a period once all its transactions are ready. It recalculates inside Excel. You do not need to run Claude again.

If you come back to a reviewed workbook later, attach it again. Claude keeps your manual edits.

Check `Fragen` if it exists. It lists points that need an accounting decision.

## What's included

- `skills/payment-statement-to-excel/SKILL.md`: the workflow and the rules Claude follows.
- `references/workbook-rules.md`: worksheet structure, columns, formulas, formatting and the checks before delivery.
- `references/kontenrahmen-reference.md`: an anonymized example of an account/VAT reference. All names, accounts and VAT codes in it are invented. It shows the format and the logic.

## Use it for your company

Most users do not need this. Without your own reference, Claude works from the standard Swiss chart (Kontenrahmen KMU), proposes an account for each transaction and leaves VAT blank for you. It uses account 1011 as the PayPal account. You can change that in one place in the workbook. Claude does not ask questions as long as the statements are attached.

The example reference is not your data. For fixed accounts and approved VAT codes, attach your own account/VAT reference together with the statements, or edit `references/kontenrahmen-reference.md`:

- the PayPal account number,
- the company and director identities,
- your chart of accounts,
- your approved VAT codes.

The workflow and the workbook structure stay the same.

## Requirements

The skill tells Claude to load the `xlsx` skill before it builds the file. Make sure that skill is available where you run the plugin.

## Updates

- **Claude chat and Cowork.** New versions come from this repository. In **Customize > Plugins**, select **Check for updates** to pull the latest. For a marketplace you added from github.com, you can also turn on **Sync automatically**.
- **Claude Code.**

  ```
  claude plugin update paypal-statement-accounting@yurii-ned-plugins
  ```

  You can also turn on auto-update for the marketplace in `/plugin`.

## Notes

- On Team and Enterprise plans, an owner can limit which sources members may add. If you can't add the marketplace, ask your owner.
- Anthropic does not review plugins you add from a marketplace URL. Read what a plugin contains before you add it.
