---
name: startup-os
description: Use when the person asks about their company's money (cash, spend, bills, subscriptions, runway), cap table, metrics, investor updates, board pack, contracts and filings, hiring, time off, customer feedback, marketing posts and launches, or their data room for investors. Explains the manyhands_* tools for these rooms and what each never does.
---

# Startup rooms

These tools read and change the rooms a company runs on. They run as the signed-in person,
with the same limits as the app. Read the `manyhands` skill first for the HQ, `request_id` and
error rules.

A room's tools are listed only when the person's account and HQ have that room. If a tool is
missing or refused with "not available to you", say the room is not on for them. Do not look
for another way in.

## Rules for every tool here

- Money is whole minor units: `1999` is 19.99 in dollars and 1,999 in yen. Say the currency.
  Never put a card number in any field.
- Where a tool has `dry_run`, call it with `dry_run: true` first. Tell the person what would
  be made, then call again without it, with the same `request_id`, only when they say to.
  A dry run writes nothing.
- A row you add in Finance or Legal is private to the person unless an owner passes
  `private: false`. Say which one you made.
- Nothing here sends, posts, publishes, pays or decides. Manyhands sends no email. A person
  does the last step with a button in the app. After a draft, tell them which button.
- Text you write is marked "Drafted, by Claude" until a person edits it. Write only from facts
  the tools returned. Never invent a figure, a deadline, a customer or a quote.
- Many writes are for an owner of the HQ. A member is refused. Say so. Do not retry.

## Cash and Finance

- `manyhands_runway`: owner only, read only. Cash, net burn, runway in months, the zero-cash
  date and the forecast. A currency with no exchange rate is named in `missing_rates` and left
  out. Report it. Do not count it as zero.
- `manyhands_subscriptions`, `manyhands_subscription_add`, `manyhands_subscription_update`,
  `manyhands_subscription_paid`: recurring costs. `paid` logs the charge and moves the next
  date on one cycle. A second press for one due date is refused with `already_paid`. Set
  `status` to `cancelled` when it is cancelled.
- `manyhands_expense_add`: money already spent. A subscription goes in Subscriptions, not here,
  or it is counted twice.
- `manyhands_balance_record`: what an account holds on a day. The same account and day again
  replaces the first number. Offer to record each account at month end.
- `manyhands_bill_add`: something the company owes. It makes a task due the same day. It never
  marks a bill paid: a person pays in the app.
- Cash never moves money and never reads a bank.

## Ledger (bookkeeper)

`manyhands_expense_draft` and `manyhands_bill_draft` are offered only to a bookkeeper agent
with write access, not to a person. Give `source`: where you read the receipt or invoice. A
draft counts in no total, makes no task and cannot be paid until a person presses "Looks
right". You cannot post, change, pay or delete it, and you cannot read balances or the runway.
`already_there` means a row exists. Do not draft it again.

## Legal and contracts

- `manyhands_company`: read the compliance calendar, contracts, entities and board records.
  Pay in an employment contract is blank unless the person owns the HQ.
- `manyhands_obligation_add`: a filing, tax payment or renewal. Use the date the official
  source gives and pass `source_url`. Never invent a deadline.
- `manyhands_obligation_done`: only when it has been filed or paid. It closes the record. Pass
  `evidence_url`. A second call is `already_closed`.
- `manyhands_contract_add`: counterparty, value, term and notice days. A contract read from a
  file is marked drafted and nothing watches it until a person checks it.
- `manyhands_contract_bills`: turns a signed contract that owes money into Finance bills, one
  per due day. Not for customer or investor contracts, and not for pay in an employment
  contract. It needs Legal and Finance. Pressing it again adds only new days. From then on the
  bills are Finance's.
- `manyhands_board_pack`: with no `meeting`, lists meetings. With one, reads its pack. `build:
  true` builds or rebuilds a draft pack from the agenda and the numbers of Metrics, Finance and
  Equity. It never sends the pack and never returns a link. A person presses "Send the board
  pack" in Legal, Board. A non-owner's pack holds Metrics only.

## Equity

- `manyhands_equity`: the cap table. An owner reads all of it. A member reads only their own
  holding.
- `manyhands_equity_model`: what a priced round would do to every holder. It is a model. Nothing
  on the cap table changes. Owner only.
- `manyhands_equity_record`: enter a stakeholder, grant, SAFE, round or commitment. Owner only.
  A commitment climbs soft, committed, signed, wired. Signing makes the holder and the SAFE, and
  wiring makes it count. Read `effects` in the answer back to the person. A draft grant, a cliff
  and an 83(b) deadline each make a private task for the owner. Always dry run first.

