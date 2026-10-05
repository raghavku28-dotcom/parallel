# Parallel application export

This folder contains the current working Parallel web application, prepared for continued development in Claude Code.

## Open the app

The application is a static, mobile-first site. From this folder, start any local static web server and open the local URL it provides. For example:

```bash
python3 -m http.server 4173
```

Then visit `http://localhost:4173`.

Do not open `index.html` directly with a `file://` URL. Authentication redirects and browser security features expect an HTTP origin.

## Files

- `index.html` — the complete interface, styling, and browser-side application logic.
- `assets/` — generated dotted profile artwork used by the interface.
- `.openai/hosting.json` — the existing OpenAI Sites deployment configuration.
- `CLAUDE.md` — implementation context and guardrails for Claude Code.

## Current services

- Production site: `https://mirrorline-ai-trades.raghavku28657297.chatgpt.site`
- Supabase project: `tsmqkfbgxzksyiaynloa`
- Authentication: Supabase passwordless magic links.
- Allowed pilot email domains: `chicagobooth.edu` and `uchicago.edu`.
- Market data: Alpha Vantage end-of-day quotes. Users enter their own key in the app; it is stored only in their browser.
- Brokerage prototype: the app prepares a reviewed dollar-based Robinhood Agentic trade ticket. It does not directly submit brokerage orders.

## Supabase dependency

The live Supabase project already contains the application's database tables, row-level security rules, realtime configuration, and database functions. Those server-side definitions are not embedded in this static export.

The browser app currently expects these objects:

- `profiles`
- `pilot_wallets`
- `pilot_portfolios`
- `pilot_holdings`
- `pilot_copies`
- `stock_universe`
- `publish_pilot_portfolio(...)`
- `copy_pilot_portfolio(...)`

If the app is moved to a new Supabase project, export the schema and row-level security policies from the existing Supabase dashboard first, then update `SUPABASE_URL` and `SUPABASE_KEY` near the beginning of the script in `index.html`.

## Security notes

- The Supabase key in `index.html` is a browser-safe publishable key. Authorization must continue to be enforced by Supabase row-level security.
- No Supabase service-role key, database password, Plaid secret, Robinhood credential, or Alpha Vantage key is included.
- Keep real-money trading user-directed and approval-based. Plaid can synchronize investment data but does not place stock orders.

## Recommended next structural change

Before the application grows substantially, split `index.html` into components and modules, add version-controlled Supabase migrations, and keep runtime configuration in environment variables. Preserve the current mobile-first flow while doing so.
