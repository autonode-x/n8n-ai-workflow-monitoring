# Privacy-Aware n8n Failure Alerting

> **Architecture:** See [ARCHITECTURE.md](ARCHITECTURE.md) for the infographic, detailed flow, and trust boundaries.

A small n8n error-workflow template that captures a minimal set of execution metadata and routes an incident notice to an operations channel, an operations inbox, and an append-only Google Sheets log. It is designed to avoid copying raw error text, stack traces, trigger payloads, or business data into routine alerts.

> **Sanitized public template:** `Workflow.json` contains no credential bindings. Your Slack channel, operations email, and Google Sheets destination must be configured for your own environment before activation.

## Project snapshot

- **Category:** n8n operations automation, incident notification, privacy-aware logging
- **Trigger:** n8n Error Trigger, invoked when the workflow is configured as an error workflow for other workflows
- **Destinations:** Slack, Gmail, and Google Sheets (each is best-effort in the supplied template)
- **Telemetry:** Timestamp, workflow/execution identifiers, last failed node, execution mode, retry reference, error class/name, and failed status
- **Explicitly omitted:** Raw error message, stack trace, original trigger payload, business data, and internal execution URL in the sanitized template

## How it works

1. An assigned n8n **Error Trigger** receives an error event from a monitored workflow.
2. **Normalize Safe Incident Context** creates a bounded incident record from selected event metadata and sets the status to `failed`.
3. Three independent branches attempt to notify a private Slack operations channel, email a configured operations inbox, and append a safe metadata row to a Google Sheets incident log.
4. Each sink is configured to continue on error so a failure in one notification destination does not intentionally prevent the other sinks from running.

See [`ARCHITECTURE.md`](ARCHITECTURE.md) for the system flow and trust boundaries.

## Import and configure

1. Import `Workflow.json` into your test n8n instance.
2. Bind your own Slack, Gmail, and Google Sheets credentials from n8n's credential manager. The public file intentionally has no credential references or secret values.
3. Replace `REPLACE_WITH_YOUR_SLACK_CHANNEL_ID`, `REPLACE_WITH_PRIVATE_OPS_EMAIL`, `REPLACE_WITH_YOUR_GOOGLE_RESOURCE_ID`, and `REPLACE_WITH_YOUR_SHEET_TAB_NAME` with private destinations you control.
4. Create a dedicated incident log with access limited to operators. Do not grant public link access. Establish retention and deletion rules for the rows.
5. Set this workflow as the **Error Workflow** in each n8n workflow you intend to monitor. Test in a non-production workflow with a controlled failure.
6. Verify the Slack, email, and sheet branches independently, including what happens when one destination is unavailable.
7. Confirm that production execution logs and workflow history have separate access controls and retention limits; this workflow does not change n8n's own execution-data retention.

## Security and privacy considerations

The submitted source export contained an owner email, private Slack channel identifier, Google Sheet identifiers/URLs, credential references, webhook IDs, and an n8n instance ID. These have been removed or replaced in this portfolio copy. **This does not revoke credentials or undo any prior disclosure.** If any actual credential was exposed in a public repository, rotate/revoke it with the provider and check repository history. The exposed email is not itself a password, but consider replacing it with a role-based operations alias and review unwanted mail.

- **Execution URLs:** The source export included an internal n8n execution URL in alerts and the sheet mapping. Execution URLs can reveal internal operational details and may be accessible to logged-in operators. This sanitized template removes that field from notification and logging paths. Apply the same change to any live copy before sharing or using it broadly.
- **Metadata is still sensitive:** Workflow names/IDs, execution IDs, node names, error classes, and timestamps can reveal operational structure. Restrict Slack, inbox, Sheets, n8n access, and retention accordingly.
- **Least privilege:** Use dedicated n8n credentials and role-based inbox/channel access. Avoid broad service-account permissions.
- **No raw data by design:** Keep the safe-context allowlist narrow. Do not add `$json` wholesale, raw error messages, stacks, secrets, customer data, or trigger payloads to notifications.
- **Best-effort delivery:** The workflow may not alert if all configured sinks fail or if n8n itself is unavailable. Consider an independent monitoring/uptime system for critical workflows.
- **Retry and duplication:** Depending on the failed workflow and n8n execution behavior, duplicate alerts/log rows may occur. Add a deduplication key and retention policy if operationally needed.

## Suggested portfolio description

> Built a privacy-aware n8n error workflow that normalizes a minimal allowlist of execution metadata and fans out incident notifications to Slack, an operations inbox, and an append-only Sheets log. The design intentionally omits raw payloads and stack traces, isolates notification failures, and documents sensitive execution-link handling and access-control requirements.

## License

Add a license only if you own or are authorized to redistribute the workflow and its configuration. Review n8n and third-party node/service terms before publishing.
