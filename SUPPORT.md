# Deal Guard support

- **Email:** vtbusiness123@gmail.com
- **Response target:** within 2 business days (Asia/Jakarta, UTC+7).
- **Issues and questions:** you can also open an issue in this repository.

When you write, please include:

1. Your Atlassian site URL (for example `your-company.atlassian.net`).
2. The issue key of the deal and what you expected to happen.
3. The exact message Deal Guard showed, or a screenshot of the panel.

## Common questions

**The panel shows "SAMPLE DATA".** Neither cash page exists yet, so Deal Guard shows its
sample company and keeps Approve locked. Click **Create cash pages** in the panel (pick a
Confluence space first), then replace the sample numbers with your own.

**The panel says a cash page "was not found" or "could not be used".** Deal Guard needs
exactly one page titled `Deal Guard Cash Position` and one titled `Deal Guard Cash Policy`,
and the app must be installed for Confluence. The message names the problem, for example
a missing "Setting | Value" table, a row whose Type is not Bill or Invoice, a date that is
not YYYY-MM-DD, or a policy amount written in a different currency than the cash position.

**The panel says "Enter this deal's numbers".** The cash pages are fine; save the deal's
value, supplier cost and dates in the panel and the check runs.

**Approve is refused.** That is the validator working: the message shows the date cash
falls below the floor and by how much. Change the deal terms in the panel, or ask the
Deal Guard agent "How can this deal pass?".

**My MCP client lists no tools.** In Atlassian Administration → Connected apps →
Deal Guard, enable Tools on **both** the Jira and the Confluence installation, then
disconnect and reconnect the client.

**Deals do not appear in Rovo Search.** A site admin must connect "Deal Guard cash
verdicts" under Connected apps → Deal Guard → Connections. Deals appear after their
terms are saved or the issue is updated; indexing can take a few minutes.
