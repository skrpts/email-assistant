---
type: service
id: gmail-mcp
title: Gmail MCP
description: "Gmail MCP server for reading emails, drafting replies, and managing inbox"
tags: [Production, Email]
connections: []
metadata:
  provider: gmail
  protocol: mcp
  auth_type: oauth
  env_var: GMAIL_OAUTH_CREDENTIALS
  required_scopes: [gmail.modify]
---

## Service Description

Provides access to Gmail via the Model Context Protocol (MCP). This service is used to scan the inbox, read email content, and create draft replies. Note: this skrpt requires `gmail.modify` scope (not just `gmail.readonly`) because it creates Gmail drafts.

## Configuration

### Authentication

Requires OAuth 2.0 credentials for Google. Set `GMAIL_OAUTH_CREDENTIALS` to the path of your credentials JSON file. The OAuth scope must include `gmail.modify` to allow draft creation.

### MCP Server Setup

```json
{
  "mcpServers": {
    "gmail": {
      "command": "npx",
      "args": ["-y", "gmail-mcp-server"],
      "env": {
        "GMAIL_OAUTH_CREDENTIALS": "{GMAIL_OAUTH_CREDENTIALS}"
      }
    }
  }
}
```

## Capabilities Used

### Reading

- `search_emails` — search inbox using Gmail query syntax
- `read_email` — retrieve full email content by message ID
- `list_email_labels` — list available Gmail labels

### Writing

- `draft_email` — create a Gmail draft (does NOT send). Parameters: `to`, `subject`, `body`, `cc`

### Not Used by This Skrpt

- `send_email` — intentionally not used. Drafts are created for human review in Gmail, never sent directly.
- `modify_email` — not used
- `delete_email` — not used

## Privacy Considerations

Email content is sent to your configured LLM provider to generate reply drafts. The `data_handling: pii` declaration makes this explicit. Draft replies are saved to your Gmail Drafts folder — they are NOT sent automatically.
