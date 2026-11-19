# yurii-ned-plugins

Claude plugins by Yurii Nedashkivskyi.

## Plugins

| Plugin | What it does |
| --- | --- |
| [paypal-statement-accounting](plugins/paypal-statement-accounting) | PayPal and payment statements to an accounting-ready Excel workbook |

Each plugin has its own README with details on how to use it.

## Install

Pick one way. Most people should use the first.

### Claude chat, Cowork and the desktop app

1. Open **Customize > Plugins** on claude.ai or in the Claude desktop app.
2. Select **Add > Add marketplace**.
3. Enter `yurii-ned/claude-plugins`. The full URL `https://github.com/yurii-ned/claude-plugins` also works.
4. Find the plugin you want and select **Add**.

The plugin is saved to your account. It works in chat and in Cowork right away. Claude Code picks it up the next time you start a session signed in to the same account. Run `/reload-plugins` to load it without a restart.

### Claude Code only

```
claude plugin marketplace add yurii-ned/claude-plugins
claude plugin install <name>@yurii-ned-plugins
```

A plugin installed this way stays on that computer. It is not added to your claude.ai account, so it does not show up in chat or Cowork. If you want it everywhere, use the first way.

## Updates

- **Claude chat and Cowork.** New versions come from this repository. In **Customize > Plugins**, select **Check for updates** to pull the latest. For a marketplace you added from github.com, you can also turn on **Sync automatically**.
- **Claude Code.** Run `claude plugin update <name>@yurii-ned-plugins`, or turn on auto-update for the marketplace in `/plugin`.

## Notes

- On Team and Enterprise plans, an owner can limit which sources members may add. If you can't add the marketplace, ask your owner.
- Anthropic does not review plugins you add from a marketplace URL. Read what a plugin contains before you add it.
