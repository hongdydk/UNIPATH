---
name: universitypath-ui-ux-review
description: Review or change UniversityPath Flutter Web and Windows UI when work affects screens, layouts, navigation, forms, tables, AI panels, or detached chat windows. Do not use for backend-only changes.
---

# UniversityPath UI/UX Review

Use this skill for a visual or interaction change in the UniversityPath student
application. Its goal is to preserve a consistent desktop-first workspace while
keeping the Web version accessible at narrower widths.

## Source of truth

Before making UI decisions, read
[`docs/product-ux.md`](../../../docs/product-ux.md).
Treat it as the detailed product UX reference. It distinguishes current behavior
from additional recommendations; do not represent a recommendation as implemented
without actually adding and verifying it.

## Review criteria

- Use the available **app-window width**, not device or monitor resolution alone,
  for responsive decisions. Preserve the documented Windows minimum size and
  Web overlay behavior instead of allowing panels or content to overlap.
- Keep the AI assistant secondary: it starts closed, uses the right dock when
  there is room, and must not obscure the active task or keyboard focus. Preserve
  the documented detached-window/maximize behavior when modifying chat UI.
- Let normal text, records, and ordinary tables reflow. Restrict horizontal
  scrolling to comparison matrices whose column alignment conveys meaning, and
  keep every matrix header and data row in the same scroll container.
- Use clear Korean task labels, one local primary action, recognizable icons with
  accessible names, and visible non-color status labels. Keep Rule Engine
  graduation results distinct from AI/RAG explanation and document evidence.
- For input, loading, empty, error, and success states, retain the user's work
  and state the next action in text. AI suggestions may not change records until
  the user confirms them.
- Preserve keyboard navigation, visible focus, usable control targets, text
  scaling, high contrast, and system theme behavior. Do not introduce custom
  styling that hides focus or relies on color alone.

## Backend and AI RAG stay outside UI work

UI changes do not implement or alter these responsibilities:

- Academic Rule Engine, including personal and official fulfillment
- School Knowledge RAG and Career/Certificate RAG, including retrieval, filters, and `insufficient_evidence`
- LLM explanation of a judgment or of retrieved evidence
- FastAPI routes, schemas, database models, and migrations

The screen sends user input and renders a payload it has already received. It does not calculate graduation status, choose evidence, or fill a missing payload with sample domain data.

## Received data only

Do not hardcode courses, rules, audit rows, document citations, or AI answers as product data. When changing a screen, write only how each received field is shown:

- the response field behind each label, column, status, and empty state
- loading, error, and success from that payload and the API error shape
- a personal calculation label only when the received result says it is not an official judgment

Remove hardcoded sample rows on any screen being edited. If the client is not connected yet, show the empty and error states and name the fields that screen will bind.

## Verification

Choose checks proportional to the change. For layout, table, panel, or window
work, inspect the affected flow at the documented desktop sizes and narrow Web
width. For Flutter code changes, run `flutter analyze`, relevant widget tests,
and the affected Web or Windows build when available. Update the UX reference
when a verified UI policy or interaction rule changes; do not update it for a
one-off visual adjustment.
