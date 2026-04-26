---
type: skill
id: inbox-scan
title: Inbox Scan
description: "Fetches recent inbox emails from Gmail via MCP"
tags: [Production, Email]
connections:
  - target: gmail-mcp
    type: runs_on
  - target: llm-service
    type: runs_on
---

## Capability

Searches the Gmail inbox for recent emails and retrieves their content for categorisation.

## What It Does

1. **Search inbox** — calls `search_emails` with `in:inbox newer_than:{lookback}h`, limited to the configured max
2. **Read content** — calls `read_email` for each result to get sender, subject, body, timestamp, and thread context
3. **Structure output** — produces a list of emails ready for categorisation

## Outputs

Structured email set: list of messages with full content, sender, subject, timestamp, labels, and thread info.
