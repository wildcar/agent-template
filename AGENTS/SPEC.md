# <project> — functional & technical specification

Source of truth for *what the product does* and *how it is built*. Fill in after the
first run; keep it in sync with reality (update before code when the contract changes).

## Purpose

<one paragraph: what problem this solves and for whom>

## Stack

- <language / runtime>
- <framework(s)>
- <data layer / storage>
- <auth, if any>
- <deployment target>

## Architecture

```
<diagram or ASCII sketch: client → server → data, boundaries, who talks to whom>
```

## Functional requirements

<numbered or sectioned list of behaviors. Mark each ✅ done / ⏳ planned.>

## Project structure

```
<directory tree with one-line role per key path>
```

## Deployment

- <where it runs, how it is built and started, where secrets live>
- <pointer to a step-by-step install/runbook if one exists>

## Current state

- ✅ <what works>
- ⏳ <what is planned / in progress>

See `AGENTS/STATE.md` for the live Now / Next snapshot.

## Data sources & dependencies

- <external systems, scheduled jobs, upstream repos, or services this depends on>
