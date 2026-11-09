# Deal Guard for Jira

Deal Guard stops a sales deal from being approved in Jira when the company's cash cannot
carry it. Finance keeps the company's cash position and cash policy on two Confluence
pages; sales enters each deal's numbers in Jira. Every check uses deterministic
arithmetic, and AI assistants can ask questions and propose fixes but can never invent
the numbers or get around the check.

> Deal Guard runs entirely on Atlassian (Forge, "Runs on Atlassian"). It sends no data
> outside your Atlassian cloud site and does not connect to an ERP or bank: the cash
> figures are the ones finance writes on the Confluence pages. Until those pages exist,
> the panel shows a clearly labelled **sample company** so you can try it, and Approve
> stays locked.
>
> The Marketplace listing (Deal Guard 1.x, in review) still checks deals against the
> built-in sample scenario only; the company-data features below ship in its next update.
> The app shared through the Codegeist installation link has them today.

## What it does

| Where | What you get |
|---|---|
| Jira issue panel **Deal Guard** | The deal's details (value, supplier cost, accepted date, when the customer pays the rest, when the supplier is paid), the verdict (BLOCKED / NEEDS APPROVAL / OK), the day-by-day cash walk, the lowest cash and the shortfall against the cash floor. The customer deposit can be changed here. |
| Jira workflow validator **Deal Guard cash check** | Refuses the transition you attach it to (for example "Approve deal") when cash would fall below the floor, and explains why with the numbers. |
| Confluence page **Deal Guard Cash Position** | The company's cash on hand and the bills (money out) and invoices (money in) already due, in the company's currency. |
| Confluence page **Deal Guard Cash Policy** | Finance's minimum cash balance and maximum customer deposit. A change on either page applies on the next check. |
| Rovo agent **Deal Guard** | Answers "Can DEAL-1 be approved?" and "How can this deal pass?". Every proposal is re-checked by the engine; terms that would still be blocked are never saved. |
| MCP server (Rovo MCP) | The same three tools (`check-deal`, `propose-deal-fixes`, `apply-deal-terms`) for MCP-compatible AI clients, after a site admin enables them. |
| Teamwork Graph connector **Deal Guard cash verdicts** | Publishes each deal with its cash verdict as a Deal object, so Rovo Search and Rovo Chat can find, for example, every deal blocked by cash. |

## Set up (about 10 minutes)

1. **Install** Deal Guard on your site for Jira, and also for Confluence (the cash pages
   live in Confluence).
2. **Add the validator** to your deal workflow: Project settings → Workflows → edit the
   transition that approves a deal → Rules → add **Deal Guard cash check**.
3. **Create the two cash pages.** Open any deal issue: with no pages yet, the Deal Guard
   panel runs on the labelled sample company. Pick a Confluence space and click
   **Create cash pages**; Deal Guard creates both pages with sample numbers for finance
   to replace. You can also write them by hand, titled exactly as below (one page each,
   anywhere on the site):

   **Deal Guard Cash Position**: a settings table and a schedule table.

   | Setting | Value |
   |---|---|
   | Currency | IDR |
   | Cash on hand | Rp30.000.000 |
   | As of | 2026-11-09 |

   | Type | Name | Amount | Due date |
   |---|---|---|---|
   | Bill | Supplier invoice (existing order) | Rp18.000.000 | 2026-11-11 |
   | Invoice | Customer invoice (earlier order) | Rp20.000.000 | 2026-11-13 |

   A Bill is money out, an Invoice is money in. Use a three-letter currency code,
   whole amounts only, and dates as YYYY-MM-DD (or Confluence date chips).

   **Deal Guard Cash Policy**: one table with two rules, in the same currency.

   | Rule | Value |
   |---|---|
   | Minimum cash balance | Rp5.000.000 |
   | Maximum customer deposit | 40% |

   Deal Guard refuses to approve deals while a page is missing, duplicated or
   unreadable, or written in a different currency (it fails closed).
4. **Open a deal issue** and enter its value, supplier cost and dates in the Deal Guard
   panel, then save. The panel shows the verdict; choose a customer deposit to see how
   the cash walk changes.
5. *(Optional)* **Rovo:** open Rovo chat and pick the **Deal Guard** agent.
6. *(Optional)* **MCP clients:** a site admin opens Atlassian Administration → Connected
   apps → Deal Guard → Details → Tools and enables them for the Jira and the Confluence
   installation. The endpoint is `https://mcp.atlassian.com/forge/1153023e-26d0-454b-8571-8677330270f0`
   for the Marketplace app, or `https://mcp.atlassian.com/forge/44a473d0-07f3-459c-908f-a26941578c42`
   for the app from the Codegeist installation link (OAuth 2.1; each call runs with the
   signed-in user's permissions).
7. *(Optional)* **Teamwork Graph:** a site admin opens Connected apps → Deal Guard →
   Connections → Teamwork Graph connectors and connects **Deal Guard cash verdicts**.

## How a check works

- The deal's details and terms (value, supplier cost, dates, customer deposit and the day
  the supplier is paid) are saved on the Jira issue as the entity property
  `deal-guard-terms`.
- Each check reads both Confluence pages again, so a change by finance applies at once.
  Money is shown in the cash position's currency, exactly as the engine computes it;
  nothing is converted.
- The engine walks every cash event (cash on hand, the bills and invoices on the page,
  and this deal's deposit, supplier cost and customer balance) day by day (outflows before inflows on the same day,
  the conservative order) and finds the lowest balance.
- **BLOCKED**: cash falls below the floor, or the deposit is above the policy maximum.
  **NEEDS APPROVAL**: cash holds but the deal needs a commercial exception, which the
  person performing the approval transition grants. **OK**: no issue.
- Proposed fixes change one term at a time (a higher deposit up to the policy maximum,
  or a later supplier payment up to 60 days) and are shown only if the engine verifies
  them.

## Permissions (scopes) and why

| Scope | Why |
|---|---|
| `read:jira-work` | Read the deal issue (summary, status, owner) and its saved terms. |
| `write:jira-work` | Save the deal terms on the issue (always as the signed-in user, so Jira's edit permission applies). |
| `read:page:confluence` | Read the "Deal Guard Cash Position" and "Deal Guard Cash Policy" pages. |
| `write:page:confluence` | Create the two pages when someone clicks **Create cash pages** (never edits existing pages). |
| `read:space:confluence` | List the Confluence spaces to choose where **Create cash pages** puts the pages. |
| `write:object:jira`, `read:object:jira`, `delete:object:jira` | Publish deal verdicts to Teamwork Graph when an admin connects the connector. |
| `storage:app` | Remember the Teamwork Graph connection id. |

See [PRIVACY.md](PRIVACY.md) for data handling, [TERMS.md](TERMS.md) for the terms of use and [SUPPORT.md](SUPPORT.md) for help.
