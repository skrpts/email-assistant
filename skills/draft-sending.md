---
type: skill
id: draft-sending
title: Draft Sending
description: "Saves approved reply drafts to Gmail via MCP — creates drafts, does NOT send"
tags: [Production, Email]
connections:
  - target: gmail-mcp
    type: runs_on
  - target: llm-service
    type: runs_on
---

## Capability

Takes the gate-approved reply drafts and saves them to Gmail as drafts using the MCP server. You can then review and send them from Gmail at your convenience.

**Important:** This step creates Gmail drafts — it does NOT send emails. You retain full control over when and whether each reply is actually sent.

## What It Does

1. For each approved draft, calls `draft_email` with:
   - `to` — the original sender's email address
   - `subject` — "Re: {original subject}"
   - `body` — the approved reply text
   - `cc` — any CC recipients from the original email (if appropriate)
2. Reports the number of drafts created and links to the Gmail Drafts folder

## What It Does NOT Do

- Send emails — only creates drafts
- Modify or delete existing emails
- Send to recipients not in the original email thread

## Outputs

Confirmation: number of drafts created, with a summary of each (recipient, subject).
