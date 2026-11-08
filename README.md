# Deal Guard for Jira

Deal Guard stops a sales deal from being approved in Jira when the company's cash cannot
carry it. Finance writes the cash policy once, on a Confluence page. Every check uses
deterministic arithmetic, and AI assistants can ask questions and propose fixes but can
never invent the numbers or get around the check.

> Deal Guard runs entirely on Atlassian (Forge, "Runs on Atlassian"). It sends no data
> outside your Atlassian cloud site. This version calculates against a built-in
> **synthetic sample scenario** (opening cash, one supplier bill, one customer invoice).
> It is meant for evaluation and does not connect to an ERP or bank.

## What it does

| Where | What you get |
|---|---|
| Jira issue panel **Deal Guard** | Verdict (BLOCKED / NEEDS APPROVAL / OK), the day-by-day cash walk, the lowest cash and the shortfall against the cash floor. The customer deposit can be changed here. |
| Jira workflow validator **Deal Guard cash check** | Refuses the transition you attach it to (for example "Approve deal") when cash would fall below the floor, and explains why with the numbers. |
| Confluence page **Deal Guard Cash Policy** | Finance's minimum cash balance and maximum customer deposit. A change on the page applies on the next check. |
| Rovo agent **Deal Guard** | Answers "Can DEAL-1 be approved?" and "How can this deal pass?". Every proposal is re-checked by the engine; terms that would still be blocked are never saved. |
| MCP server (Rovo MCP) | The same three tools (`check-deal`, `propose-deal-fixes`, `apply-deal-terms`) for MCP-compatible AI clients, after a site admin enables them. |
| Teamwork Graph connector **Deal Guard cash verdicts** | Publishes each deal with its cash verdict as a Deal object, so Rovo Search and Rovo Chat can find, for example, every deal blocked by cash. |

## Set up (about 10 minutes)

1. **Install** Deal Guard on your site for Jira, and also for Confluence (the cash
   policy lives in Confluence).
2. **Create the cash policy page** in any Confluence space. Title it exactly
   `Deal Guard Cash Policy` and add a two-column table:

   | Setting | Value |
   |---|---|
   | Minimum cash balance | Rp5.000.000 |
   | Maximum customer deposit | 40% |

   Deal Guard refuses to approve deals while the page is missing, duplicated or
   unreadable (it fails closed).
3. **Add the validator** to your deal workflow: Project settings → Workflows → edit the
   transition that approves a deal → Rules → add **Deal Guard cash check**.
4. **Open a deal issue.** The Deal Guard panel shows the verdict. Choose a customer
   deposit to see how the cash walk changes.
5. *(Optional)* **Rovo:** open Rovo chat and pick the **Deal Guard** agent.
6. *(Optional)* **MCP clients:** a site admin opens Atlassian Administration → Connected
   apps → Deal Guard → Details → Tools and enables them for the Jira and the Confluence
   installation. The endpoint is `https://mcp.atlassian.com/forge/1153023e-26d0-454b-8571-8677330270f0`
   (OAuth 2.1; each call runs with the signed-in user's permissions).
7. *(Optional)* **Teamwork Graph:** a site admin opens Connected apps → Deal Guard →
   Connections → Teamwork Graph connectors and connects **Deal Guard cash verdicts**.

## How a check works

- The deal terms (customer deposit and the day the supplier is paid) are saved on the
  Jira issue as the entity property `deal-guard-terms`.
- The engine walks every cash event day by day (outflows before inflows on the same day,
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
| `read:page:confluence` | Read the "Deal Guard Cash Policy" page. |
| `write:object:jira`, `read:object:jira`, `delete:object:jira` | Publish deal verdicts to Teamwork Graph when an admin connects the connector. |
| `storage:app` | Remember the Teamwork Graph connection id. |

See [PRIVACY.md](PRIVACY.md) for data handling, [TERMS.md](TERMS.md) for the terms of use and [SUPPORT.md](SUPPORT.md) for help.
