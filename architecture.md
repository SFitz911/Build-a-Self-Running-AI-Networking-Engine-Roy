# System Architecture

Complete data-flow from QR scan to email delivery, including self-healing and second-touch paths.

```mermaid
flowchart TD
    %% Entry Point
    QR["QR Code Scan\n(UTM-attributed)"]
    LP["Landing Page\n(Cloudflare Pages)"]
    TN["Turnstile\n(Bot Protection)"]

    %% Data Capture
    SB_INSERT["Supabase INSERT\n(visitors table)"]

    %% AI Drafting
    COUNCIL["Claude Connection Council\n(C1: Follow-Up Drafter)"]
    SENDS["sends table\n(status: pending)"]

    %% Client Approval
    DASH["Client Dashboard\n(approve / edit / reject)"]
    APPROVED["status: approved"]

    %% Auto-Send
    TRIGGER["pg_net BEFORE UPDATE trigger"]
    RESEND["Resend API\n(email delivery)"]
    SENT["status: sent"]

    %% Response Tracking
    RESPONSES["responses table\n(reply tracking)"]

    %% Refinement
    E5["E5: Weekly Refinement\n(template performance)"]

    %% Second Touch
    STALE["stale_sends view\n(no reply after 7 days)"]
    C4["C4: Second-Touch Drafter\n(value-add message)"]

    %% Infrastructure
    WORKER["Keep-Alive Worker\n(daily cron)"]
    HEALTH["/health endpoint\n(component checks)"]

    %% Main Flow
    QR --> LP
    LP --> TN
    TN -->|verified| SB_INSERT
    SB_INSERT --> COUNCIL
    COUNCIL --> SENDS
    SENDS --> DASH
    DASH -->|approve| APPROVED
    APPROVED --> TRIGGER
    TRIGGER --> RESEND
    TRIGGER --> SENT
    RESEND --> RESPONSES

    %% Refinement Loop
    RESPONSES --> E5
    E5 -->|improves| COUNCIL

    %% Second Touch Path
    SENT --> STALE
    STALE --> C4
    C4 --> SENDS

    %% Infrastructure Connections
    WORKER -->|pings| SB_INSERT
    HEALTH -->|checks| WORKER
    HEALTH -->|checks| RESEND
    HEALTH -->|checks| SB_INSERT
```

## Key Paths

- **Main flow**: QR > Turnstile > Supabase > Claude drafts > Client approves > pg_net auto-sends > Resend delivers
- **Refinement loop**: Response data feeds E5 weekly, which improves Council drafting quality
- **Second touch**: Sends with no reply after 7 days get flagged, C4 drafts a value-add follow-up
- **Self-healing**: Keep-alive Worker prevents Supabase pause. Health endpoint monitors all components.
