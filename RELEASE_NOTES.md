# Release Notes

## v1.0.16
GH#833 — drop the embedded `gmail-mcp` service node; dep-reference `_shared/gmail-mcp@1.0.0`. contents.services 1→0.

## v1.0.15
GH#745 — declare per-step `output: {name, type}` on every execution step (inbox_scan/text, categories/text, draft_replies/text, gate_decision/decision, sent_confirmation/text). Lights up the #744 rich flow-map. Content-only; no bindings or logic changes.

## v1.0.14
GH#645 Row 3 final — re-pin 1 prompt-dep to the new v1.0.2/v1.0.3 versions that now expose `nodes[].content` via /api/shared/<slug>/<v>/metadata (per GH#651 endpoint extension + d1Execute dual-mode fix). Engine validator's dep-aware loop-body `{{loop.item}}` interpolation check + binding from_step resolution now pass through deps for this consumer. No content changes; identity + dep-version repin only.

## v1.0.13
Fix-forward after Row 3b v1.0.12 publish failure. The v1.0.12 per-skrpt CI's "Register version with Hub API" step failed because the consumer's source `manifest.id` (49ec1574…) did not match the D1 catalog row's id (8dd844b6…) — a legacy drift from before Action 6 (`0bcc5ae0`) made publish-skrpt.mjs Step 2 INSERT use `manifest.id` for the D1 id column. v1.0.13 reconciles the source `manifest.id` to the catalog authoritative value (Row-5-equivalent for consumers) and republishes. Per Adj-1: no re-tag of v1.0.12; the orphaned GitHub release artefact stays inert (no D1 versions row, no consumer pinned it).

## v1.0.12
GH#645 Row 3b — migrate to K-037 dep-referenced schema. Strip 2 inline shared-content files and declare 2 hub-shared deps (UUID id + slug name + version + checksum from `gen-dep-checksums.mjs`). Internal slug references rewritten for E2 rename/mirror-drop pair(s): draft-reply→draft-reply-batch. Closes pre-Step-3 inline-vendoring for this bundle.

## v1.0.11
Wave 2: re-signed with canonical engine signing pipeline.

## v1.0.10
Tags migrated inline into manifest (GH#586). tags.yaml retired.

## v1.0.9
Bundle re-signed with canonical engine signing pipeline (Wave 2 migration).

## v1.0.8
Signature fix — RELEASE_NOTES.md now included in integrity checksum.

## v1.0.7
Initial catalog release with full structural and content-quality validation. All scanner checks pass.
