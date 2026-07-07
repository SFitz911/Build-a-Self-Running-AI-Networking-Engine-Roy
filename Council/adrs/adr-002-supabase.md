# ADR-002: Supabase over Firebase

## Context

The system needs a database for visitors, drafts, sends, and responses. It must support Row Level Security, async HTTP calls from triggers (to auto-send emails on approval), real-time subscriptions, and multi-tenant indexing.

## Decision

Use Supabase (Free tier).

- Full Postgres with Row Level Security policies.
- pg_net extension enables async HTTP from inside triggers (no external webhook service needed).
- Realtime subscriptions push new drafts to the client dashboard without polling.
- REST API with anon/service-role keys for direct client-side INSERTs.
- Free tier: 500 MB database, 50k MAU, unlimited API requests.

## Consequences

- Positive: Single service handles CRM storage, access control, auto-send trigger, and live updates.
- Negative: Free tier pauses after 1 week of inactivity (solved by keep-alive Worker).

## Reversal Cost

Medium. Postgres is portable, but the pg_net trigger and RLS policies are Supabase-specific. Migration requires rewriting the auto-send logic as an external webhook handler.
