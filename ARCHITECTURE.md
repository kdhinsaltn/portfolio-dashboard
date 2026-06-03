# Architecture — Portfolio Dashboard

This document explains, in plain language, how the dashboard described in [SPEC.md](SPEC.md) is put together. It's written for a non-technical owner. The guiding principle is **keep it simple** — no heavy frameworks, no moving parts we don't need.

---

## The big picture

There are three pieces:

1. **A place to store the data** — the quarterly metrics and the charts users have built
2. **A small server** — answers questions like "give me this company's metrics" and "save this new chart"
3. **A webpage** — what the portfolio manager actually sees and clicks on

That's it. Three pieces, talking to each other.

```
  [ Webpage in browser ]  <-->  [ Small server ]  <-->  [ Data store ]
```

---

## 1. Where the data lives — SQLite

**What it is:** A single file on disk that acts as a database. Think of it like a spreadsheet file, but designed to be queried quickly and safely.

**Why this choice:**
- It's just one file — easy to back up (copy the file), easy to inspect, easy to move
- No separate "database server" to install, configure, or pay for
- Plenty fast for the scale we're talking about (one portfolio company, charts and quarterly numbers)

**Tradeoff:** If this product grows to thousands of companies with many simultaneous users, we'd outgrow SQLite and move to something like Postgres. That's a known, well-trodden upgrade path — not a trap.

We'll store two kinds of things:
- **Metrics** — the quarterly numbers the portfolio manager has submitted (revenue, EBITDA, cash position, etc.)
- **Charts** — the visuals the user has built (title, chart type, which metrics it shows)

---

## 2. The small server — plain Node.js

**What it is:** A small program that runs in the background. When the webpage asks "what are this company's metrics?", the server reads from the database and answers. When the user creates a chart, the server saves it.

**Why this choice:**
- Node.js comes with a built-in web server — no framework like Express or Next.js needed for something this small
- One language (JavaScript) across the server and the webpage means less context-switching
- Easy to run locally, easy to deploy to almost any hosting provider later

**Tradeoff:** Without a framework, we write a few more lines of "plumbing" ourselves (routing requests, parsing JSON). For an app of this size that's a small price for keeping the stack understandable. If the API grows past ~10–15 endpoints, adding a lightweight framework later is straightforward.

---

## 3. The webpage — plain HTML, CSS, and JavaScript

**What it is:** A single webpage the portfolio manager opens in their browser. It shows the dashboard of charts and the controls to create, edit, and delete them.

**Why this choice:**
- No React, Vue, or build step — just files the browser opens directly
- A non-technical owner can open the HTML file in any editor and read what it does
- Faster to load, fewer dependencies to keep up to date

**For the actual charts:** we'll use **Chart.js**, a small, well-maintained library that draws bar and line charts. It's a single file you include in the page — no framework, no build pipeline. It handles multiple metrics on one chart out of the box, which the spec requires.

**Tradeoff:** As the interface grows (drag-and-drop reordering, resizing — the v2 features in the spec), plain JavaScript starts to feel clunky. At that point, introducing a small framework like Svelte or Preact is a reasonable next step. We're not painting ourselves into a corner — we're just not paying that cost yet.

---

## How a typical action flows

**The user creates a new chart:**

1. They click "New Chart" on the webpage, pick "Bar," choose Revenue and EBITDA, give it a name, hit Save
2. The webpage sends that information to the server
3. The server writes a new row into the `charts` table in the SQLite file
4. The webpage refreshes the dashboard and the new chart appears

**The user opens the dashboard:**

1. The webpage asks the server: "what charts exist?"
2. For each chart, it asks: "give me the metric values for this chart"
3. Chart.js draws each one in its grid card

---

## What we are explicitly NOT adding right now

- **No user accounts / login system.** The spec describes a single role (portfolio manager) inside one company. Authentication can be bolted on later when there's a real need.
- **No cloud hosting decisions.** This runs on any laptop or basic server. We pick a host when we're ready to share it with others.
- **No framework on the frontend.** Until we need drag-and-drop and resizing, plain HTML and JavaScript is enough.
- **No separate "API layer" or microservices.** One server, one database, one page. Splitting things up before we need to is a common and expensive mistake.

---

## Summary of tradeoffs

| Choice | Why now | When we'd revisit |
|---|---|---|
| SQLite for storage | Zero setup, one file, fast enough | Many companies + many concurrent users |
| Plain Node.js server | No framework overhead, easy to read | API grows past ~10–15 endpoints |
| Plain HTML/JS frontend | No build step, easy to inspect | When v2 interaction features (reorder, resize) get painful |
| Chart.js for visuals | Small, focused, does exactly what the spec needs | If we need chart types it doesn't support |

The theme: **pick the simplest thing that meets the spec, and only add complexity when something concrete forces us to.**
