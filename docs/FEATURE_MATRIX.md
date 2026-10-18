# Feature Preservation & Navigation Matrix

This is a **planning checklist**, not proof that every feature exists. Confirm each row against the actual source repository and runtime before marking status.

Status vocabulary: `Not audited`, `Implemented`, `Partially implemented`, `External dependency`, `Unsupported`, `Regression tested`.

| Existing/target capability | Proposed navigation | Audit evidence required | Regression checks |
|---|---|---|---|
| Authentication/session | Global | Guards, session config, auth routes | Login/logout, session expiry, role access |
| Users and roles | System | Policies, roles, permission tables | Forbidden access, delegated permissions |
| Tenant/domain management | System / Reseller | Tenant model, scopes, middleware | Cross-tenant access denied |
| Extensions/devices | PBX | Routes, models, provisioning, FreeSWITCH config | CRUD, registration, device provisioning |
| SIP gateways/trunks | PBX | Gateway models/configuration, status jobs | Save/reload, status, outbound test |
| Inbound routes | PBX | Dialplan generation and route services | DID matching and destination routing |
| Outbound routes | PBX | Dialplan, permissions, rate patterns | Authorized routing and rejection cases |
| IVR / virtual receptionist | PBX | IVR models, dialplan, media files | DTMF, timeout, invalid input, destination |
| Ring groups | PBX | Ring strategies and member config | Member ringing, timeout, fallback |
| Queues/contact center | Contact Center | Queue/agent implementation and events | Join, distribute, pause, wrap-up |
| Voicemail | PBX | Mailbox, media, notification logic | Record, retrieve, permissions |
| Call recordings | PBX / Reports | Storage, ACL, retention, links | Access control, playback, retention |
| CDR/call history | Reports | CDR source, filters, tenant scope | Accuracy, filters, pagination, export |
| Call monitoring | Contact Center | Actual FreeSWITCH support and permissions | Listen/whisper/barge only where supported |
| Campaign/dialer | Campaigns | Jobs, provider/telephony integration | DNC, retry, concurrency, duplicate prevention |
| SMS / Voice SMS | Messaging | Provider APIs, billing, DLR/webhooks | Delivery state, idempotency, tenant billing |
| Chatbot/omnichannel | Messaging | Provider API/webhook contracts | Signature validation, retries, handoff |
| Reseller tariffs/billing | Reseller & Billing | Ledger, tariff model, wallet, invoices | Correct price, idempotency, tenant isolation |
| Audit logs | System | Audit event service and storage | Sensitive changes recorded; secrets redacted |
| Backup/restore | System | Existing scripts and retention rules | Backup integrity and documented restore test |
| Gateway failover | PBX / System | Health checks, gateway switching logic | Failure threshold 1, recovery stable 60s, failback |
| Reporting/analytics | Reports | Source events and metric definitions | Calculations reconcile with raw data |
| Easy/Advanced Mode | User preferences | UI state/config persistence | Switching modes preserves settings |

## Audit record template

For each feature, record:
- Existing implementation and exact source files/routes/API endpoints.
- Existing roles/permissions and tenant boundary.
- Current behavior and known defects.
- New navigation destination and UX change.
- Whether backend changes are necessary and why.
- Automated/manual tests and observed results.
- Rollback or migration plan if behavior changes.

Do not mark a row complete until source evidence and tests are available.