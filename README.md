# ZenSched MCP

ZenSched is field-workforce scheduling for AI agents. Your MCP client goes from schedule → GPS-verified punches → forms → timesheets. **No dashboard required.**

This repo is the public **plugin / directory kit**: a root `.mcp.json` so [cursor.directory](https://cursor.directory) and other Open Plugins scanners can list the hosted server. It is not a vertical operations kit (those live at `zenschedmcp/zensched-*-kit` and add a local SQLite database).

- **Docs:** [zensched.com/docs/quickstart](https://www.zensched.com/docs/quickstart/)
- **Pricing:** [zensched.com/pricing](https://www.zensched.com/pricing/)
- **Endpoint:** `https://mcp.zensched.com/mcp`
- **Keys:** `zsc_…` (the key *is* the organization)

## Connect

`.mcp.json` and `mcp.json` in this repo point at the hosted server **without** embedding a key (directory scanners must not ship secrets).

```json
{
  "mcpServers": {
    "zensched": {
      "url": "https://mcp.zensched.com/mcp",
      "headers": { "Authorization": "Bearer zsc_…" }
    }
  }
}
```

`mcp.json.example` is the same block with a `zsc_your_key_here` placeholder for paste-into-Cursor / Claude Desktop.

Account and guide tools work **before** a key exists. After `account_create`, put the `zsc_` key in the header (or call `account_use_key` for the current session).

## From zero

1. Call `zensched_guide`.
2. Call `account_create` with your company name. Copy the `zsc_` key.
3. Free tools work within **200 MCP calls/day**. Metered actions (`location_create` geocode, `worker_invite`, GPS verify, forms, processed timesheets) return `payment_required` with the payment path. The **$5** activation deposit is credited to balance.
4. Invite workers. They use the mobile app — [App Store](https://apps.apple.com/us/app/zensched/id6800081657) · [Google Play](https://play.google.com/store/apps/details?id=com.zensched.app).

Full tool list: [zensched.com/docs/tools](https://www.zensched.com/docs/tools/). Agent instructions: `skills/zensched/SKILL.md`.

## Directory listing

This repository exists so publisher forms that want a **public GitHub repo with `.mcp.json`** have something to scan. Submit:

`https://github.com/zenschedmcp/zensched`

at [cursor.directory/plugins/new](https://cursor.directory/plugins/new) (GitHub or Google sign-in). Auto-detect looks for `.mcp.json` at the repo root.

Also listed on the [official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=zensched) as `com.zensched/zensched`, [Glama](https://glama.ai/mcp/connectors/com.zensched/zensched), [Smithery](https://smithery.ai/servers/mikef/zensched), and [mcp.so](https://mcp.so/servers/zensched).

## License

MIT. See `LICENSE`.
