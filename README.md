# Form5500Lookup for Claude

![Form5500Lookup](assets/icon.png)

Form5500Lookup helps employee benefits brokers see who holds an account and when it renews. Ask Claude about an employer, and it reads the company's public Form 5500 filing for you: the broker of record, the carrier on each line of coverage, how many people each contract covers, and the date the policy year ends.

## What you can ask

- **Check an employer.** "Who's the broker on Acme Manufacturing's benefits, and who's their health carrier?" Claude names the broker, lists the carriers by line with the people each one covers, tells you when the plan renews, and says which filing year the facts come from.
- **Work a renewal month.** "Pull up the July 1 renewals in Georgia." Claude counts the plans that renew that month and lists the largest groups with their broker and carrier.
- **Read a broker's book.** "What does this broker's book of business look like?" Claude shows the employers, carriers, lines of coverage and renewal months on that broker's filings.

Every answer links to the record on Form5500Lookup, so you can check it against the filing.

## How Claude reads the filings

- **Renewal dates.** Filings record the month a policy year ends. A plan whose policy year ends in June renews July 1, and Claude says it that way.
- **Retirement plans.** A 401(k) or pension plan has an agent of record and a contract year. Claude uses those words for retirement plans.
- **Small contracts.** A plan can list a 30-person contract next to a 2,000-person one. Claude compares the people covered, and names the largest contract as the main carrier.
- **Self-funded benefits.** A plan can list dental with no insured dental contract. That usually means the employer pays the claims itself, and Claude says so.
- **Previews.** Without a subscription, renewal lists and broker books show the largest few rows. Claude tells you how many rows there are in all and shows only the rows it received.

## Install

In Claude Code, add the marketplace and install the plugin:

```
/plugin marketplace add harperai/form5500lookup-plugin
/plugin install form5500lookup@form5500lookup
```

Restart Claude Code if it asks you to. You can start asking right away.

## Signing in

You can look up employers, renewal months and broker books without an account.

Saved searches and alerts use your Form5500Lookup account. CSV exports and full lists also need a subscription. If you ask for one of these before you sign in, Claude tells you what it needs, and your lookups keep working.

To sign in, ask Claude to sign you in to Form5500Lookup. Then type `/mcp`, choose **form5500lookup**, and choose **Authenticate**. Your browser opens the Form5500Lookup sign-in page. Sign in with your email, approve the connection, and come back to Claude Code.

To sign out, run `claude mcp logout plugin:form5500lookup:form5500lookup` in a terminal.

## About the data

The records come from the U.S. Department of Labor's public Form 5500 filings. Filings arrive months after a plan year ends, so every answer says which plan year it reflects. Form5500Lookup is not affiliated with the Department of Labor or with any employer, carrier or broker named in the data. The information is for research and prospecting. It is not legal, tax, insurance or investment advice.

When you ask a question, Claude sends the names, states and months in it to form5500lookup.com. Once you sign in, it also sends your sign-in token so the answers can use your account. The plugin has no code that runs on your computer.

## Help

Write to support@form5500lookup.com, or visit https://www.form5500lookup.com/support.

- Privacy policy: https://www.form5500lookup.com/legal/privacy
- Terms: https://www.form5500lookup.com/legal/terms

The plugin is MIT licensed.
