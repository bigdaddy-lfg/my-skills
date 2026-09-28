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
   - Check the plan's income side before quoting its low point. If cash in is only invoices already raised, say plainly that the closing balance is not a forecast, and set it against the value still to be valued (revised contract sum less works complete, from step 6).
   - Reconcile: grid supplier bills + excluded entries should equal aged payables; the grid's opening balance against Xero's bank figure; the portal's non-draft payables against Xero's. Report any gap as an exception.
4. **Profit.** Xero `get_profit_and_loss` for the month to date and the same days of the prior month, and `get_financial_position` for the balance sheet. Draft bills do not reach the P&L, so a month with many drafts or an empty CIS labour line shows inflated profit: compare cost of sales with the prior month and qualify the profit line when it is clearly incomplete. Report odd balance-sheet signs (e.g. negative current assets) as exceptions for the accountant; don't interpret them.
5. **Allocation and books quality gate.** Portal `list_xero_ledger_lines` with no status and read its `tabBar` (`toCode`, `workOrderBills`, `labourOutstanding`, `awaitingAction`), never the raw unallocated count. `get_xero_cost_code_option_gaps` for mapping gaps. Anything outstanding here is a data-quality exception that qualifies the finance section.
6. **Projects.** `list_projects`, then for each live project (LiveDelivery, CloseOut, DefectsPeriod):
   - `list_valuations` for the latest claim, its status and what is certified or awaiting payment. A locked claim with no invoice is cash not yet asked for: flag it with days since lock, and check receivables to see whether it was raised outside the portal;
   - `list_variations` for variations Issued or Awaiting AI (value waiting on approval);
   - `get_programme` only where needed to flag slippage against the latest baseline or Liquidated Damages claims;
   - `get_package_reconciliation` for margin (forecast buying gain) where packages exist.
   One line per project: stage, headline commercial position, the one thing that matters this week.
7. **Sales pipeline.** `list_leads` and `list_sales_strategies`: new leads this week, estimates due or submitted, wins and losses, total value in play where the portal gives it.
8. **Risk and compliance.** `list_lapsed_cover_on_site` (subcontractors on site with lapsed insurance), `list_compliance_register` (status Expired, and note the register's Missing count: a clean lapsed-cover check only covers firms with certificates on file), `list_hs_audits` with projectId "all" (latest audit outcome, and whether it has been issued), `list_defects` on projects in DefectsPeriod.
9. **Actions.** `get_todo_brief` across all projects. Report counts, overdue items first, and anything owned by the Managing Director or Finance Director. Use the portal's `nextStep` wording.
10. **CFO advice.** Apply the `jbb-virtual-cfo` principles to this week's figures (see "CFO advice section" below).
11. **Compare with last week** only if the previous report is available in this conversation or the person supplies it. Otherwise say "no prior report to compare against"; do not guess movements.

Some reads (`get_aged_payables`, `get_todo_brief`) return more than fits in one tool result. Process the saved output with a script, read all of it, and total with code rather than by eye.

Skip a step that fails, record it as an exception, and carry on. A partial report with visible gaps beats no report or a complete-looking one with invented figures.

## CFO advice section

A short set of recommendations, not a repeat of the figures. **Three to five items**, most valuable first. Each item has:

- **The finding** it answers, pointing to the section and figure it comes from;
- **The action**, in one sentence, in Jewel's words;
- **An owner** (a role: MD, FD, QS, PM, Accounts);
- **A deadline** (a date, not "soon");
- **The £ at stake**, where the data gives one; otherwise say "not quantifiable from this week's data".

Run these standing tests every week and turn any failure into an advice item:

| Test | Fails when |
|---|---|
| Cash not asked for | Any locked valuation has no invoice, or retention is past due |
| Stretched creditors | The over-90-day payables balance rises week on week |
| Overhead run-rate | The last full month's Xero overheads (including Consulting) are above the board's target |
| Work-in-hand cover | Value still to be valued, divided by average monthly invoicing, is under 6 months, or new leads carry no value |
| Tax and loans | Any HMRC instalment or loan payment missed, bounced or late |
| Project health | Any live job where cost to date is higher than works value, or there's no budget to forecast against |
| Books fit to report | The allocation queue or draft bills would distort this month's profit |

Rules:

- Advice is for directors to act on. Never carry it out yourself: no writes, no emails, no to-dos.
- Don't give legal, tax or insolvency opinions. Where one is needed, the advice is "take advice from…".
- Don't repeat last week's advice word for word. If an item is still open, show how many weeks it has been open and whether it has got worse.

## Report shape

Keep it to what a director can read in five minutes. Use tables for figures.

1. **Headline**: three to five bullets: the position, the biggest change, the biggest risk, the decision needed.
2. **Cash and money owed**: bank, receivables and payables (aged), the 13-week low point.
3. **Profit**: month to date against prior month; qualifications from the quality gate.
4. **Projects**: one row per live project.
5. **Sales pipeline**.
6. **Risks and compliance**: lapsed cover, H&S, defects, anything contractual flagged for Nigel.
7. **Actions and decisions**: overdue to-dos, and decision requests each with evidence, recommendation and who approves.
8. **CFO advice**: the three to five recommendations above, as a table (finding · action · owner · deadline · £ at stake).
9. **Data-quality exceptions**: failed reads, allocation backlog, mapping gaps, stale data.
10. **Evidence**: for each headline figure, the tool, the record or report, and the as-at date.

Mark the report **Provisional** while any exception in section 9 affects a headline figure.

## Delivery

Produce the report as a self-contained HTML file in the session scratchpad, styled to Jewel Bespoke Build's brand (Bespoke Navy #1A1E29, Bespoke Orange #FF8300, Jewel Gold #C09A51; Switzer headings, Geom Graphic body, system fallbacks), with the logo embedded when supplied. Summarise the headline in the conversation. Do not email it, post it to the portal, create to-dos from it or commit it to GitHub; those need Nigel's say-so in that conversation. Schedule, cadence and recipients are not yet agreed (proposal: open decisions).

## Completion condition

Every figure in the report has a source and date; the CFO advice section has three to five owned, dated items; every failed or qualified read is listed as an exception; no action was taken in the portal or Xero; recommendations name who decides.

## Failure log

- 28 September 2026: skill created for the Jewel pilot.
- 28 September 2026, first test run: (1) a draft said "£2.4m still to claim" using total contract value instead of value remaining; caught before release. Remedy: remaining value is revised contract sum less works complete, computed in code. (2) The 13-week plan has no future valuation income, so its closing balance looked like a forecast; remedy added to step 3. (3) September P&L showed a 98.9% gross margin because costs were still in draft; remedy added to step 4. (4) Two reads exceeded tool output limits; remedy added to run order.
