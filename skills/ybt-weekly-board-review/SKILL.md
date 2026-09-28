---
name: ybt-weekly-board-review
description: "YourBoardroomToday weekly board review for Jewel Bespoke Build Ltd, the first pilot business. Reads the JewelBB Portal and Xero connectors and produces an evidence-backed weekly flash report: cash, money owed each way, project and commercial position, sales pipeline, risks and the actions and decisions the board needs. Use for the scheduled weekly review, 'run the board review', 'weekly flash' or 'how is Jewel doing this week'. Read-only: never writes to the portal or Xero and never sends anything. Not for single-record questions, valuation or invoice work, or contractual correspondence."
---

# YBT weekly board review: Jewel pilot

The weekly flash report from the YourBoardroomToday proposal (`projects/yourboardroomtoday/master-proposal.md`), run against Jewel Bespoke Build Ltd. The aim is a short, honest board-level view of the week: what changed, what matters, and what needs deciding, with every figure traceable to a record.

## Hard rules

1. **Read-only.** Call only read tools. Never call a tool whose description starts `WRITE:`, never `perform_action`, `add_todo`, `complete_todo`, `log_todo_progress`, `post_request_message`, `save_skill` or anything that sends email, raises in Xero or changes a record. If something needs doing, it goes in the report as a recommended action with an owner, for a person to approve.
2. **Figures come from tools, never from memory or the last report.** Every figure carries its source and as-at date. If a read fails or comes back empty, say so in the report as a data-quality exception. Never fill the gap with an estimate that looks like a fact.
3. **Follow the portal's own doctrine.** At the start of each run call `list_skills`, then `load_skill` for `jbb-second-brain`, `jpms-xero-allocation`, `jpms-cash-forecast`, `jpms-valuation-cycle` and `jpms-todo-brief`. Where they conflict with this skill on how to read a figure, the portal skill wins; note the conflict under Evidence.
4. **Portal beats Xero's own reports where the doctrine says so.** Aged payables and receivables come from the portal (`get_aged_payables`, `get_aged_receivables`), because Jewel deliberately holds purchase bills in draft until allocated and Xero's report undercounts. Xero is the source for P&L, balance sheet and bank position.
5. **Arithmetic is shown, not asserted.** Any total, difference or percentage you compute yourself shows its inputs. Prefer a figure the tool already totals over re-adding.
6. **Evidence, assumption and recommendation stay separate.** Observed changes are stated as facts with sources; causes you cannot see in the data are labelled "possible explanation"; recommendations name who decides.
7. **Use Jewel's words** (see `jbb-second-brain`): programme, valuation invoice, variation V72, work order WO-0045, Architect's Instruction. Plain UK English, lead with the position.
8. **Stay in scope.** No bank advice, finance broking or loan suggestions. Contractual disputes (notices, pay-less, loss and expense, claims against Jewel) are Nigel's; list them as items for him, don't analyse them.
9. **Jewel data stays in the Jewel pilot.** Do not copy Jewel figures into other clients' work, benchmarks, marketing or the GitHub repository.

## Run order

1. **Context.** `get_current_context` (as-at date, user). `get_connected_organisations` in Xero; confirm it is "Jewel Bespoke Build Ltd" and stop with an exception if not. Load the portal skills in rule 3.
2. **Period.** The review week is the seven days to the run date unless the person says otherwise. State the period and data cut-off at the top.
3. **Cash and money owed.**
   - Bank / cash position: Xero `get_cash_position`.
   - Owed to Jewel: portal `get_aged_receivables` (drafts included). Owed by Jewel: portal `get_aged_payables` (drafts included). Call out anything over 60 days and the largest balances.
   - Next weeks' cash: portal `get_weekly_cashflow_grid` (13-week plan). Report the weeks where net movement is most negative. Amounts are authoritative, timing is indicative (cash doctrine); say so.
4. **Profit.** Xero `get_profit_and_loss` for the month to date and the prior month, and `get_financial_position` for the balance sheet. Note that project costs only reach a project once allocated; if the allocation queue is large, profit is qualified.
5. **Allocation and books quality gate.** Portal `list_xero_ledger_lines` with no status and read its `tabBar` (`toCode`, `workOrderBills`, `labourOutstanding`, `awaitingAction`), never the raw unallocated count. `get_xero_cost_code_option_gaps` for mapping gaps. Anything outstanding here is a data-quality exception that qualifies the finance section.
6. **Projects.** `list_projects`, then for each live project (LiveDelivery, CloseOut, DefectsPeriod):
   - `list_valuations` for the latest claim, its status and what is certified or awaiting payment;
   - `list_variations` for variations Issued or Awaiting AI (value waiting on approval);
   - `get_programme` only where needed to flag slippage against the latest baseline or Liquidated Damages claims;
   - `get_package_reconciliation` for margin (forecast buying gain) where packages exist.
   One line per project: stage, headline commercial position, the one thing that matters this week.
7. **Sales pipeline.** `list_leads` and `list_sales_strategies`: new leads this week, estimates due or submitted, wins and losses, total value in play where the portal gives it.
8. **Risk and compliance.** `list_lapsed_cover_on_site` (subcontractors on site with lapsed insurance), `list_compliance_register`, `list_hs_audits` (latest audit outcome and open actions), `list_defects` on projects in DefectsPeriod.
9. **Actions.** `get_todo_brief` across all projects. Report counts, overdue items first, and anything owned by the Managing Director or Finance Director. Use the portal's `nextStep` wording.
10. **Compare with last week** only if the previous report is available in this conversation or the person supplies it. Otherwise say "no prior report to compare against"; do not guess movements.

Skip a step that fails, record it as an exception, and carry on. A partial report with visible gaps beats no report or a complete-looking one with invented figures.

## Report shape

Keep it to what a director can read in five minutes. Use tables for figures.

1. **Headline**: three to five bullets: the position, the biggest change, the biggest risk, the decision needed.
2. **Cash and money owed**: bank, receivables and payables (aged), the 13-week low point.
3. **Profit**: month to date against prior month; qualifications from the quality gate.
4. **Projects**: one row per live project.
5. **Sales pipeline**.
6. **Risks and compliance**: lapsed cover, H&S, defects, anything contractual flagged for Nigel.
7. **Actions and decisions**: overdue to-dos, and decision requests each with evidence, recommendation and who approves.
8. **Data-quality exceptions**: failed reads, allocation backlog, mapping gaps, stale data.
9. **Evidence**: for each headline figure, the tool, the record or report, and the as-at date.

Mark the report **Provisional** while any exception in section 8 affects a headline figure.

## Delivery

Return the report in the conversation. Do not email it, post it to the portal, create to-dos from it or commit it to GitHub; those need Nigel's say-so in that conversation. Schedule, cadence and recipients are not yet agreed (proposal: open decisions).

## Completion condition

Every figure in the report has a source and date; every failed or qualified read is listed as an exception; no action was taken in the portal or Xero; recommendations name who decides.

## Failure log

- 28 September 2026: skill created for the Jewel pilot. Not yet run against live data; tool coverage and report shape to be tuned after the first runs.
