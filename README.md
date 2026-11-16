# TRADING HQ

Private trading journal & analytics platform (Next.js + Supabase). See `docs/ARCHITECTURE.md`.

## Setup

1. **Create a Supabase project** (free tier) at https://supabase.com → New project. Pick a region near you and
   save the database password in your password manager (the app never needs it).
2. **Create the schema**: Supabase dashboard → SQL Editor → New query → paste the whole of
   `supabase/migrations/0001_phase1_foundation.sql` → Run. It creates tables, RLS policies, the
   `trade-screenshots` private storage bucket and its policies. Then run
   `supabase/migrations/0002_api_grants.sql` the same way (API privileges; required on new projects).
3. **Keys**: Project Settings → API Keys. Copy `.env.example` to `.env.local` and set
   `NEXT_PUBLIC_SUPABASE_URL` (the bare `https://<ref>.supabase.co`, without `/rest/v1/`) and `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` (or the legacy anon key as
   `NEXT_PUBLIC_SUPABASE_ANON_KEY`). Do **not** put the secret/service-role key in this file.
4. **Auth URLs**: Authentication → URL Configuration → Site URL `http://localhost:3000`, and add
   `http://localhost:3000/auth/callback` to Redirect URLs. (Optional for a local-only setup: Authentication →
   Sign In / Providers → Email → turn off “Confirm email”.)
5. `npm install` → `npm run dev` → open http://localhost:3000 → **Create account**.
6. **Lock it down**: Authentication → Sign In / Providers → disable “Allow new users to sign up”.
7. Settings → Instruments: enter your broker’s real commission and exchange/NFA fee per side.
   Settings → Accounts: create your accounts (e.g. “Backtesting”).

## Tests
```bash
npx vitest run
```
Runs the calculation-engine, timezone and database tests. The DB tests execute the real migration in
PGlite (Postgres in WebAssembly) and verify Row Level Security isolation between two users.
TRADING HQ — Personal Trading Journal & Analytics Platform
