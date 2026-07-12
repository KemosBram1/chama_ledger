# Kemoli's Chama — Ledger

A simple, mobile-friendly web app for tracking monthly contributions, savings, loans, and payouts for our chama. Built to replace pen-and-paper tracking with something the treasurer can update in seconds.

**Live app:** https://chama-ledger-dusky.vercel.app/

## What it does

- **Ledger** — enter the total amount a member sent this month; the app automatically splits it into contribution (KES 2,000), transaction fee (KES 20), and savings. If someone pays less than required, the shortfall automatically carries forward and adds to what they owe next month — no manual math.
- **Payout** — pick the two members receiving this month's pooled contributions; the app calculates each person's share.
- **Members** — add, rename, or mark members inactive without losing their history.
- **Loans** *(admin only, visible after signing in)* — issue loans, apply 10% monthly interest on the outstanding balance, record repayments, and mark loans paid.

## Access levels

- **Anyone with the link** can view the ledger, payout, and member list — read only.
- **Signed-in admins** (the treasurer and organizer) can edit amounts, manage members, and see/manage loans. Sign in via the button top-right.

## Tech stack

- Plain HTML/CSS/JS — no build step, no framework
- [Supabase](https://supabase.com) — free Postgres database + authentication
- [Vercel](https://vercel.com) — free static hosting, auto-deploys on every push to `main`

## Setup (for a fresh copy of this app)

1. Create a free Supabase project at supabase.com
2. In the Supabase **SQL Editor**, run the scripts in this repo in order:
   - `supabase-setup.sql` — creates the `members` and `month_data` tables
   - `supabase-restrict-access.sql` — locks down editing to signed-in accounts only
   - `supabase-add-loans.sql` — adds the loans table (admin-only)
3. In Supabase **Settings → API**, copy your **Project URL** and **anon public key**
4. Open `index.html` and paste those two values into the `SUPABASE_URL` and `SUPABASE_ANON_KEY` constants near the top of the script
5. In Supabase **Authentication → Users**, create a login (email + password) for each admin (treasurer, organizer)
6. Push this repo to GitHub, then import it into Vercel as a new project — no build command or framework needed, it deploys as-is
7. Before real use, sign in and use **Members → Danger zone → Reset all balances** to clear out any test data

## Notes

- Editing the anon key doesn't need to be kept secret — Supabase is designed for it to be public, and access is controlled through the database rules (RLS policies) in the SQL scripts instead.
- "Reset all balances" is permanent and cannot be undone — use it once before handing the app to the treasurer, not routinely.
- There's currently no way for a member to see just their own record privately; loan and payment status is shared verbally by the treasurer for now.
