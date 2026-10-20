# Privacy-Aware n8n Failure Alerting — Architecture

![Architecture infographic](assets/architecture.png)


**End-to-end incident notification flow: capture minimal failure metadata and fan it out to operations channels without copying raw payloads or stack traces.**

## 1. Workflow at a glance

```mermaid
flowchart LR
    A["1. FAILURE EVENT<br/>Monitored n8n workflow"] --> B["2. ERROR HANDOFF<br/>Configured Error Workflow"]
    B --> C["3. ERROR TRIGGER<br/>Receives execution context"]
    C --> D["4. SAFE NORMALIZATION<br/>Allowlisted metadata only"]
    D --> E["5. PRIVATE SLACK<br/>Ops incident notice"]
    D --> F["6. OPS EMAIL<br/>Role-based inbox"]
    D --> G["7. SHEETS LOG<br/>Append incident row"]
    E -. "continue on error" .-> H["BEST-EFFORT DELIVERY<br/>Other sinks still attempt"]
    F -. "continue on error" .-> H
    G -. "continue on error" .-> H
    classDef trigger fill:#f3f1ff,stroke:#7256bd,color:#16112b,stroke-width:1.5px
    classDef normalize fill:#eef5ff,stroke:#4b79c7,color:#102343,stroke-width:1.5px
    classDef sink fill:#eff9f6,stroke:#238a77,color:#123a32,stroke-width:1.5px
    classDef caution fill:#fff5e8,stroke:#d98a36,color:#4e2c0c,stroke-width:1.5px
    class A,B,C trigger
    class D normalize
    class E,F,G sink
    class H caution
```

## 2. Safe incident payload

| Included operational metadata | Deliberately excluded |
|---|---|
| Timestamp, workflow/execution IDs, failed node, execution mode, error class/name, retry reference, failed status | Raw error message, stack trace, trigger payload, business/customer data, internal execution URL |

Identifiers and node names can still reveal internal operations. Keep all destinations and n8n execution history access-controlled.

## 3. Destinations and behavior

- **Slack:** private operations channel.
- **Gmail:** configured operations inbox.
- **Google Sheets:** append-only incident log.
- Each sink is best-effort/continue-on-error; one destination failing should not intentionally block the others. This does not guarantee delivery if n8n or all sinks are unavailable.

## 4. Services used

`n8n Error Trigger` · `Slack` · `Gmail` · `Google Sheets`.

## 5. Deployment notes

Import the sanitized workflow, bind least-privilege credentials, configure private destinations, and assign it as the Error Workflow for monitored n8n workflows. Test a controlled failure and each sink failure separately. Set independent retention/access controls for n8n's own execution logs; this flow does not change them.

> **Portfolio output:** this is an internal Error Workflow, not a public webhook service. Use an independent uptime monitor for cases where n8n itself is unavailable.
