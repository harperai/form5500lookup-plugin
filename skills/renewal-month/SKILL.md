---
name: renewal-month
description: Build a prospect list of employers whose group benefits renew in a given month, by state, from public Form 5500 filings. Use for requests such as July 1 renewals in Georgia, January renewals in Texas, how many groups renew each month in Ohio, or the biggest plans renewing next quarter, with the broker of record, carrier and size of each group.
license: MIT
metadata:
  author: Harper AI, Inc.
---

# Work a renewal month

This skill turns a renewal question into the right Form5500Lookup calls and reads the results back in renewal language.

## Step 1: turn the renewal date into the policy-end month

Form 5500 records the month a plan's policy year **ends**, not the month it renews. Most policies run to the end of a month and renew the next day.

| The person asks for | Policy-end month to pass as `month` |
| --- | --- |
| January 1 renewals | 12 (December) |
| April 1 renewals | 3 (March) |
| July 1 renewals | 6 (June) |
| October 1 renewals | 9 (September) |

The rule: a renewal on the first of a month is the month before it. "July 1 renewals in Georgia" is `state` GA and `month` 6. A request that names the policy-end month itself ("policy years ending in June") uses that month as given. When the request is unclear, choose the reading above and say which policy-end month you used.

States are two-letter codes (GA, TX, OH).

## Step 2: size the month

Call `count_renewals` with the `state`. It returns the number of plans for each policy-end month. Give the count for the month in question, so the list that follows has context.

Counts work fully without an account. `count_renewals` takes the size, premium and commission filters (`minParticipants`, `minPremium`, `minCommission`, `healthOnly`, `sizeBand`) for everyone; only `carrier` needs an account. When the person narrows the request ("groups over 100 lives", "commissions above $50,000"), pass those filters to `count_renewals` and give that exact count before listing.

## Step 3: list the employers

Call `list_renewals` with `state` and `month`. For each row, give the employer, the broker of record, the carrier, participants, disclosed commissions when shown, the renewal date (`renewsOn`) and the row `url`.

## Step 4: say it in renewal language

- Describe these plans as renewing on the renewal date: "renews July 1 (policy year ends June)".
- Never say a plan renews in its policy-end month. A June policy-end month is a July 1 renewal.
- Give the filing vintage when it differs between rows, since some filings are a year older than others.

## Step 5: be honest about withheld rows

Without a subscription, `list_renewals` returns a preview: the largest plans for the month, and a `gate` with `shown`, `total` and `hidden`.

- Say exactly what was shown, for example "showing 5 of 152 plans; the other 147 are not in this preview". When the result's summary or notes say where the rest are, such as being included with a subscription, repeat that rather than describing plans from memory.
- List only the rows returned. Do not estimate, extend or invent rows, names or figures beyond them.
- In a preview, `list_renewals` returns the largest plans for the month and does not apply filters such as carrier, size or commission. Say the list filters were not applied, and give the filtered count from Step 2.
- Do not offer links about plans or pricing of your own. Cite record links only.
- Do not offer exports, saved searches or alerts the person did not ask for.

## Exports and saved searches

A list in the chat comes from `list_renewals`. `export_renewals` (a CSV file) and `save_search` (a saved search with alerts) are for when the person asks to export, download or save. They need a signed-in Form5500Lookup account.

## When a tool needs an account

`export_renewals`, `save_search`, `watch_employer` and `match_list` need a signed-in Form5500Lookup account. Without one they answer with a short message saying so. Exports and full lists also need a subscription.

- Tell the person plainly what the tool needs, in the words of the tool's own message. If they already have a Form5500Lookup account, they can ask you to sign them in.
- Do not promise that signing in will show the full list or make the export. That depends on the account's subscription.
- Never give a download link, file or saved search that the tool did not return. Repeat a link only when the tool's own message includes it.
- Call `sign_in` only when the person asks to sign in, log in or connect their account.

