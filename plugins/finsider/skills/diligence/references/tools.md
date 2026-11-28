# Finsider tool guide

Use the live MCP schema for exact arguments. All workspace and opaque-handle
access is checked for the authenticated Finsider user. Classification below
matches the gateway capability registry: read tools may be called to gather
evidence; record tools write only the calling agent's own session on the
Actions board; write and destructive tools prepare a confirmation before execution.

| Tool | Classification | Use and interpretation |
| --- | --- | --- |
| `get_account_status` | read | Resolve account setup and returned handoffs. |
| `list_workspaces` | read | Find workspaces accessible to this account. |
| `get_workspace_context` | read | Establish workspace context before analysis. |
| `assess_engagement` | read | Assess deterministic readiness and blocking evidence. |
| `ask_fin` | read | Use reportingContext with startDate/endDate and basis reported, adjustments_only, or adjusted; reuse continuationHandle for follow-ups. Recommendations remain review-only. |
| `get_pnl` | read | Dates use startDate/endDate; adjustToggle 1 is reported/original and 3 is adjusted (0 is a legacy original alias). |
| `get_financial_schedule` | read | Read cash_flow, working_capital, trial_balance, proof_of_cash, accounts_receivable, accounts_payable, accounting_transactions, bank_transactions, adjustments, or chart_of_accounts. Only the first three support adjusted basis. Other schedules require original and return native/null provenance except original accounting_transactions. chart_of_accounts has no period filter. Use page/pageSize and follow pagination.nextPage without changing scope. |
| `get_balance_sheet` | read | endDate selects the as-of date. Use original_transactions or adjusted_transactions explicitly. |
| `get_data_connection_status` | read | Inspect source connection readiness. Only a direct QuickBooks connection reads as connected. |
| `get_financial_sync_status` | read | Check source processing and freshness. |
| `list_verification_runs` | read | Discover available runs rather than guessing handles. |
| `get_verification_run_by_handle` | read | Read an authorized opaque run handle and findings. |
| `get_verification_run` | read | Legacy numeric run ID lookup; supplied result limits can truncate findings. |
| `get_pnl_identities` | read | Read deterministic monthly arithmetic checks with source provenance. |
| `get_balance_sheet_checks` | read | Read the backend balance sheet/check payload; do not invent tests missing from it. |
| `get_discrepancies` | read | Read open, kept, or removed discrepancy findings. |
| `get_financial_upload_status` | read | Follow ingestionHandle; awaiting_confirmation requires review of warnings. |
| `get_operation_status` | read | Follow operationHandle across prepared, executing, succeeded, failed, expired, or outcome_unknown. |
| `get_report_status` | read | Follow reportHandle; ready is required before retrieving the download. |
| `get_report_download` | read | Request a private, short-lived download URL for a ready report. |
| `generate_qoe_memo` | read | Compatibility summary with an exact-period verification gate. |
| `create_workspace` | write | Prepare creating a business in the authorized home organization. |
| `create_financial_upload_url` | write | Prepare fileName, contentType, byteLength, sha256, and supported type. |
| `complete_financial_upload` | write | Submit a file already transferred to the scoped upload URL. |
| `trigger_financial_sync` | write | Prepare a new sync for a directly connected QuickBooks company. Any other company is refused with a link to connect QuickBooks or upload a general ledger in Finsider. |
| `trigger_verification_run` | write | Prepare a run; periodStart/periodEnd specify the requested period. |
| `scan_discrepancies` | write | Start a scan that records findings; this is not a read-only preview. |
| `review_discrepancy` | write | Record keep/remove for one findingKey; use the real reviewer's identity. |
| `request_fdd_datapack` | write | Prepare a data pack; use explicit paired dates for a scoped engagement. |
| `request_diligence_report` | write | Prepare the report for startDate/endDate after verification. |
| `confirm_financial_upload` | destructive | Insert uploaded rows, including warned rows, after specific approval. |
| `cancel_financial_upload` | destructive | Cancel the selected upload job. |
| `remove_all_discrepancies` | destructive | Bulk removal requires the exact requiredPhrase as confirmation_phrase. |
| `list_actions` | read | List a company's open engagements and their steps (Actions). Use the stepId of the step you are working on. |
| `start_agent_session` | record | Put this agent session on the Actions board as the signed-in person: provider (claude, codex or cursor), your externalId and a title, linked to an Action by workspaceId, engagementId and stepId. Report again with the same externalId to update. |
| `update_agent_session` | record | Report status (working, needs_input, finished, failed), a summary, the pull request, or move the session to another Action. Finishing never ticks the step; the team moves it to Done. |

Dates are ISO calendar dates. Retain nulls and unknowns. Never treat missing
rows as financial zeroes. A successful mutation can return a resource handle
whose processing is still ongoing; use the appropriate status tool to verify
the intended result before proceeding.
