# ADR-004: Static HTML over Framework

## Context

The QR landing page must load fast on mobile at conferences, remain functional for 5+ years without maintenance, and deploy with zero build steps.

## Decision

Use static HTML with Tailwind CSS CDN.

- Zero build step. No webpack, no bundler, no node_modules.
- No framework dependency rot. React, Next.js, and Vite all release breaking changes yearly.
- Deploys in seconds to any CDN. Lighthouse mobile score 95+ achievable.
- Tailwind Play CDN provides utility classes without a build pipeline.

## Consequences

- Positive: 5-year durability with zero maintenance. Fastest possible load time. Any developer can edit.
- Negative: No component reuse. No SSR or dynamic routing (not needed for a single landing page).

## Reversal Cost

Low. A single HTML file migrates into any framework as a template in minutes.
