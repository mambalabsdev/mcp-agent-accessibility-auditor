# Agent Accessibility Auditor MCP Server

[![Smithery](https://smithery.ai/badge/mambabuilt/mcp-agent-accessibility-auditor)](https://smithery.ai/servers/mambabuilt/mcp-agent-accessibility-auditor) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-agent-accessibility-auditor/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-agent-accessibility-auditor) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-agent-accessibility-auditor%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-agent-accessibility-auditor&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-agent-accessibility-auditor)](https://www.npmjs.com/package/@mambalabsdev/mcp-agent-accessibility-auditor) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-agent-accessibility-auditor)](https://www.npmjs.com/package/@mambalabsdev/mcp-agent-accessibility-auditor) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-agent-accessibility-auditor)](https://github.com/mambalabsdev/mcp-agent-accessibility-auditor/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-agent-accessibility-auditor)

MCP server for the Mamba Labs [Agent Accessibility Auditor](https://apify.com/mambalabs/agent-accessibility-auditor) actor on Apify.

Can an AI agent read this site? Give it a domain and it returns one flat row of 42 fields covering five families of fact: the `llms.txt` family, robots AI crawler policy including the newer Content Signal directives, structured data presence and health, render mode, and machine readable endpoint discovery.

## Install

```bash
npx -y @mambalabsdev/mcp-agent-accessibility-auditor
```

### Claude Desktop

```json
{
  "mcpServers": {
    "mamba-agent-accessibility-auditor": {
      "command": "npx",
      "args": ["-y", "@mambalabsdev/mcp-agent-accessibility-auditor"],
      "env": { "APIFY_TOKEN": "your-apify-token" }
    }
  }
}
```

Get an Apify token at [console.apify.com/account/integrations](https://console.apify.com/account/integrations).

## Tool

### `audit_agent_accessibility`

Domain in, whether an AI agent can read that site out.

| Input | Type | Required | Notes |
| --- | --- | --- | --- |
| `domain` | string | yes | One company domain, for example vercel.com. Protocol and path are stripped. |
| `check_endpoints` | boolean | no | Probes sitemap, OpenAPI, well known files and feeds. Adds 7 concurrent requests. Default `true`. |
| `check_structured_data` | boolean | no | Parses JSON-LD, microdata, Open Graph and canonical off the homepage. Costs no extra requests. Default `true`. |
| `skipCache` | enum | no | Leave as `false` to use the 7 day cache. Set to `true` to re-audit the domain from scratch. Default `false`. |

## Reading the output

Every field is a fact read off a fetch. No model is called at any point, so the same domain returns the same row today and next month unless the site actually changed.

`has_llms_txt` is true only when `/llms.txt` returns 200 and the body is real markdown, and `llms_txt_reject_reason` says why a 200 was not counted. Twelve requests per domain, `robots.txt` first and then the homepage and ten probes concurrently. Typical wall clock is 2 to 4 seconds.

Built for a technical SEO or growth engineer preparing a site for AI crawlers and agent traffic, or an agency selling that work and needing a before and after audit across a client list.

## Billing

You are charged per domain analyzed, plus a small actor start fee. A repeat run inside the 7 day cache window costs nothing new.

Pricing is on the [actor's Apify page](https://apify.com/mambalabs/agent-accessibility-auditor). Running this server consumes Apify credits.

## What this server does and does not do

It is a thin client for the Apify actor. It passes your input through and returns the actor's output unchanged. Every behavior described above lives in the actor, not here.

Errors are surfaced, never swallowed. An invalid input, an invalid token, an exhausted balance, a timeout, or a run that returns anything other than a dataset all come back as an explicit tool error rather than as an empty result.

## Source

The actor is on the [Apify Store](https://apify.com/mambalabs/agent-accessibility-auditor). This wrapper is [MIT licensed](LICENSE).

Built by [Mamba Labs](https://apify.com/mambalabs)
