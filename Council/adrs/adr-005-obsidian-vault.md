# ADR-005: Obsidian Vault over Hosted CRM

## Context

The system needs a workspace for agent prompts, brand voice configuration, templates, and execution logs. Options: hosted CRM (HubSpot, Notion), or a local Obsidian vault with filesystem MCP.

## Decision

Use an Obsidian vault on the operator's machine.

- Operator owns all data locally. No vendor lock-in. No subscription.
- Filesystem MCP connects Claude directly to vault files for reading and writing.
- Markdown is future-proof and version-controllable with Git.
- Obsidian's graph view helps visualize agent relationships.

## Consequences

- Positive: Zero cost. Full data ownership. Claude reads/writes via MCP without API keys.
- Negative: No multi-user collaboration (not needed for a single operator). No cloud sync unless added.

## Reversal Cost

Low. Every file is plain markdown. Export to Notion, a CRM, or a database at any time with a simple script.
