# ADR-006: Cloudflare Turnstile over reCAPTCHA

## Context

The landing page form needs bot protection. Conference attendees should not encounter friction. Options: Google reCAPTCHA or Cloudflare Turnstile.

## Decision

Use Cloudflare Turnstile (Free, Managed mode).

- Free with no request limits.
- WCAG 2.2 AA compliant (accessible to all attendees).
- Managed mode auto-decides challenge level. Invisible to most real visitors.
- Same vendor as Pages, Workers, and DNS. No additional account needed.
- Privacy-preserving: no tracking cookies, no data sold to advertisers.

## Consequences

- Positive: Zero friction for real conference attendees. Zero cost. Same-vendor integration.
- Negative: Less market share than reCAPTCHA (smaller ecosystem of troubleshooting resources).

## Reversal Cost

Low. Swap the Turnstile script tag and widget div for reCAPTCHA equivalents. Update server-side validation endpoint. Under 30 minutes of work.