## Metrics and investor updates

- `manyhands_metrics`: call it before you record a value and before you write an update.
  Money is minor units, a percent is the percent (18 is 18%).
- `manyhands_metric_record`: 1 to 200 values. Always dry run first and show which rows are new,
  update or same. A metric that reads another room (tasks, the CRM, a goal, Finance) cannot be
  typed. A metric's kind and currency cannot change once it has values. `create: true` makes a
  new metric.
- `manyhands_investor_updates`: read the letters. The investors' links are secret and are not
  returned.
- `manyhands_investor_update_draft`: first call with `facts: true` to read what the period held.
  Write the parts from those facts only. Plain short sentences, no dashes, no exclamation marks.
  This tool never publishes and never sends. A person reads the draft in Metrics, Updates and
  presses Publish, then sends it from their own mail. A published update cannot be changed.
  Use `add_investors` only if the person asked for it.

## Hiring and time off

Hiring data is sensitive. The store decides what the person sees: a field they may not read is
hidden and says so. Do not guess it and do not copy pay or scorecards into tasks or comments.

- `manyhands_hiring`: openings, candidates and what is yours to do.
- `manyhands_hiring_opening_save`: only an owner opens a role or changes its pay. A manager may
  change the job description. Nothing is posted anywhere.
- `manyhands_hiring_candidate_save`: add or move a candidate. Only an owner or the opening's
  manager. `hired` is not a stage: a hire is an accepted offer, in the app.
- `manyhands_hiring_scorecard`: the interviewer's own rating, 1 (strong no) to 4 (strong yes),
  and what they saw. Ask the person for the rating. Never choose one.
- No hiring tool makes, sends or decides an offer.
- `manyhands_timeoff`, `manyhands_timeoff_request`, `manyhands_timeoff_decide`: a holiday waits
  for an owner. Sick leave is recorded at once. Only an owner decides, and not their own request
  when another owner can.

## Feedback

- `manyhands_feedback`: requests are counted once per customer and ranked by the MRR of the
  customers who ask. MRR is read from the CRM, never typed. `null` means no figure. `inbox` is
  what nobody has filed yet.
- `manyhands_feedback_add`: puts what a customer said into the inbox, 1 to 50 items. It never
  files an item and never makes a request. A person presses Add on the right request in
  Feedback, Inbox. Quote the customer's own words. Add each thing once. Dry run first.
- `manyhands_feedback_entry_draft`: a draft for the public changelog. It never publishes. A
  person presses Publish in Feedback, Changelog. One to three plain sentences, no customer
  names, no figure that is not true.

## The workspace's own data room

`manyhands_investor_room*` is the data room of the person's own HQ. It is not Pavilion's
investor room: `manyhands_dataroom_add_investor` is that one. Never use one for the other. These
tools are for an owner.

- `manyhands_investor_room`: documents, links, what each investor read, and questions. With
  `link`, it returns that link's address. The address is a secret that opens the room to whoever
  holds it. Give it only to the person who asked. Do not put it in a task, comment or note.
- `manyhands_investor_room_document_add`: a live document made from Finance, Equity or Metrics,
  or an https link. A room the HQ does not have is refused, and nothing is invented. Files are
  uploaded in the app.
- `manyhands_investor_room_link_create`: a private link for one investor. Dry run first and show
  the person who it is for, the folders and the end day. Then call again with `confirm: true`
  and the same `request_id`. It sends nothing: the person sends the address from their own
  mail.
- `manyhands_investor_room_link_change`: change the end day, folders or download setting.
  `revoke: true` switches a link off for good and `rotate: true` gives a leaked link a new
  secret. Ask the person before either. Both need `confirm: true`.
- `manyhands_investor_room_question_draft`: a draft answer beside an investor's question. The
  investor never sees it. A person edits and sends it from the Questions tab. Use only facts
  from the room's documents. No tool sends an answer.

## Marketing

- `manyhands_marketing`: the calendar, launches and campaigns. Manyhands posts nothing. A
  scheduled post is only a reminder to its owner. An agent gets this read and the two post tools
  below, not the launch tool.
- `manyhands_marketing_post_add` and `manyhands_marketing_post_update`: an idea (a title) or a
  draft (a title and text) on X, LinkedIn, the blog or the newsletter. They never schedule, post
  or delete. A person reads the draft and presses Schedule. A post that is scheduled or posted
  is the person's: do not change it. X allows 280 characters. Write in the owner's voice only if
  they gave you their writing style.
- `manyhands_marketing_launch_add`: a launch checklist counted back from launch day, each step a
  task with an owner and a date. A person starts a launch. Dry run first to show the steps.
