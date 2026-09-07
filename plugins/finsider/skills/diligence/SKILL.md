---
name: diligence
description: Perform financial diligence in an authenticated Finsider workspace, assess evidence and verification gaps, explain quality of earnings, and prepare reports for professional review. Use for Finsider engagement, financial statement, reconciliation, and diligence requests.
---

# Finsider diligence

Use the connected Finsider MCP tools. The host model conducts the conversation;
`ask_fin` invokes Finsider's server-side Fin analyst. A plugin does not change
the host's model. Financial values and verdicts must come from Finsider's
deterministic results, with source, period, layer, and freshness intact.

Read [the tool guide](references/tools.md) when choosing a tool or interpreting
an operation. Discover the live tool schemas before calling them; host prefixes
vary. If a required tool is absent, identify the missing capability and use
available read tools to collect evidence. Do not invent a tool or claim that an
unsupported action ran.

## Establish the engagement

1. Use `get_account_status` and `list_workspaces` to identify accessible targets.
   Resolve the user's business against that list. Ask only if multiple targets
   fit or no authorized target is available. Never infer a workspace ID or use
   an ID from another account. If account setup is incomplete, use the returned
   Finsider handoff.
2. Establish the requested date range, reporting currency, and original or
   adjusted statement layer. Read `get_workspace_context` for the selected
   workspace. Use dates from the user or returned context and state any default;
   ask when the requested period cannot be resolved. Do not silently substitute
   a different period or layer to obtain a passing result.
3. Use `assess_engagement` for the chosen scope. Carry its returned blockers,
   evidence, verification state, and tasks into a short plan. Follow task
   dependencies and inspect each task's action arguments and requiresApproval.
   An assessment describes readiness; it is not an audit opinion.

## Gather and assess evidence

Read connection status, sync status, P&L, balance sheet, and relevant
discrepancies. Select `get_pnl_identities` for arithmetic tie-outs and
`get_balance_sheet_checks` for available balance sheet evidence. Keep sources,
retrieval times, and statement layers attached to findings. Missing evidence,
null values, empty results, and unavailable checks are unknowns, never zeroes
or passes.

Use `get_financial_schedule` for cash flow, working capital, trial balance,
proof of cash, receivables, payables, transactions, adjustments, and chart of
accounts when those schedules bear on the user's question or assessment tasks.
Carry the same explicit dates into each call. Only cash flow, working capital,
and trial balance support original/adjusted selection. Request the other
schedules with basis `original` and inspect the returned provenance: native
sources return a null basis, except accounting transactions whose basis is
known to be original. Chart of accounts is not period-filtered and returns a
null period. Never label native schedules adjusted or assume an unfiltered
account list belongs only to the requested dates.

Use page/pageSize to bound paginated reads (defaults 1/100, maximum pageSize
200). Follow pagination.nextPage with the same company, schedule, dates, and
basis before claiming complete coverage. Missing pagination metadata does not
establish that every record was returned.
Match adjustments to returned source evidence and distinguish proposed review
items from approved adjustments. Do not infer a schedule is complete because
its statement totals tie. Use the returned evidence to identify missing
periods, account mappings, cash support, and working-capital issues.

Use `list_verification_runs` to find the run for this workspace and exact period;
read it with `get_verification_run_by_handle`. Inspect completion, findings,
errors, skipped checks, tolerances, source coverage, and whether the run predates
the latest sync or approved change. A completed job does not by itself mean
verified financials. Do not derive a fleet or engagement accuracy percentage
from a mismatch counter. Use the returned deterministic assessment or verdict.

Use `ask_fin` for an evidence-based explanation or proposed investigation.
Supply reportingContext with the selected period and basis; reuse the returned
continuationHandle for follow-up questions in the same workspace.
Preserve its evidence references and review flags. Fin's prose and confidence
cannot override deterministic results or authorize changes. If the tool cannot
return evidence for a financial claim, explain the gap instead of filling it
with model arithmetic, estimated addbacks, or invented transaction detail.

## Prepare, approve, execute, and verify each change

Carry out only changes within the user's request. For each write or destructive
action, use the server's two-call confirmation protocol:

1. Call the exact action with its intended arguments and no `confirmation_token`
   to prepare it. Inspect the returned `confirmation_required` preview, including
   `summary`, `consequences`, `operationHandle`, `expiresAt`, and `requiredPhrase`
   if present. Verification preparation may wrap this under `confirmation` and
   return a resolved `period`; show that period too.
2. Present the concrete workspace, period or resource, and consequences to the
   user. Obtain explicit approval for that prepared action. If the preview
   requires `requiredPhrase`, the user must supply it. A broad request to finish
   diligence does not approve every discrepancy removal or source deletion.
3. After approval, call the same tool with the same substantive arguments and
   the returned token as `confirmation_token`. Supply `confirmation_phrase`
   only for a tool whose schema supports it. Do not expose tokens in the final
   answer, save them in documents, or reuse them for other actions. If the token
   expires or inputs change, prepare the revised action and obtain approval for
   that preview.
4. Read `get_operation_status` with the returned operation handle. Follow the
   resource handle into the matching verification, sync, upload, or report
   status tool. Poll with the server's retry guidance, or modest increasing
   delays when none is returned, and report progress while work is running.
   `succeeded` at dispatch can still mean the downstream job is processing.
5. On `outcome_unknown`, `OPERATION_STATE_UNKNOWN`, or an ambiguous timeout,
   read the existing operation and resource status. Do not create a second
   mutation to guess whether the first completed. Stop further writes when
   state cannot be resolved and provide the handle for support. On failure or
   expiry, report the reason and next supported step.

After a data change, reread the affected resource and reassess the engagement.
Request a fresh verification run, through this same approval flow, when prior
evidence is stale. Do not mark findings resolved merely because a write returned
success.

For uploads, the MCP tools prepare and finalize scoped upload URLs; they do not
transfer local file bytes. Use a host upload capability only when it can send
the actual supplied file safely to the returned destination, with matching
size and SHA-256. Otherwise hand the user to Finsider's upload flow. Read
validation warnings before preparing `confirm_financial_upload`.

Prefer reviewing a specific discrepancy with evidence. Bulk removal requires
its own explicit request, preview, and exact workspace-name phrase. For
`reconcile_deletions`, inspect the current reconciliation summary first; the
compatibility MCP surface does not implement a dry-run call with `apply: false`.

## Gate reporting and professional review

Re-run `assess_engagement` before conclusions or report generation. Require
current verification for the selected scope and the backend's report gate.
If verification is absent, running, failed, stale, incomplete, or blocked, return
the blocking findings and a concrete next action. Never switch periods, remove
findings, or lower tolerances to make a report available.

When the gate permits and the user requests it, prepare `request_fdd_datapack`
or `request_diligence_report`, obtain approval, execute, and poll
`get_report_status`. Retrieve `get_report_download` only when ready. A report
request or progress value is not a delivered report. Treat returned download
URLs as private, short-lived capabilities for this user; do not publish them or
send them to other people without explicit authorization.

Use `generate_qoe_memo` only as the backend's verification-gated compatibility
summary. It does not replace the full diligence report. Provide the actual
report link when available, cited findings, unresolved limitations, approved
changes, and remaining decisions. Identify the output as prepared for
professional review; do not claim an audit, certification, CPA sign-off, or
investment recommendation.
