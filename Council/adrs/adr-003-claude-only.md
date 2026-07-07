# ADR-003: Claude-Only over Dual-Model

## Context

The Connection Council needs an LLM for drafting follow-ups, refining templates, and running compliance checks. Options: Claude-only (via Max 20x subscription) or a dual-model approach mixing providers.

## Decision

Use Claude exclusively (Max 20x at $200/mo).

- Single-provider simplifies prompt engineering, context management, and billing.
- Max 20x provides 20x more usage than Pro, enough for all 25 agents plus daily drafting.
- Sonnet 4.6 for complex reasoning (refinement, compliance). Haiku 4.5 for volume drafting.
- MCP integration via Obsidian filesystem gives Claude direct access to the vault.

## Consequences

- Positive: One billing relationship. Consistent voice across all agents. No provider-switching logic.
- Negative: Single point of failure if Anthropic has an outage. No cost arbitrage with cheaper models.

## Reversal Cost

Low. Prompts are plain markdown. Any LLM can read them. The MCP integration is Claude-specific but the vault content is portable.
