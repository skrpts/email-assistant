---
type: skill
id: email-categorisation
title: Email Categorization
description: "Classifies inbox emails — needs reply, FYI, archive, or spam"
tags: [Production, Email]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Categorizes each email and extracts the subset that needs replies. The "needs reply" list becomes the input for the for_each reply drafting loop.

## What It Does

1. **Needs reply** — emails requiring a response: direct questions, requests, meeting follow-ups, messages from important contacts
2. **FYI** — worth reading but no reply needed: announcements, updates, shared documents
3. **Archive** — processed or low-value: automated notifications, receipts, marketing
4. **Spam/noise** — should be filtered: unsolicited outreach, irrelevant newsletters

For each "needs reply" email, extracts: what the sender wants, suggested reply priority, and key points to address.

## Outputs

Categorized email set. The `needs_reply` list is structured for the for_each loop: each item contains the full email plus reply guidance.
