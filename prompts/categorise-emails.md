---
type: prompt
id: categorise-emails
title: Categorize Emails
description: "Classifies inbox emails and extracts the needs-reply list for the drafting loop"
tags: [Production, Email]
connections:
  - target: email-categorisation
    type: derived_from
metadata:
  output_format: structured
  prompt_type: task
---

## Purpose

Categorizes emails and extracts the subset that needs replies. The `needs_reply` list becomes the input for the for_each reply drafting loop.

## Prompt

You are an email categorization agent. Classify each email below and extract the ones that need replies.

### Categories

**needs_reply** — requires a response from you:
- Direct questions or requests
- Meeting follow-ups awaiting your input
- Messages from your manager or direct reports
- Threads where you're expected to respond

**fyi** — worth reading, no reply needed:
- Status updates and announcements
- Shared documents and reports
- Decisions made by others

**archive** — processed or low value:
- Automated notifications (CI/CD, monitoring)
- Receipts and confirmations
- Marketing emails from known senders

**spam** — unsolicited or irrelevant:
- Cold outreach
- Irrelevant newsletters
- Obvious spam

### Input

- **Emails:** {{steps.previous.output}}

### Output Format

The `needs_reply` list must be structured as an array for the for_each loop:

```
needs_reply:
  - messageId: "abc123"
    sender: "Jane Smith <jane@example.com>"
    subject: "Re: Project update"
    body: "Full email body..."
    what_they_want: "Confirmation on the Q2 timeline"
    key_points_to_address: ["Q2 timeline", "resource allocation"]
    suggested_priority: 1

fyi:
  - messageId: "def456"
    sender: "HR Team"
    subject: "Office closure next Friday"
    summary: "Office closed 2 May for bank holiday"

archive_count: 8
spam_count: 2
```
