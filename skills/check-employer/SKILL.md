---
name: check-employer
description: Check one employer's benefit plans in public Form 5500 filings. Use when someone asks who the broker of record is on a company's health, dental, vision, life, disability or retirement plan, which carrier writes a line of coverage, how many people a plan or contract covers, how big a group is, or when a company's benefits renew. Works from a company name or a nine-digit EIN.
license: MIT
metadata:
  author: Harper AI, Inc.
---

# Check an employer

This skill answers questions about one employer's benefit plans from its Form 5500 filing, using the Form5500Lookup tools.

## Steps

1. Find the record with `search_filings`, passing the company name or the EIN as `q`.
   - Each result has a `url`. The record's slug is the last part of that url, after `/employer/`.
   - A company can have several plans: a health and welfare plan (`kind` is `welfare`) and a retirement plan (`kind` is `retirement`) in `employers`, and filers without insured contracts in `filings`, such as self-funded, retirement-only or small plans. A `filings` row never means the employer offers no coverage. Open the plan that answers the question, and mention the others in one line.
   - When a group's `fuzzy` flag is true, its rows are the closest matches to the name, not exact matches. Say that plainly, name the closest match, and ask the person to confirm it is the company they mean. When only one closest match is plausible, open it, and still say it is the closest match.
2. Open the record with `get_employer`, passing the slug.
3. Answer from that record only. A field that is null or missing is not on the filing, and the answer says so.

## Health and welfare plans (`isPension` is false)

Report, in this order:

- **Broker of record**: `brokerOfRecord`.
- **Carriers by line of coverage**: read `coverage.rows`, and give each insured line with its carrier and the persons covered. Use `contracts` for detail: each contract's carrier, benefits, `personsCovered`, premium and commission when shown.
- **The main carrier for a line** is the one whose contract covers most of the plan's people. Compare each contract's `personsCovered` with `participants` before calling a carrier the main one. A contract that covers a small share of the plan, such as 30 people on a plan of 2,000, is a side contract (often a regional HMO, a small group or one location). Name it as a side contract with its persons covered. It is never the plan's main health carrier.
- **Size**: `participants`.
- **Renewal timing**: give the renewal date from `renewsOn`, with the month the policy year ends (`policyPeriodEndMonth`) in brackets. A policy year that ends in December renews January 1. Say "renews January 1 (policy year ends December)", never "renews in December".
- **Declared but uninsured lines**: a coverage row with status `declared_uninsured` means the plan listed that benefit on Form 5500 line 8b but filed no insured contract for it, usually because it is self-funded or carved out. It never means the employer does not offer the benefit. Write it as "dental is declared on the filing, with no insured contract filed (often self-funded)".
- **Filing vintage**: the plan year and the date filed, from `vintage`, so the reader knows how old the data is.
- **Citation**: the record `url`.

## Retirement plans (`isPension` is true)

A retirement plan funded by annuity or GIC contracts has its own vocabulary. Use these words:

- **Agent of record** (`agentOfRecord`), never "broker".
- **Contract year ends** (`contractYearEndMonth`), never "renews" or "renewal". The end of a contract year is not a benefit renewal.
- **Contract value** (`contractValue`), never "premium".
- The carrier or provider that holds the contract, its persons covered, the filing vintage and the record `url`.

When `agentOfRecord` is null, say no agent of record is named on the filing.

## Being honest about what is shown

- Report only what the tools return. Do not add brokers, carriers or figures from memory.
- When a result says rows or fields are withheld, repeat its counts plainly, for example "the filing lists 9 contracts; commissions are not shown here". When the result's summary or notes say where they are, such as being included with a subscription, repeat that rather than describing plans from memory.
- Do not offer links about plans or pricing of your own. Cite record links only.
- `export_renewals`, `save_search` and `watch_employer` are for when the person asks for an export, a saved search or alerts. A look-up uses `search_filings` and `get_employer` only.

## When a tool needs an account

`export_renewals`, `save_search`, `watch_employer` and `match_list` need a signed-in Form5500Lookup account. Without one they answer with a short message saying so. Exports and full lists also need a subscription.

- Tell the person plainly what the tool needs, in the words of the tool's own message. If they already have a Form5500Lookup account, they can ask you to sign them in.
- Do not promise that signing in will show the full list or make the export. That depends on the account's subscription.
- Never give a download link, file or saved search that the tool did not return. Repeat a link only when the tool's own message includes it.
- Call `sign_in` only when the person asks to sign in, log in or connect their account.

## Answer shape

Lead with the answer in one or two sentences (broker, main carrier, renewal date). Then a short table of the lines of coverage with carrier and persons covered. End with the filing vintage and the record link.
