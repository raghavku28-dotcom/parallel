# Parallel development context

Parallel is a mobile-first social investing pilot for Chicago Booth and University of Chicago users. Members receive $100,000 in simulated Parallel Cash, publish equity and ETF portfolios, discover classmates' strategies, and copy them into paper portfolios.

The product thesis is trusted capital allocation: users are backing people they know and respect, not anonymous trading personalities.

## Product constraints

- Preserve the dark charcoal, white, and electric-lime visual system.
- Preserve the `||` Parallel brand mark without a diagonal slash.
- Keep the experience mobile-first and consumer-oriented rather than academic.
- Use the generated dotted profile images until real profile photos are implemented.
- Keep Booth/UChicago magic-link authentication as the entry point.
- Clearly separate simulated returns from real-money activity.
- Never claim that Plaid executes trades; use it only for investment-account data and reconciliation.
- Never claim that a Robinhood order was executed unless an authorized broker integration returns a confirmed execution result.
- Require explicit review before preparing or submitting any real-money order.

## Current architecture

The current implementation is a dependency-free static application in `index.html`. Supabase is loaded from its CDN and supplies authentication, relational data, database functions, row-level security, and realtime updates.

The page includes:

- Booth/UChicago magic-link login.
- New-user profile setup.
- A $100,000 simulated wallet.
- Live class portfolio discovery.
- Stock and ETF search from `stock_universe`.
- Weighted portfolio creation and publishing.
- Atomic paper-copy allocation through `copy_pilot_portfolio`.
- A paper-versus-real copy sheet.
- A reviewed Robinhood Agentic trade-ticket handoff.
- Alpha Vantage end-of-day quote refresh.
- A small WebMCP tool for starting a paper simulation.

## Important backend expectations

Do not weaken or replace Supabase row-level security with browser-only checks. The client-side domain check is user experience validation, not the security boundary.

When adding backend behavior, create version-controlled SQL migrations for every table, policy, trigger, function, and realtime publication change. Do not place privileged credentials in browser code.

## Suggested next release

1. Add persistent `brokerage_copy_intents` and `brokerage_execution_receipts` records.
2. Add a portfolio detail view showing creator thesis, positions, updates, risk, and copy activity.
3. Add rebalance previews that compare a follower's current holdings with the creator's latest target weights.
4. Add a confirmed broker execution adapter only after an approved brokerage relationship and compliance review.
5. Add Plaid investment synchronization for read-only reconciliation after production access is available.
