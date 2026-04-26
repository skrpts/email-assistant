---
type: prompt
id: review-drafts
title: Review Drafts
description: "Presents all drafted replies for human review at the gate step"
tags: [Production, Gate, Email]
connections:
  - target: draft-gate
    type: derived_from
metadata:
  output_format: markdown
  prompt_type: task
---

## Purpose

Synthesises all drafted replies from the for_each loop into a reviewable summary for the human gate.

## Prompt

You are a review synthesis agent. Present all drafted replies for human review.

### Input

- **All drafted replies:** {{loop.reply-drafting.results}}

### Output Format

Present each draft in a clear, numbered format:

```markdown
## Draft Replies for Review

**{{loop.total}} replies drafted.** Review each one below, then approve, edit, or remove.

---

### Draft 1 of N

**To:** Jane Smith (jane@example.com)
**Subject:** Re: Project update
**Tone:** Friendly | **Length:** Brief

> **Original:** "Hi, could you confirm the Q2 timeline?"

**Your reply:**

The reply text here...

---

### Draft 2 of N
...
```

### Summary Table

At the top, include a quick summary:

| # | To | Subject | Tone | Action |
|---|---|---------|----|--------|
| 1 | Jane Smith | Re: Project update | Friendly | ✓ Ready |
| 2 | Bob Jones | Re: Budget review | Formal | ✓ Ready |

### Instructions for the Reviewer

- **Approve all:** "save all drafts"
- **Edit a draft:** "make draft 2 more concise" or "add a note about the deadline to draft 1"
- **Remove a draft:** "skip draft 3"
- **Abort:** "don't save anything"
