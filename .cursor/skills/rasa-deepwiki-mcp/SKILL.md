---
name: rasa-deepwiki-mcp
description: >
  Integrates the public DeepWiki MCP server so the assistant can answer questions
  about public GitHub repositories. Use when configuring or extending DeepWiki in
  endpoints.yml, the public_github_repo_info flow, or MCP tool mappings
  (ask_question, read_wiki_structure, read_wiki_contents).
license: Apache-2.0
metadata:
  author: rasa
  version: "0.1.0"
  rasa_version: ">=3.14.0"
  docs-url: https://docs.devin.ai/work-with-devin/deepwiki-mcp
---

# DeepWiki MCP in this assistant

[DeepWiki MCP](https://docs.devin.ai/work-with-devin/deepwiki-mcp) is a free, remote MCP service (no API key) that exposes documentation and Q&A for **public** GitHub repositories.

## Server URL

- **Streamable HTTP (recommended):** `https://mcp.deepwiki.com/mcp`
- Registered in `endpoints.yml` as `mcp_servers` entry `name: deepwiki`.

## Tools (from the MCP server)

| Tool | Purpose |
|------|---------|
| `ask_question` | Natural-language Q&A grounded in the repo (used by the default flow). |
| `read_wiki_structure` | List documentation topics for `repoName` (owner/repo). |
| `read_wiki_contents` | Fetch wiki-style documentation for `repoName`. |

Parameters use **`repoName`** (string, owner/repo, e.g. `rasaHQ/rasa`). `ask_question` also requires **`question`**.

## Assistant behavior

- **Flow:** `public_github_repo_info` in `data/general/public_github_repo_info.yml`
- **Slots:** `github_repo`, `github_repo_question`, `deepwiki_answer`
- **MCP integration:** a flow `call` step invokes **`ask_question`** on `mcp_server: deepwiki`, with `mapping` from slots to `repoName` and `question`, and output into `deepwiki_answer` via `result.structuredContent.result` (same tool names as the Cursor **deepwiki** MCP: `read_wiki_structure`, `read_wiki_contents`, `ask_question`).
- **Timeout note:** Rasa’s built-in MCP tool executor uses a **fixed ~10s** read timeout for flow `call` steps (not overridden by `endpoints.yml` in current releases). Slow DeepWiki responses can hit that limit; if that becomes a problem, track Rasa Pro releases or support channels for a configurable MCP tool timeout.

## Extending

- Add `call` steps for `read_wiki_structure` or `read_wiki_contents` in new or branched flows when users need a topic list or raw wiki text instead of Q&A.
- Private repositories require a different product (see Devin docs); this project only configures the public DeepWiki endpoint.
