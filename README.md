# Full-stack take-home: Order import

Build a full-stack application that helps an internal user safely import third-party order data into an existing database.

The user needs to inspect incoming data, understand potential problems, decide what to import, and understand the result.

## Time and tools

Spend approximately **four hours**. The problem is intentionally larger than this timebox: we do not expect every feature or edge case to be completed. Deliver the most useful coherent increment you can, and explain your priorities and remaining limitations.

Choose your language, frontend and backend frameworks, libraries, architecture, and development tools. AI models, agents, and code generators are welcome. You should understand, review, and be prepared to explain the work you submit.

## Expected outcomes

Your application should demonstrate that:

- A user can inspect incoming data before importing it.
- Questionable records are identified or handled safely, with enough information to support an informed decision.
- Successful imports persist in the supplied SQLite database, or a working copy of it.
- Repeated or failed actions do not silently duplicate orders, corrupt data, or overwrite valid existing information.
- Unexpected input fails safely and understandably.
- Someone else can run the application locally using your instructions.

This partner has a history of frequent data-quality issues. The supplied files are representative samples, not an exhaustive catalog of those issues. We do not fully control the partner's format or contents, and future files may contain problems not represented here. Treat incoming data as untrusted and build the safest useful import workflow you can within the time available.

We may try additional plausible inputs when reviewing your work. We care about graceful behavior and explicit assumptions; there are no undisclosed business rules to discover.

## Supplied data

- `data/existing-orders.db`: a SQLite database containing 12 existing orders.
- `data/partner-export-01.csv`
- `data/partner-export-02.csv`

Each CSV contains 30 data rows. All supplied records are fictional.

The database contains one `orders` table:

```sql
CREATE TABLE orders (
    id                INTEGER PRIMARY KEY,
    external_order_id TEXT NOT NULL,
    customer_email    TEXT NOT NULL,
    amount_minor      INTEGER NOT NULL
                      CHECK (
                          typeof(amount_minor) = 'integer'
                          AND amount_minor >= 0
                      ),
    currency          TEXT NOT NULL
                      CHECK (currency IN ('USD', 'EUR', 'GBP')),
    order_date        TEXT NOT NULL,
    status            TEXT NOT NULL
                      CHECK (
                          status IN ('pending', 'paid', 'shipped', 'cancelled')
                      ),
    source            TEXT NOT NULL,
    UNIQUE (source, external_order_id)
);
```

These are the domain rules for this exercise:

- An order is identified by the combination of `source` and `external_order_id`. These identifiers are case-sensitive; the same external ID can belong to different sources.
- All fields other than the generated internal `id` are required.
- CSV `order_amount` values represent major currency units—for example, `12.50` means 1,250 minor units. The database stores integer minor units in `amount_minor`.
- Supported currencies are `USD`, `EUR`, and `GBP`. Amounts must be nonnegative; zero is allowed. Currency conversion is outside scope.
- Order dates represent calendar dates, with `YYYY-MM-DD` as the expected format. No timezone interpretation is needed.
- Supported statuses are `pending`, `paid`, `shipped`, and `cancelled`. No status-transition rules are required.

The expected CSV columns are:

```text
external_order_id,customer_email,order_amount,currency,order_date,status,source
```

These rules describe accepted order data, not a guarantee that incoming files comply with them. Document any additional assumptions.

You may modify or extend the database schema. Keep the supplied database and CSV files unchanged as reproducible inputs, use a working database copy, and document how to initialize or reset it.

## Scope and judgment

Decide how the interface works, how users select or approve imports, and how to handle records that cannot safely be imported. You may narrow supported inputs if you make the limits clear and handle unsupported input safely.

We care about a working end-to-end increment, frontend and backend fundamentals, persistence, validation, technical safety, and your judgment about priorities, testing, and debugging. A plain, dependable application is sufficient; visual polish is not a priority.

Authentication, deployment, cloud infrastructure, drag-and-drop upload, multiple user roles, real-time processing, a spreadsheet-style interface, and an AI integration are not required. You do not need to write a custom CSV parser or support every possible CSV format.

## Documentation

In your repository or pull request, include:

- Local setup and run instructions.
- A short explanation of what you built.
- Important assumptions, tradeoffs, and known limitations.
- What you tested or otherwise verified.
- What you would do next.
- How you used AI, if applicable, and what you independently reviewed or verified.

Keep this brief. We do not need a process diary.

## Submission

1. Click **Use this template → Create a new repository**.
2. Create a **private** repository under your **personal GitHub account**. Do not use an employer, school, or other organization account.
3. Keep the original template state on `main`.
4. Create a branch named `submission` from `main` and commit your work there.
5. Add **`StrideTechHiring`** as a collaborator in your repository settings.
6. Open a pull request from `submission` into `main`. Leave it **unmerged**.
7. Reply to the original assessment email with:

```text
READY FOR REVIEW

GitHub username:
Private repository URL:
Pull request URL:
```

Once you send the email, leave the submission branch unchanged until we confirm receipt. We will verify access, record the pull request's exact head commit SHA, download that commit, and confirm receipt. That recorded commit is your official submission.
