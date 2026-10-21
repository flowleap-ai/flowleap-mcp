# FlowLeap Patent AI: hosted MCP server

FlowLeap Patent AI is a hosted, remote [Model Context Protocol](https://modelcontextprotocol.io)
(MCP) server for patent data. It gives your AI assistant read-only tools for
EPO and USPTO records, patent searches, patent-law references and PATSTAT
analytics.

```
https://api.flowleap.co/mcp
```

## What it is

| | |
|---|---|
| Server URL | `https://api.flowleap.co/mcp` |
| Transport | Streamable HTTP (remote; nothing to install) |
| Tools | 49 tools, all read-only |
| Sign-in | OAuth 2.1 with a FlowLeap account, one scope: `patent-data` |
| Publisher | Flow Leap SASU |
| Registry name | `co.flowleap/flowleap` |

The server also serves 6 MCP resources and 3 MCP prompts: the PATSTAT
reference documents and three PATSTAT workflows.

## What it does

Every tool reads data. No tool writes, files or changes anything at a patent
office. The server's live tool list is the reference; the groups below are the
tools at the time of writing.

**EPO OPS document reads** (EPO Open Patent Services, worldwide coverage)

| Tool | What it returns |
|---|---|
| `get_bibliography` | Title, abstract, applicants, inventors, IPC/CPC, dates, cited references |
| `get_abstract` | The abstract text |
| `get_claims` | The claims text, language selectable |
| `get_description` | The description (specification) text |
| `get_fulltext` | Which claims and description languages are available |
| `get_family` | The extended INPADOC family |
| `get_patent_family` | The simple family (equivalents) |
| `get_legal_status` | Legal-status events: grants, lapses, oppositions |
| `get_register_events` | EP Register prosecution events |
| `get_citations` | Forward citations: the documents that cite a patent |
| `get_patent_image` | Drawings and figures |
| `convert_patent_number` | A number converted between epodoc, docdb and original formats |

**USPTO ODP file wrappers** (USPTO Open Data Portal)

| Tool | What it returns |
|---|---|
| `get_us_grant` | A granted US patent, resolved to its application file wrapper |
| `get_us_application` | The full application file wrapper by application number |
| `get_continuity` | The parent and child continuity chain |
| `get_transactions` | The prosecution transaction history |
| `get_assignments` | Assignment records: chain of title and current owner |
| `get_foreign_priority` | Foreign priority claims |
| `get_patent_term_adjustment` | Official patent term adjustment (PTA) data |
| `get_attorney` | The attorney or agent of record and the customer number |
| `search_uspto_portfolio_by_customer_number` | The US applications filed under a customer number |
| `get_application_documents` | The Image File Wrapper (IFW) document list |
| `read_application_document` | One IFW document, such as an office action, as text |

**USPTO citations** (USPTO enriched citation data from office actions)

| Tool | What it returns |
|---|---|
| `search_office_action_citations` | The prior art an examiner cited against an application |
| `search_enriched_citations` | The office actions that cite a given document |
| `get_citation_stats` | Citation counts by category (X, Y, A) for an application |

**Searches**

| Tool | What it returns |
|---|---|
| `search_patents` | A patent search: EPO OPS CQL (worldwide) or USPTO ODP Lucene (US) |
| `get_search_syntax` | The query grammar for each search provider |
| `patent_analytics` | Filing trends, country and CPC breakdowns, and top assignees over a worldwide corpus |
| `search_academic` | Academic literature from Semantic Scholar and arXiv |
| `search_npl` | Non-patent literature from OpenAlex |

**PATSTAT analytics** (FlowLeap's self-hosted copy of EPO PATSTAT Global)

| Tool | What it returns |
|---|---|
| `patstat_portfolio` | One applicant's portfolio: applications by filing year, office and grant status |
| `patstat_technology` | The landscape of one CPC class: top applicants, filing trend, grant rate by office, new entrants, most-cited families |
| `patstat_patent` | One patent's full picture: citations in both directions with their citation origin, family, applicants, CPC, priorities |
| `patstat_query` | One guarded, read-only SQL `SELECT` over the documented PATSTAT views |
| `patstat_docs` | The PATSTAT reference documents and the semantic model of the views |
| `patstat_resolve` | A publication number or an applicant name resolved to a graph anchor |
| `patstat_cpc` | A keyword search over the CPC scheme titles |
| `patstat_applicant` | One applicant's card: filings by year, top CPC areas, jurisdictions, co-applicants |
| `patstat_neighborhood` | The citation and family neighborhood of a patent, one or two hops |
| `patstat_path` | The shortest citation or family path between two patents |
| `patstat_explain` | One node's card with its top connections |

**One-call verbs** (several reads combined in one call)

| Tool | What it returns |
|---|---|
| `get_patent_summary` | Bibliography, legal status, family and estimated term |
| `compare_patents` | A side-by-side bibliographic comparison of 2 to 10 patents |
| `get_prosecution_timeline` | EP Register events and INPADOC legal events in date order |
| `get_patent_term` | The estimated patent term |
| `examiner_baseline` | Every document cited across a granted patent's family, by office |

**Utilities**

| Tool | What it returns |
|---|---|
| `ocr` | Text from a PDF, an image or an office document |
| `server_info` | The server's configured providers and available tools |

## How to connect

Add the server URL to your client. On first use, the client opens the
FlowLeap sign-in page. Sign in with your FlowLeap account and allow the
`patent-data` scope.

### VS Code (GitHub Copilot)

Install in one click:

```
vscode:mcp/install?%7B%22name%22%3A%22flowleap%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.flowleap.co%2Fmcp%22%7D
```

Or add the server to your MCP configuration (`.vscode/mcp.json` uses
`servers`):

```json
{
  "servers": {
    "flowleap": { "type": "http", "url": "https://api.flowleap.co/mcp" }
  }
}
```

### Cursor

Install in one click:

```
cursor://anysphere.cursor-deeplink/mcp/install?name=flowleap&config=eyJ1cmwiOiJodHRwczovL2FwaS5mbG93bGVhcC5jby9tY3AifQ==
```

Or add it to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "flowleap": { "url": "https://api.flowleap.co/mcp" }
  }
}
```

### Claude Code

```bash
claude mcp add --transport http flowleap https://api.flowleap.co/mcp
```

Then run `/mcp` in Claude Code to sign in.

### claude.ai, Claude Desktop and the Claude mobile apps

1. In Settings, open Connectors and add a custom connector.
2. Paste the server URL. No other field is needed.
3. Sign in to FlowLeap and allow the `patent-data` scope.

A connector you add on claude.ai belongs to your Claude account, so Claude
Desktop and the Claude mobile apps show it too.

### ChatGPT

1. In Settings, turn on developer mode.
2. Create a connector with the server URL and OAuth as its authentication.
3. Sign in to FlowLeap and allow the `patent-data` scope.

### Gemini CLI

In `~/.gemini/settings.json`:

```json
{
  "mcpServers": {
    "flowleap": { "httpUrl": "https://api.flowleap.co/mcp" }
  }
}
```

### Windsurf

In `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "flowleap": { "serverUrl": "https://api.flowleap.co/mcp" }
  }
}
```

### Zed

In `settings.json`:

```json
{
  "context_servers": {
    "flowleap": { "url": "https://api.flowleap.co/mcp" }
  }
}
```

If the agent lists only the authenticate tool after sign-in, ask it to
authenticate once.

### Amazon Q Developer CLI

In the agent configuration:

```json
{
  "mcpServers": {
    "flowleap": { "type": "http", "url": "https://api.flowleap.co/mcp" }
  }
}
```

Then run `/mcp` in a chat to sign in in your browser.

### Clients without OAuth: a personal token

A client that cannot sign in with OAuth can send a FlowLeap personal token as
a bearer header. Make the token with the FlowLeap CLI
(`flowleap auth create-token`). You can revoke it at any time. Replace
`fl_pat_...` with your token.

Claude Code:

```bash
claude mcp add --transport http flowleap https://api.flowleap.co/mcp --header "Authorization: Bearer fl_pat_..."
```

Cursor and other clients with a `url` and `headers` entry:

```json
{
  "mcpServers": {
    "flowleap": {
      "url": "https://api.flowleap.co/mcp",
      "headers": { "Authorization": "Bearer fl_pat_..." }
    }
  }
}
```

A personal token gives the same access as the OAuth sign-in.

## Patent-Data Keys

The office tools (EPO OPS and USPTO ODP) read the patent office's data with a
Patent-Data Key: an EPO OPS consumer key and secret, or a USPTO ODP API key.
Each office gives these keys free of charge.

- If your FlowLeap plan does not include FlowLeap's own office keys, the
  office tools use your own key.
- Add your key once on the Patent-data keys page of your FlowLeap dashboard:
  https://www.flowleap.co/en/dashboard/keys. FlowLeap checks the key with the
  office, then keeps it encrypted. FlowLeap never shows the key value again.
- Never type or paste a key in the chat. The assistant does not take a key in
  the chat. When a key is missing, the tool result names the keys page.
- The PATSTAT tools need no Patent-Data Key.

## Example prompts

- "Show me Siemens' patent filings by year and office since 2016."
- "Who dominates solid-state batteries?"
- "Who cites EP3477840, and how?"
- "Which CPC codes do perovskite tandem solar cell filings use?"
- "How many of Siemens' EP patents were opposed, by year?"

## Data and provenance

- **EPO OPS**: live reads from the European Patent Office's Open Patent
  Services.
- **USPTO ODP**: live reads from the USPTO Open Data Portal.
- **EPO PATSTAT Global**: a self-hosted copy of the EPO's worldwide patent
  statistics database. Every PATSTAT answer names its data edition and carries
  the EPO attribution.
- **Read-only**: every tool declares `readOnlyHint: true`. The server never
  writes to a patent office or to your data.

## Links

- Documentation: https://www.flowleap.co/en/mcp
- Privacy policy: https://www.flowleap.co/en/privacy
- Terms: https://www.flowleap.co/en/terms
- Support: contact@flowleap.co
- Official MCP Registry entry: https://registry.modelcontextprotocol.io/v0.1/servers?search=co.flowleap

The server's source code is private. This repository documents the hosted
server. `server.json` is a copy of its Official MCP Registry entry, for
reference.

## License

The contents of this repository are under the MIT License. See `LICENSE`.
