# Portfolio Dashboard

A simple dashboard where portfolio managers can build and compare visuals from quarterly operating data they've submitted for a portfolio company. Intended for internal reporting.

## What it does

- Display a dashboard of charts for a single portfolio company
- Create, edit, and delete bar or line charts
- Plot one or more metrics (e.g. Revenue, EBITDA, Cash Position) on a single chart, with quarters along the X-axis
- Name each chart manually; no limit on how many charts a dashboard can hold

## Who it's for

Portfolio managers, who both submit the underlying quarterly metrics and build the visuals on top of them.

## Status

Early-stage spec. No code yet.

## Documents

- [SPEC.md](SPEC.md) — product spec: goals, users, must-haves, v2 ideas
- [ARCHITECTURE.md](ARCHITECTURE.md) — proposed architecture in plain language (SQLite + plain Node.js + plain HTML/JS + Chart.js)

## Out of scope (for v1)

- Reordering or resizing charts on the dashboard
- Chart types beyond bar and line
- Export / PDF
- Multi-user roles or authentication

See [SPEC.md](SPEC.md) for the full picture.
