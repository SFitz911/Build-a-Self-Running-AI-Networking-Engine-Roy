# ADR-001: Cloudflare Pages over Vercel

## Context

Marcus's landing page needs global CDN hosting that is free for commercial use, fast on mobile, and low-maintenance for 5+ years. Two leading options: Cloudflare Pages and Vercel.

## Decision

Use Cloudflare Pages (Free tier).

- Unlimited sites, requests, and bandwidth on the free plan.
- Commercial use explicitly allowed.
- Same-vendor ecosystem: Workers, Turnstile, Analytics, and DNS all integrate natively.
- Vercel Hobby plan restricts usage to non-commercial personal projects only.

## Consequences

- Positive: Zero hosting cost at any traffic level. Single vendor for CDN, serverless, bot protection, and analytics.
- Negative: Fewer framework-specific optimizations compared to Vercel (not relevant for static HTML).

## Reversal Cost

Low. Static HTML deploys anywhere. Swap the Git integration to Vercel or Netlify in under 10 minutes. Only the Worker (keep-alive cron) needs a new home.
