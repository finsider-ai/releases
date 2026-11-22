# Finsider tool guide

Use the live MCP schema for exact arguments. All workspace and opaque-handle
access is checked for the authenticated Finsider user. Classification below
matches the gateway capability registry: read tools may be called to gather
evidence; write and destructive tools prepare a confirmation before execution.

| Tool | Classification | Use and interpretation |
| --- | --- | --- |
| `get_account_status` | read | Resolve account setup and returned handoffs. |
| `list_workspaces` | read | Find workspaces accessible to this account. |
| `get_workspace_context` | read | Cheap orientation: the workspace record and its briefing of transaction counts, last activity, statement sources and jobs. Reads no statements. |
| `assess_engagement` | read | Assess deterministic readiness and blocking evidence. detail summary, the default, returns counts, statuses, handles and an appLink per task. Use detail full only when you need raw evidence payloads. |
| `ask_fin` | read | Use reportingContext with startDate/endDate and basis reported, adjustments_only, or adjusted; reuse continuationHandle for follow-ups. Recommendations remain review-only. |
| `get_pnl` | read | Dates use startDate/endDate. layer is original or adjusted, default original. adjustToggle is a deprecated alias where 1 or 0 means original and 3 means adjusted. |
| `get_financial_schedule` | read | Read cash_flow, working_capital, trial_balance, proof_of_cash, accounts_receivable, accounts_payable, accounting_transactions, bank_transactions, adjustments, or chart_of_accounts. Only the first three support adjusted basis. Other schedules require original and return native/null provenance except original accounting_transactions. chart_of_accounts has no period filter. Use page/pageSize and follow pagination.nextPage without changing scope. |
| `get_balance_sheet` | read | endDate selects the as-of month. Use original_transactions or adjusted_transactions explicitly. bridge_transactions is rejected because the gateway cannot return bridge-only figures. |
| `get_data_connection_status` | read | Inspect source connection readiness. |
| `get_financial_sync_status` | read | Check source processing and freshness. |
| `list_verification_runs` | read | Discover available runs rather than guessing handles. |
| `get_verification_run_by_handle` | read | Read an authorized opaque run handle and findings. |
| `get_verification_run` | read | Legacy numeric run ID lookup; supplied result limits can truncate findings. |
| `get_pnl_identities` | read | Read deterministic monthly arithmetic checks with source provenance. Takes the same layer input as get_pnl. |
| `get_balance_sheet_checks` | read | Returns the same backend payload as get_balance_sheet for the endDate month. The checks are the backend's verified flag, verificationNote and GrandTotal balance line. The gateway computes no tie-out of its own, so do not invent tests missing from it. |
| `get_discrepancies` | read | Read open, kept, or removed discrepancy findings. |
| `get_reconciliation_summary` | read | Read existing source-deletion reconciliation evidence. |
| `get_financial_upload_status` | read | Follow ingestionHandle; awaiting_confirmation requires review of warnings. |
| `get_operation_status` | read | Follow operationHandle across prepared, executing, succeeded, failed, expired, or outcome_unknown. |
| `get_report_status` | read | Follow reportHandle; ready is required before retrieving the download. |
| `get_report_download` | read | Request a private, short-lived download URL for a ready report. |
| `generate_qoe_memo` | read | Compatibility summary with an exact-period verification gate. |
| `list_adjustment_candidates` | read | QoE adjustment candidates as the backend stores them, each with reviewStage recommended, proposed, accepted, rejected or not_quantified. Filter by reviewStage and optional dates, and page with page/pageSize. Decide one with decide_adjustment. |
| `list_data_room_documents` | read | Documents in the workspace data room as the backend lists them. |
| `get_deal_inputs` | read | The saved deal scenario assumptions as stored. Do not recompute outputs. |
| `get_valuation_scenarios` | read | Saved valuation scenarios and their backend outputs as stored. |
| `get_forecast_inputs` | read | The saved forecast draft assumptions as stored. |
| `list_forecast_runs` | read | Forecast projection runs and their backend outputs as stored. |
| `create_workspace` | write | Prepare creating a business in the authorized home organization. |
| `start_accounting_connection` | write | Prepare a connection handoff; an unavailable integration requires the returned fallback. |
| `create_financial_upload_url` | write | Prepare fileName, contentType, byteLength, sha256, and supported type. |
| `complete_financial_upload` | write | Submit a file already transferred to the scoped upload URL. |
| `trigger_financial_sync` | write | Prepare a new financial sync for the selected workspace. |
| `trigger_verification_run` | write | Prepare a run; periodStart/periodEnd specify the requested period. |
| `scan_discrepancies` | write | Start a scan that records findings; this is not a read-only preview. |
| `review_discrepancy` | write | Record keep/remove for one findingKey; use the real reviewer's identity. |
| `request_fdd_datapack` | write | Prepare a data pack; use explicit paired dates for a scoped engagement. |
| `request_diligence_report` | write | Prepare the report for startDate/endDate after verification. |
| `decide_adjustment` | write | Approve or reject one candidateId from list_adjustment_candidates, with an optional reason recorded as the rationale. The preview names the candidate's label, amount and period as the backend stores them. A needed offsetting account or a possible duplicate comes back as a refusal with the Finsider link, so send the reviewer there and never retry with guessed values. Finsider can reopen a decision by reverting it to Recommended. |
| `create_data_request` | write | Create a seller data request link, emailed to recipientEmail when given. The link appears once, in the confirmed result as dataRequest.url. It is a bearer credential, so share it only with the intended seller and never post it elsewhere. get_operation_status returns only a datarequest_ handle. |
| `confirm_financial_upload` | destructive | Insert uploaded rows, including warned rows, after specific approval. |
| `cancel_financial_upload` | destructive | Cancel the selected upload job. |
| `remove_all_discrepancies` | destructive | Bulk removal requires the exact requiredPhrase as confirmation_phrase. |
| `reconcile_deletions` | destructive | Applies source deletion changes; apply:false is unsupported on this compatibility surface. |

Dates are ISO calendar dates. Every workspace tool returns `open_in_finsider`, a link to the
matching page in the Finsider app, and invalid arguments return `VALIDATION_ERROR` with the
failing `issues` paths and messages. Retain nulls and unknowns. Never treat missing
rows as financial zeroes. A successful mutation can return a resource handle
whose processing is still ongoing; use the appropriate status tool to verify
the intended result before proceeding.
