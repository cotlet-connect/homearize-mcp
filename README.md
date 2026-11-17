# HomeArize MCP server

Connect HomeArize to ChatGPT, Claude and any AI agent through Cotlet.dev. Work in your own signed-in HomeArize account with 9 hosted tools for customers, quotes, invoices, installers, jobs, stock, purchase orders, commissions and reports. Write tools ask for approval by default.

## What you can ask

> Draft a flooring quote from the measurements and products I provide.

> Show stock on hand and allocated to jobs.

> List purchase orders still pending.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its HomeArize tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 homearize_agent.py serve
```

Or with Docker:

```bash
docker build -t homearize-mcp .
docker run -i --rm -e COTLET_API_KEY homearize-mcp
```

Tools: `homearize_guide`, `homearize_status`, `homearize_open`, `homearize_read`, `homearize_click`, `homearize_type`, `homearize_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 homearize_agent.py connect   # ready config for ChatGPT, Claude, Claude Code and Cursor
python3 homearize_agent.py status    # is HomeArize ready in your Cotlet.dev account
python3 homearize_agent.py guide     # HomeArize screens and sign-in link
python3 homearize_agent.py read      # open HomeArize, read the page, close the browser
python3 homearize_agent.py tools     # list Cotlet.dev tools
python3 homearize_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from HomeArize users

**How do I connect HomeArize?**

Request access at https://cotlet.dev/signup?app=homearize and sign in at https://app.homearize.com/ in your Cotlet Cloud Browser. Then add the Cotlet connector to your AI agent.

**Which actions ask for approval?**

The hosted integration has 3 read tools and 6 write tools. Write tools ask for approval by default. Purchase orders require the HomeArize Growth plan or above; inventory requires Enterprise.

**Is Cotlet affiliated with HomeArize?**

No. Cotlet.dev is independent and not affiliated with HomeArize.

## Learn more

- [HomeArize integration and hosted tools](https://cotlet.dev/integrations/homearize-api-mcp-server)

---

More: https://cotlet.dev/integrations/homearize-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=homearize

Request access: https://cotlet.dev/signup?app=homearize&utm_source=github&utm_medium=readme&utm_campaign=homearize

## Workflows on the hosted integration

The live integration page documents these 9 operations:

- `manage_customers`
- `create_quotes`
- `create_invoices`
- `schedule_contractors`
- `manage_jobs`
- `track_inventory`
- `create_purchase_orders`
- `track_commissions`
- `view_business_reports`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
