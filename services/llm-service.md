---
type: service
id: llm-service
title: LLM Service
description: "Language model service for email categorisation, reply drafting, and synthesis"
tags: [Production, Tested]
connections: []
metadata:
  serviceType: llm
  auth_type: api_key
---

## LLM Service

This skrpt uses a language model for email categorisation and reply generation. The LLM handles inbox triage, reply drafting (in a for_each loop), and draft review synthesis.

### Configuration

- **Temperature:** 0.3 for categorisation, 0.5 for reply drafting
- **Max tokens:** 2,000 per reply draft, 4,000 for categorisation, 6,000 for review synthesis

### Requirements

- A configured LLM provider in skrptiq settings
- Token quota scales with email count (each reply draft is a separate LLM call in the loop)
