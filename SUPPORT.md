# Deal Guard support

- **Email:** vtbusiness123@gmail.com
- **Response target:** within 2 business days (Asia/Jakarta, UTC+7).
- **Issues and questions:** you can also open an issue in this repository.

When you write, please include:

1. Your Atlassian site URL (for example `your-company.atlassian.net`).
2. The issue key of the deal and what you expected to happen.
3. The exact message Deal Guard showed, or a screenshot of the panel.

## Common questions

**The panel says "NO CASH POLICY".** Deal Guard could not use the Confluence page titled
`Deal Guard Cash Policy`. Check that exactly one page has that title, that the app is
installed for Confluence, and that the table has the rows "Minimum cash balance" and
"Maximum customer deposit" with readable values (for example `Rp5.000.000` and `40%`).

**Approve is refused.** That is the validator working: the message shows the date cash
falls below the floor and by how much. Change the deal terms in the panel, or ask the
Deal Guard agent "How can this deal pass?".

**My MCP client lists no tools.** In Atlassian Administration → Connected apps →
Deal Guard, enable Tools on **both** the Jira and the Confluence installation, then
disconnect and reconnect the client.

**Deals do not appear in Rovo Search.** A site admin must connect "Deal Guard cash
verdicts" under Connected apps → Deal Guard → Connections. Deals appear after their
terms are saved or the issue is updated; indexing can take a few minutes.
