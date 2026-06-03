# Portfolio Dashboard — Visual Builder Spec

## Overview

A dashboard within a portfolio company's workspace that allows portfolio managers to create, edit, and delete chart-based visuals built from their own quarterly operating data. The primary use case is internal reporting — visualizing how key metrics trend and relate to one another over time.

---

## Goals

- Enable portfolio managers to self-serve chart creation without needing engineering support
- Support internal reporting by making quarterly operating data visually interpretable
- Allow comparison of multiple metrics (e.g., revenue vs. EBITDA) for a single company over time

---

## Users

**Portfolio Managers** — they both submit the data and build the visuals. No separate viewer or admin role in v1.

---

## Data

- Submitted quarterly by portfolio managers
- Operating metrics include: Revenue, Cash Position, EBITDA, and similar financial/operational KPIs
- Data is pre-submitted and stored before any visualization is created — the visual builder reads from existing submitted data, it does not collect new data

---

## Must-Have Features (v1)

### Visual Builder
- Create a new chart from submitted quarterly data
- Choose chart type: **bar** or **line**
- Select one or more metrics to plot on a single chart (e.g., Revenue + EBITDA on the same axis or dual-axis)
- X-axis is time (quarters)
- Edit an existing chart (change metrics, chart type, title)
- Delete a chart

### Dashboard
- Display all created charts in a grid
- Each chart is a self-contained card with a title

---

## Nice-to-Have (v2+)

- Reorder charts on the dashboard (drag-and-drop)
- Resize chart cards
- Additional chart types beyond bar and line
- PDF / export functionality
- Annotations on charts (e.g., flag a quarter with a note)

---

## Out of Scope

Nothing is explicitly excluded at this stage. Features not listed above are deferred, not ruled out.

---

## Decisions

- A single chart can display multiple metrics together
- No maximum on the number of charts per dashboard
- Users name their charts manually (no auto-generated titles)
