# Scandiweb Magento

Manage your Magento 2 catalog and storefront content by talking to Claude. This
plugin connects Claude to your own store through the Scandiweb Magento MCP
modules, and teaches Claude how to work with products, prices, stock,
categories, CMS pages and blocks, images, Hyva CMS content, and Snowdog menus
safely.

![Scandiweb Magento](assets/icon.png)

## Before you start

Your store must already have the Scandiweb Magento MCP modules installed and
enabled: https://github.com/scandiweb/scandiweb-magento2-mcp. Each merchant
connects their own store and signs in with their own Magento admin account.
There is no Scandiweb-hosted relay, and no store address is built into this
plugin.

## What's inside

- **setup** skill: walks you through connecting your store, signing in, and
  checking the connection with a read-only identity call.
- **magento-store** skill: rules Claude follows when reading and editing store
  data, such as checking store-view scope, confirming price, stock, bulk and
  publish changes before making them, and reading changes back to confirm
  they saved.
- An MCP server entry named `magento` that points at your store's endpoint
  (used by Claude Code).

## Install

- **Claude apps:** find **Scandiweb Magento** under **Customize > Plugins > Discover** and add it.
- **Claude Code:**

```
/plugin marketplace add scandiweb/scandiweb-magento-plugin
/plugin install scandiweb-magento@scandiweb
```

## Set it up

After adding the plugin, say "Set up my Magento store connection."

- **Claude apps (web, desktop, mobile, Cowork):** Claude guides you to add your
  store's `https://your-store.com/mcp` address under **Settings > Connectors >
  Add custom connector**. Leave the client ID and secret empty, select
  **Connect**, sign in to your Magento admin and select **Authorize**.
- **Claude Code:** when you enable the plugin, Claude Code asks for your
  **Magento MCP endpoint**. Then run `/mcp`, pick `magento`, and choose
  **Authenticate** to sign in through your Magento admin.

Then try:

- "List the first five products in my store with SKU, name and price."
- "Show my CMS pages and which store views they are assigned to."
- "Which products in the Shoes category have less than 5 in stock?"
- "Update the meta description of the About Us CMS page, and show me the
  change before saving it."

## Data and permissions

- Claude talks only to the store address you provide. Requests and responses
  (catalog records, CMS content, admin identity) travel directly between Claude
  and your store over HTTPS.
- Sign-in uses OAuth 2.1 with dynamic client registration. Tokens are stored by
  Claude's own connector storage, never in this plugin's files or in chat.
- Every tool call runs as the Magento admin who signed in, limited by that
  admin's role. Claude can never do more than that role allows.
- Edits change live store data. Claude asks before price, stock, bulk,
  publish/unpublish and destructive changes.
- The plugin itself contains only instructions and configuration. It runs no
  scripts, stores nothing, and sends nothing anywhere else.

## Support

- Support: https://github.com/scandiweb/scandiweb-magento2-mcp/blob/master/SUPPORT.md
- Privacy: https://github.com/scandiweb/scandiweb-magento2-mcp/blob/master/PRIVACY.md
- Terms: https://github.com/scandiweb/scandiweb-magento2-mcp/blob/master/TERMS.md
- Contact: info@scandiweb.com

## License

Open Software License 3.0 (OSL-3.0).
