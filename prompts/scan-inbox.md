---
type: prompt
id: scan-inbox
title: Scan Inbox
description: "Fetches recent inbox emails from Gmail via MCP"
tags: [Production, Email]
inputs:
  lookback_hours:
    label: "Lookback Hours"
    description: "How many hours of inbox to scan"
    example: "24"
    required: false
    type: text
  max_emails:
    label: "Max Emails"
    description: "Maximum number of emails to process"
    example: "20"
    required: false
    type: text
connections:
  - target: inbox-scan
    type: derived_from
metadata:
  output_format: structured
  prompt_type: task
---

## Purpose

Fetches recent inbox emails for categorization and reply drafting.

## Prompt

You are a data retrieval agent. Using the Gmail MCP server, fetch recent inbox emails.

### Steps

1. Call `search_emails` with the query `in:inbox newer_than:{{input.lookback_hours}}h` (default: `newer_than:24h`). Set `maxResults` to {{input.max_emails}} (default: 20).
2. For each email, call `read_email` with the `messageId` to get: sender (name + email), subject, body text, timestamp, labels, thread ID, and whether it's a reply in a thread.
3. Include thread context: if the email is part of a thread, note the thread subject and message count.

### Output Format

```
emails:
  - messageId: "abc123"
    sender: "Jane Smith <jane@example.com>"
    subject: "Re: Project update"
    body: "Full email body text..."
    timestamp: "2026-04-26T08:15:00Z"
    labels: ["INBOX", "IMPORTANT"]
    thread_id: "thread456"
    is_reply: true
    thread_message_count: 4
```

### Error Handling

- If Gmail is unreachable, report the error clearly
- If no emails match, return an empty set
