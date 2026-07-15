---
type: workflow
id: email-assistant
title: Email Assistant
description: "Scans inbox via Gmail MCP, categorizes emails, drafts replies in a for_each loop, gates for human review, then saves as Gmail drafts"
tags: [Production, Email, Loop, Gate]
connections:
  - target: inbox-scan
    type: uses
  - target: email-categorisation
    type: uses
  - target: reply-drafting
    type: uses
  - target: draft-gate
    type: uses
  - target: draft-sending
    type: uses
  - target: gmail-mcp
    type: runs_on
  - target: llm-service
    type: runs_on
metadata:
  estimated_duration: "30-120 seconds"
  avg_tokens: 20000
  trigger: manual
  loop_modes: ["for_each"]
loops:
  - id: "reply-drafting"
    mode: "for_each"
    inputExpression: "{{steps.Email Categorisation.output}}"
    steps:
      - "reply-drafting"
    maxIterations: 50
output_step: "draft-sending"
composite_steps:
  - "inbox-scan"
  - "email-categorisation"
  - "reply-drafting"
  - "draft-gate"
  - "draft-sending"
execution:
  - skill: "inbox-scan"
    step_type: "generation"
    prompt: "scan-inbox"
    output: { name: "inbox_scan", type: "text" }
  - skill: "email-categorisation"
    step_type: "synthesis"
    prompt: "categorise-emails"
    output: { name: "categories", type: "text" }
  - skill: "reply-drafting"
    step_type: "content"
    prompt: "draft-reply-batch"
    output: { name: "draft_replies", type: "text" }
    context:
      voice_profile: "Neutral professional tone"
      reply_tone: "Professional"
      reply_length: "Concise"
  - skill: "draft-gate"
    step_type: "validation"
    gate: true
    prompt: "review-drafts"
    output: { name: "gate_decision", type: "decision" }
  - skill: "draft-sending"
    step_type: "content"
    prompt: "send-drafts"
    output: { name: "sent_confirmation", type: "text" }
---

## Overview

This workflow is your daily email assistant. It scans your inbox, identifies which emails need replies, drafts a reply to each one in your voice, pauses for your review, and then saves the approved replies as Gmail drafts.

The **for_each loop** drafts one reply per email. The **gate step** ensures you review every draft before anything is saved. Drafts are saved to Gmail — never sent automatically.

## Pipeline Stages

### Stage 1: Inbox Scan

**Input:** Lookback period, max emails

Using the Gmail MCP service, fetch recent inbox emails with full content, sender info, and thread context.

### Stage 2: Email Categorization

Classifies each email into four categories: needs reply, FYI, archive, or spam. Extracts the "needs reply" subset with guidance on what each sender wants.

### Stage 3: Reply Drafting (for_each Loop)

The for_each loop iterates over the "needs reply" list. For each email, a reply is drafted in your voice using the configured tone and length. Each iteration is independent — drafts don't see each other.

**Loop variables:**
- `{{loop.item}}` — the current email with sender, subject, body, and reply guidance
- `{{loop.index}}` — which email we're on (0-based)
- `{{loop.total}}` — total emails needing replies

### Stage 4: Draft Gate (Human Review)

Execution **pauses**. You see all drafted replies in a numbered list. For each draft, you can:

- **Approve** — keep as-is
- **Edit** — adjust tone, add context, fix details
- **Remove** — skip this reply
- **Abort** — don't save any drafts

### Stage 5: Save Drafts

Approved drafts are saved to Gmail using `draft_email`. They appear in your Gmail Drafts folder for final review and sending.

**No emails are sent automatically.** You send them from Gmail when ready.

## Error Handling

- If Gmail is unreachable, abort and report the error
- If no emails need replies, skip the loop and report "inbox clear"
- If a specific draft fails to save, log the error and continue with remaining drafts
- The loop caps at 50 iterations to prevent runaway processing

## Inputs

| Name | Required | Description | Example |
|------|----------|-------------|---------|
| `{{input.lookback_hours}}` | No | Hours of inbox to scan. Default: 24. | `12` |
| `{{input.max_emails}}` | No | Maximum emails to process. Default: 20. | `30` |

## Outputs

| Name | Description |
|------|-------------|
| Draft replies | Saved to Gmail Drafts folder, ready for review and sending |
| Inbox summary | Categorized overview: needs reply, FYI, archive, spam counts |

## Setup

1. **Gmail MCP server** — OAuth 2.0 with `gmail.modify` scope (required for draft creation)
2. **Authorise** — on first run, authorise the MCP server to access your Gmail

## Provider Notes

- Categorization is a single LLM call — fast on any model.
- Reply drafting runs once per email in the loop — token usage scales linearly with email count.
- A model with strong writing capabilities produces better voice-matched replies.

## Example Input

```
Lookback hours: 24
Max emails: 20
```
