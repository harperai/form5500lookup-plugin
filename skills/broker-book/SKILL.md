---
name: broker-book
description: Read a benefits broker's or agency's book of business from public Form 5500 filings, or a carrier's book. Use when someone asks which employers a broker or competing agency places, which carriers an agency works with, its lines of coverage, how its accounts spread across renewal months and states, or the commissions disclosed on its contracts.
license: MIT
metadata:
  author: Harper AI, Inc.
---

# Read a broker's book

This skill reads a broker's (or a carrier's) book from Schedule A filings with the Form5500Lookup tools. It reports the record. It does not judge the broker.

## Steps

1. Find the broker with `search_filings`, passing the agency name as `q`. Brokers come back in the `brokers` group, carriers in `carriers`. Each has a `url`; the slug is the last part of it, after `/broker/` or `/carrier/`.
   - A large agency often has several offices, each its own record. Choose the office the person means (by city or state), and say that other offices exist.
   - When the `fuzzy` flag is true for the group, the rows are closest matches. Say so and confirm the name before reading the book.
2. Open the book with `get_broker` and the slug. For a carrier, use `get_carrier`.
   - To narrow it, pass `state` (two-letter code) or `month`. `month` is the month the policy year ends: a July 1 renewal is month 6, a January 1 renewal is month 12.
3. Answer from that result only.

## What to report

- **Size of the book**: `contractsBrokered`, `book.employers` and `book.personsCovered`.
- **Where it is**: `book.topStates`.
- **Carriers it works with**: `book.topCounterparties`, with contract counts.
- **Lines of coverage**: `book.linesOfCoverage`.
- **Renewal spread**: `book.contractsByPolicyEndMonth` gives contracts by policy-end month. Say it in renewal dates: December policy ends renew January 1.
- **Commissions**: `disclosedCommissions` and `commissionBand` when shown.
- **Accounts**: the `rows`, each with employer, carrier, benefits, persons covered, renewal date (`renewsOn`) and the row `url`.
- **The firm**: when `firm` is present, give its office count and the firm record `url`, which combines every office.

## Citations

Cite the broker record `url` and each employer row's `url`, so the reader can check every account against the filing.

## Being honest about what is shown

- Without a subscription, `get_broker` shows the top rows of the book and a `gate` with `shown`, `total` and `hidden`. Say exactly that, for example "showing the top 5 of 62 contracts; the other 57 are not in this preview". When the result's summary or notes say where the rest are, such as being included with a subscription, repeat that rather than describing plans from memory.
- List only the rows returned. Do not guess at the rest of the book.
- Report facts, not a verdict. Do not describe a broker as good, bad, strong or weak, and do not use the record to vet or screen a person.
- Do not offer links about plans or pricing of your own. Cite record links only.
- Do not offer exports, saved searches or alerts the person did not ask for.

## When a tool needs an account

`export_renewals`, `save_search`, `watch_employer` and `match_list` need a signed-in Form5500Lookup account. Without one they answer with a short message saying so. Exports and full lists also need a subscription.

- Tell the person plainly what the tool needs, in the words of the tool's own message. If they already have a Form5500Lookup account, they can ask you to sign them in.
- Do not promise that signing in will show the full list or make the export. That depends on the account's subscription.
- Never give a download link, file or saved search that the tool did not return. Repeat a link only when the tool's own message includes it.
- Call `sign_in` only when the person asks to sign in, log in or connect their account.
