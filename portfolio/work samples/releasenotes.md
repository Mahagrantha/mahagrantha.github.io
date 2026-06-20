# La Poche API Documentation — Release Notes

**Release:** Documentation restructuring
**Date:** June 2026
**Files affected:** `La_Poche_API_Documentation.html` (and companion `.docx` export)

## Summary

The API reference has been reorganized so every endpoint follows one consistent, predictable format. Previously, endpoints showed whatever subset of details applied (some had sample requests, some didn't; error handling was only documented once, globally). Now each endpoint is fully self-contained.

## What changed

### New per-endpoint structure
Every endpoint now includes, in this fixed order:

1. **Title** — short endpoint name (e.g. "Create product")
2. **Overview** — one-line description of what the endpoint does
3. **HTTP Method** — GET / POST / PUT / DELETE
4. **Endpoint** — the path, e.g. `/products/{id}`
5. **Request Parameters** — table of query-string or body fields, with type and description
6. **Sample Request** — example `curl` call where applicable
7. **Response Parameters** — table of fields returned in a successful response
8. **Success Response** — the relevant status code (200, 201, or 202) with a plain-language description, plus a sample JSON response where applicable
9. **HTTP Error Codes** — table of relevant error codes (400, 401, 403, 404, 409, 429, 500) with meaning, scoped to that specific endpoint instead of a single shared list
10. **Additional Error Notes** — endpoint-specific error codes not covered by the standard table (e.g. `sku_conflict`, `email_conflict`, `invalid_status_transition`), with sample error payloads

### Coverage
All 18 endpoints across the 8 resources were restructured:

- **Products** (4): list, create, update, deactivate
- **Orders** (4): list, retrieve, update status, generate invoice
- **Inventory** (3): get stock level, adjust stock, set reorder threshold
- **Marketplaces** (2): list channels, trigger sync
- **Finance & Invoices** (2): retrieve invoice, list settlements
- **Reports** (2): request report, check report status
- **Users** (3): list, create, deactivate

Webhooks remains a reference section (event table + payload example) rather than a request/response endpoint, so it wasn't restructured into the same format.

### What stayed the same
- Overview, Authentication, and Errors & Rate Limits chapters are unchanged in content.
- Visual design, color palette, and sidebar navigation are unchanged.
- Base URL, versioning policy, and rate limit values (120 req/min) are unchanged.

## Why this change

- Makes every endpoint scannable in isolation — a developer integrating against `POST /orders/{id}/invoice` no longer needs to cross-reference the global error table to know what 404 means in that context.
- Surfaces endpoint-specific errors (e.g. `invalid_status_transition` on order status updates) that were previously undocumented at the endpoint level.
- Produces a format that converts cleanly to other outputs (Word/.docx) without losing structure.

## Formats delivered

- `La_Poche_API_Documentation_Restructured.html` — same visual theme as the original, restructured content
- `La_Poche_API_Documentation_Restructured.docx` — Word version of the same restructured content

## Known limitations / follow-ups

- Some endpoint-specific error codes (e.g. `sync_in_progress` on marketplace sync) were inferred to fill out the standard format and should be verified against the actual API behavior before publishing externally.
- Pagination details (e.g. on `GET /settlements`) were not in the original source and may need to be added if that endpoint supports paging.