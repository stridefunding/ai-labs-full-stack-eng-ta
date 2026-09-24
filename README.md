# AI Labs L1 Eng Tech Assessment

*Bring a little order to order imports.*

Hey! Thanks for taking the time to build something with us.

Your task: help an internal teammate import order data from a third-party partner with confidence.

Give this **about four hours**. There's more here than anyone needs to finish in that time, and that's okay. Pick a useful slice, get it working end to end, and leave us a few notes about what's next.

The stack is yours to choose. Libraries, docs, AI models, agents, and code generators are all welcome. Use whatever helps, and come ready to walk us through your code and decisions.

## What to build

Build a full-stack app that lets someone:

- Inspect incoming orders and understand any concerns before importing.
- Decide what to import, then save it to the supplied SQLite database (using a working copy).
- See what happened and what needs their attention.

Keep existing data safe. Repeating an action or encountering a failure shouldn't silently duplicate or corrupt orders. When something unexpected arrives, help the user understand what went wrong.

The partner has had plenty of data-quality issues. These files are just samples: we don't control every detail of their exports, and future files may bring new surprises. Treat incoming data as untrusted, and be clear about what your app can safely handle.

The interface and implementation are up to you. Plain and dependable is great. No need for authentication, deployment, fancy upload controls, or an AI feature. Use an existing CSV parser if you'd like; you don't need to support every possible CSV format.

## What's in the box

```text
data/
  existing-orders.db      # 12 orders in one orders table
  partner-export-01.csv   # 30 rows
  partner-export-02.csv   # 30 rows
```

All records are fictional. A few rules to save you guessing:

- `source` + `external_order_id` identifies an order. Both are case-sensitive; different sources can use the same external ID.
- Every order field is required; the database generates the internal `id`.
- CSV `order_amount` is in major currency units: `12.50` becomes `1250` in the database's integer `amount_minor` field. Amounts must be nonnegative; zero is fine.
- Currencies are `USD`, `EUR`, and `GBP`. No currency conversion needed.
- `order_date` is a calendar date in `YYYY-MM-DD` format. No timezones needed.
- Statuses are `pending`, `paid`, `shipped`, and `cancelled`. No status-transition rules needed.

Those are the rules for accepted orders, not promises about incoming files. Note any other assumptions you make.

Keep the supplied files unchanged and work with a database copy. You're welcome to change or extend its schema; just explain how to initialize or reset it.

## Leave us a short handoff

In your repo or PR, tell us how to run the app locally, what you built, and what you tested or verified. Include your main assumptions, tradeoffs, limitations, and what you'd tackle next.

If you used AI, briefly tell us how and what you independently checked. A few useful notes are plenty—no process diary needed.

## Getting started and sending it back

1. Click **Use this template → Create a new repository**.
2. Create a **private** repo under your **personal GitHub account**; organization policies can get in the way of sharing access.
3. Leave `main` at the original template state. Create a branch named `submission` from it and do your work there.
4. Add **`StrideTechHiring`** as a collaborator in your repo settings.
5. Open a PR from `submission` into `main` and leave it **unmerged**.
6. Reply to the original assessment email with:

```text
READY FOR REVIEW

GitHub username:
Private repository URL:
Pull request URL:
```

After emailing, hold off on further changes until we confirm receipt. We'll save the exact commit at the head of your PR as your official submission and let you know we've got it.
