---
type: skill
id: reply-drafting
title: Reply Drafting
description: "Drafts a reply to a single email in your voice — runs inside a for_each loop"
tags: [Production, Email]
connections:
  - target: llm-service
    type: runs_on
context_params:
  voice_profile:
    label: "Voice Profile"
    description: "Your email writing style — the reply will sound like you"
    required: false
  reply_tone:
    label: "Reply Tone"
    description: "Tone of the reply — Formal, Friendly, or Concise"
    default: "Friendly"
    required: false
  reply_length:
    label: "Reply Length"
    description: "How long the reply should be — Brief, Standard, or Detailed"
    default: "Brief"
    required: false
---

## Capability

Drafts a reply to a single email. This skill runs inside a for_each loop — it receives one email per iteration via `{{loop.item}}` and produces one draft reply.

## What It Does

1. Reads the email content: sender, subject, body, thread context
2. Identifies what the sender wants: a decision, information, confirmation, or follow-up
3. Drafts a reply that addresses all points raised
4. Matches the configured tone and length
5. Applies Voice Profile if set

## Tone Levels

- **Formal** — professional and structured. Suitable for external stakeholders and senior management.
- **Friendly** — warm and conversational. Suitable for colleagues and regular contacts.
- **Concise** — direct and minimal. Suitable for quick confirmations and simple answers.

## Length Levels

- **Brief** — 2–4 sentences. Answer the question, nothing more.
- **Standard** — 1–2 short paragraphs. Address all points with context.
- **Detailed** — full response with background and next steps.

## Outputs

One draft reply: subject line (Re: original), body text, recipient address.
