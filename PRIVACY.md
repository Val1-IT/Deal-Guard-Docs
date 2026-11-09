# Deal Guard privacy policy

Last updated: 9 November 2026

Deal Guard ("the app") is a Forge app for Atlassian Jira and Confluence, published by
Valent Tansuri ("we"). This policy explains what data the app touches and where it goes.

## Summary

- The app runs entirely on Atlassian's Forge platform ("Runs on Atlassian"). It makes
  **no calls to servers outside Atlassian** and we, the publisher, **cannot see your
  data**.
- The app does not sell, share or use your data for advertising or AI training.

## Data the app reads

- **Jira issues** you open the Deal Guard panel on, or that a workflow transition,
  the Rovo agent, an MCP client or the Teamwork Graph sync checks: summary, status,
  assignee/reporter account id, created date, and the Deal Guard terms property.
- **The Confluence pages titled "Deal Guard Cash Position" and "Deal Guard Cash Policy"**:
  their table values (currency, cash on hand, the bills and invoices listed with their
  amounts and due dates, minimum cash balance, maximum customer deposit), version and
  link. The app reads them inside your site and stores no copy.
- **The list of Confluence spaces**, only when someone uses **Create cash pages**, to pick
  where the two pages go.

## Data the app stores, and where

| Data | Where | Retention |
|---|---|---|
| Deal details and terms (deal value, supplier cost, accepted date, customer payment days, customer deposit %, supplier payment day) | Jira issue entity property `deal-guard-terms` on your site | Until you delete the issue or the property |
| The two cash pages, **only when someone clicks Create cash pages** | Your Confluence space, as normal pages with sample numbers | Until you delete them |
| Teamwork Graph connection id | Forge app storage (Atlassian-hosted) | Until the app is uninstalled |
| Deal objects (issue key and summary, Jira status, deal amount, cash verdict, lowest cash, cash floor, shortfall, deposit, supplier day, link to the issue, owner's Atlassian account id) | Atlassian Teamwork Graph, **only if a site admin connects the connector** | Deleted by Atlassian when the admin disconnects the connector |

The cash figures come from the Confluence pages finance writes, not from your accounting
systems or bank. Until those pages exist, the app uses a built-in sample company, which
is labelled as sample data and can never approve a deal. (The Marketplace listing,
version 1.x, uses only the built-in sample scenario until its next update.)

## Logs

When a Teamwork Graph sync fails, the app writes a warning to Forge logs containing the
issue key and Atlassian's error message. If you allow log sharing in Atlassian
Administration, we can read these logs for up to 30 days to troubleshoot. You can turn
log sharing off at any time.

## Personal data

The only personal data the app processes is the Atlassian account id of a deal's
assignee or reporter, used to mark the deal owner in Teamwork Graph. Atlassian processes
it under the Atlassian Customer Agreement.

## Your choices

- Disconnect the Teamwork Graph connector to stop publishing and remove the objects.
- Disable the MCP tools in Atlassian Administration to stop external AI client access.
- Uninstall the app to stop all processing.

## Contact

vtbusiness123@gmail.com
