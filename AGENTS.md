# AGENTS.md — Homesafe franchise application form

Inherit the universal instructions from `${AGENT_WORKBENCH:-$HOME/agent-workbench}/AGENTS.md`. This file is the project-specific delta.

## Review guidance

Codex PR review should stay high-signal and focus on P0/P1 issues:

- Flag correctness, security, privacy, data loss, authorization, migration, concurrency, billing, deployment, and user-visible workflow regressions.
- Check changed behavior against the closest `AGENTS.md`, existing project patterns, and the affected runtime workflow.
- Treat missing or misleading verification as a review issue when a change touches user-visible behavior, data writes, auth, jobs, billing, or deployment.
- Do not leave low-priority style comments unless they hide a real bug or future maintenance risk.

## Scope and source of truth

This repository is a single-page static franchise application form. `index.html` owns the UI, validation, HubSpot field mapping, submission endpoint, and success/error behavior.

Do not add a framework, build system, server-side datastore, analytics, or new lead-routing service unless explicitly requested. HubSpot internal property names and the configured form endpoint are integration contracts, not presentational details.

## Invariants

- Treat every submission as personal and commercially sensitive applicant data. Do not log, commit, screenshot, or persist real form contents.
- Keep required fields, conditional fields, declaration text, and payload property names aligned with the intended HubSpot form.
- A successful HTTP response is not proof that HubSpot accepted every field or routed the lead correctly; verify the named test record when authorized.
- Prevent duplicate submits, preserve clear failure/retry behavior, and keep the form usable by keyboard and on narrow screens.
- Do not replace the HubSpot portal/form ID or mix building-and-pest and strata applications without explicit approval.

## Commands and risk

### Local-safe

```bash
python3 -m http.server 8000
```

Exercise validation with fictitious data and inspect desktop/mobile layouts. Serving locally does not make a real HubSpot submission safe.

### External read-only

Inspecting the deployed page or a named HubSpot form/schema is read-only only when no form is submitted and no record is changed.

### External write

Submitting the form creates a HubSpot record. Use only an explicitly approved test identity/account. Endpoint changes, GitHub Pages publication, CRM routing changes, and production deployment require explicit current-turn approval naming the target.

### Destructive or irreversible

Deleting applicant records, bulk editing CRM fields, replacing the live form endpoint, or removing the deployed form requires explicit confirmation and a recovery/rollback plan.

## Verification

Validate required/conditional fields, invalid input, duplicate-click protection, network failure, success state, keyboard flow, and mobile layout. For an authorized integration test, confirm the exact test record and field mapping in HubSpot without exposing applicant data.
