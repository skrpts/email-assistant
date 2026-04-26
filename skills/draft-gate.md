---
type: skill
id: draft-gate
title: Draft Gate
description: "Human gate — pauses for you to review all drafted replies before they are saved to Gmail"
tags: [Production, Gate, Email]
connections:
  - target: llm-service
    type: runs_on
metadata:
  gate: true
---

## Capability

Pauses the workflow and presents all drafted replies for human review. You decide which drafts to keep, edit, or discard before they are saved to Gmail.

This is a **gate step** — execution pauses until you respond.

## What Happens

1. Execution pauses with status `awaiting_input`
2. You see all drafted replies in a table: original email summary, your drafted reply, and tone/length used
3. You respond with:
   - **Approve all** — "save all drafts" → all replies are saved to Gmail Drafts
   - **Edit** — "change draft 3 to be more formal" or "remove the second paragraph from draft 1" → drafts are adjusted
   - **Remove** — "skip draft 2" → that reply is not saved
   - **Abort** — "don't save anything" → no drafts are created
4. Your decisions feed into the draft sending step

## Why a Gate

Email replies should never be saved or sent without human review. The gate ensures you see exactly what will appear in your Gmail Drafts folder and can catch tone mismatches, incorrect assumptions, or missing context before the drafts are created.
