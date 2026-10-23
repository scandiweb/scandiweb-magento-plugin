---
name: setup
description: Connect, sign in to, repair, or switch the merchant's own Magento store connection (Scandiweb Magento MCP). Use for first-time plugin setup, when the user gives a Magento store or /mcp URL, when Magento tools are missing, or when Magento authorization has expired.
---

# Connect a Magento store

Guide the user through connecting their own store. Claude handles the
connection and sign-in natively, so there is no helper script to run. Never ask
for Magento passwords, admin tokens, client secrets, or OAuth callback codes in
chat. Do not install Magento modules or change store content during setup.

## 1. Check whether a store is already connected

Look at the tools available in this conversation. If tools such as
`scandiweb_mcp_whoami` or `scandiweb_mcpcontent_product` are present (the prefix
depends on how the connection was added), a store is already connected. Skip to
step 4 and verify it. If tools from more than one store are present, ask which
store the user means before doing anything else.

## 2. Ask for the store endpoint

Ask for the full HTTPS MCP endpoint of the user's own store, normally
`https://store.example.com/mcp`. If they already gave it, use it. If they give a
homepage URL, confirm the derived `/mcp` endpoint with them before using it.
Never use a demo store or a URL from another conversation.

The endpoint must:

- use `https://`, with no username, password, query string, or fragment
- use the store's public hostname, not an IP address or `localhost`
- include the path (normally `/mcp`), not just the homepage

If the Scandiweb Magento MCP modules are not installed on the store yet, point
the user to the [installation guide](https://github.com/scandiweb/scandiweb-magento2-mcp)
and wait until they confirm the modules are installed and enabled.

## 3. Connect, depending on where the user is

Work out which Claude app the user is in from the context. If unsure, ask.

### Claude on the web, desktop, or mobile app (chat and Cowork)

These apps connect to the store as a custom connector. Give the user these
steps, filled in with their endpoint:

1. Open **Settings > Connectors** (on Team and Enterprise plans an Owner may
   need to add it under **Organization settings > Connectors** first).
2. Choose **Add custom connector**.
3. Name it, for example `Magento - <store host>`, and paste the endpoint as the
   URL. Leave the OAuth Client ID and Secret empty; the store registers Claude
   automatically.
4. Select **Add**, then **Connect**. A browser window opens on the store's
   Magento admin. Sign in and select **Authorize**.
5. Back in Claude, enable the connector for the conversation (the tools menu in
   the message box) and say "Verify my Magento connection".

### Claude Code

The plugin declares the store as an MCP server named `magento`, using the
**Magento MCP endpoint** value asked for when the plugin was enabled.

- If the value is missing or wrong, ask the user to update it in `/config`
  (the plugin's **Magento MCP endpoint** row) or by re-enabling the plugin,
  then run `/reload-plugins` or start a new session.
- To sign in, ask the user to run `/mcp`, select the `magento` server, and
  choose **Authenticate**. A browser opens on the store's Magento admin; they
  sign in and select **Authorize**.
- If the store needs a static admin Bearer token instead of OAuth, the user can
  follow the Claude Code section of the module README. Do not ask for the token
  in chat and do not run commands that print it.

Do not edit Claude settings files or MCP configuration files yourself, and do
not read credential stores. Never silently replace, repoint, or remove an
existing connection.

## 4. Verify and finish

Call only the connected store's `scandiweb_mcp_whoami` tool, with empty
arguments. A good response identifies `Scandiweb_Mcp` and reports the
administrator and role status. Report only what it returns. No store writes,
catalog scans, or permission tests are part of setup.

- If the tools are not available yet, say the store is **configured, not yet
  connected**, and ask the user to finish sign-in (or reload plugins in Claude
  Code) and then ask again. Do not claim success before `scandiweb_mcp_whoami`
  answers.
- If the call returns an authorization error, ask the user to reconnect
  (**Settings > Connectors** in the apps, or `/mcp` > **Authenticate** in
  Claude Code). Never try to broaden Magento permissions to fix a failure.

After a successful identity check, confirm the store host and role status, then
offer a read-only first task such as listing five products. For all later store
work, follow the `magento-store` skill in this plugin.

Removing the plugin does not remove a custom connector the user added in
Settings, and does not revoke the store's authorization. To fully disconnect,
remove the connector in Claude and revoke the integration in Magento admin.
