# CRM Sales Agent Workspace, AI Agent Control Center & Customer Provisioning

**Status:** Product/implementation specification  
**Target:** Existing SoftCents FS PBX / FreeSWITCH application  
**Design reference:** User-supplied CRM/PBX screenshots showing Lead Management, click-to-call, extension status, call analytics, call reports, agent dashboards and extension directories.

## 1. Non-negotiable implementation rules

- Audit the actual FS PBX source before implementation. The planning repository is not the production application.
- Preserve existing features, routes, API contracts, authentication, database behavior, billing, tenant boundaries and FreeSWITCH call flows.
- FreeSWITCH remains the telephony engine. Do not replace it with Asterisk or introduce Asterisk-only commands.
- No fake metrics, simulated call states, dead buttons or claims of successful provisioning without backend verification.
- UI visibility never substitutes for server-side authorization.
- Do not commit provider API keys, SIP passwords, recordings, customer secrets or real personal data.
- Production call-routing, billing, destructive, high-cost and external campaign actions require explicit policy checks and approvals.
- If source access or a provider integration is missing, document the dependency and leave the feature honestly marked unavailable/pending.

## 2. Product areas and navigation

Create role-specific workspaces with a clean white background, dark-blue sidebar, restrained neutral surfaces, compact tables, search, filters, pagination and responsive layouts. Use the supplied screenshots as UX references, not as source code or branding.

### Sales / Support Agent workspace
- My Dashboard
- CRM: Leads, Contacts, Deals, Sales Pipeline, Tasks, Follow-ups, Calendar
- Phone: Web Dialer, Call History, Voicemail (if supported), Recordings (permission-gated)
- PBX: My Extension, Call Summary, My Reports
- Notifications
- My Profile and Settings
- Team/Supervisor tools only when authorized

### Supervisor workspace
- Team dashboard and agent availability
- Live call/queue monitoring when supported
- Agent performance and call QA
- Listen/Whisper/Barge only after verifying actual FreeSWITCH and application support, permissions, and legal/compliance requirements
- Team assignments and reporting within delegated scope

### Super Admin / Reseller / Customer workspaces
- Super Admin: provider/model control, global policies, tenant administration, audit and platform health
- Reseller: only delegated tenants, service packages, approved tariff controls, provisioning status and customer support
- Customer: own CRM/PBX services, own extensions, allowed IVR/recording settings, usage reports and conversational setup wizard
- Enforce every role and tenant boundary on every backend read/write, export, recording request, job and websocket/event subscription.

## 3. CRM Sales Agent Dashboard

Dashboard metrics must be calculated from actual authorized data:
- Assigned leads, active deals, won/lost deals
- Calls today, answered/missed/failed calls, talk time
- Tasks due, overdue follow-ups and completion rate
- Pipeline conversion and sales performance where source data supports the formula

Lead list:
- Search by name, phone, email, company, subject or notes
- Filters for owner, stage, source, service interest, date and next follow-up
- Lead detail, assignment, notes, activity timeline, task creation and permitted exports
- Lead stages configurable by authorized administrators
- Click-to-call from lead, contact and deal records
- Call outcome/disposition and notes recorded after calls
- Follow-up reminders and calendar
- Convert qualified lead to customer only through validated, idempotent workflow; do not create duplicate customers.

CRM matching:
- Match inbound caller IDs against normalized phone records within the current tenant.
- Show a verified exact contact match only when confidence/rules support it.
- If no match is found, display exactly **Unknown**; never hallucinate a contact name.
- Do not reveal another tenant's matching contact.

## 4. Browser WebRTC softphone / dialer

Implement a working browser dialer using the existing supported FreeSWITCH SIP/WebRTC integration:
- Numeric keypad and editable destination
- Click-to-call from CRM
- Incoming call notification and CRM screen pop
- Answer, reject, hang up, mute, hold, DTMF, transfer and other controls only when supported by the actual stack
- Agent extension and presence states: Available, Busy, Ringing, On Call, Offline, Unknown
- Registration status, connection health, reconnect behavior and user-friendly errors
- Call timer, remote party, call direction and live status sourced from actual telephony events
- Post-call disposition, notes, follow-up scheduling and permitted recording link
- Prevent accidental duplicate dial requests and enforce server-side calling permission, route policy, credit/budget restrictions and destination restrictions
- Never expose SIP secrets to the browser or log credentials
- Test inbound/outbound calling, WebRTC/WSS, reconnection, DTMF, hold, transfer, recording permissions and call event accuracy.

If a control cannot be implemented with the existing telephony stack, disable it with a clear explanation rather than presenting a fake working button.

## 5. PBX extension dashboard and reports

Extension dashboard:
- Total/configured extensions and package capacity
- Registered/reachable, idle/available, ringing, active call, offline and unknown counts
- Live activity breakdown and last-updated timestamp
- Tenant/workspace capacity and PBX connectivity
- Extension directory with user, extension, caller ID, registration/activity state and authorized actions
- Auto-refresh only through supported polling/events; display stale-data state when events stop.

Call analytics and reports:
- Date presets and custom date range
- Incoming/outgoing, total, answered, missed/no answer, busy and failed counts
- Answer rate and failure/miss rates with documented formulas
- Talk time, ring/dial time and wait time only when source data supports them
- Search by caller/called number, extension, agent, status and direction
- Permission-controlled recording playback/download
- CSV/XLSX/PDF export only when supported, with the same tenant/role filters as the UI
- All totals must reconcile with underlying CDR/call event records; never invent numbers.

## 6. Universal AI Model Hub

Create an extensible provider-adapter registry rather than hard-coding a static list of model names. Support compatible integrations, subject to API availability, access, pricing, region and model capability:
- OpenAI
- Anthropic Claude
- Google Gemini
- DeepSeek
- xAI Grok
- Alibaba Qwen
- Mistral AI
- Meta Llama-compatible inference endpoints
- Cohere and other compatible providers
- OpenRouter and other aggregators
- Together AI, Fireworks AI, Groq and similar inference platforms when compatible
- Self-hosted models through supported Ollama/vLLM/OpenAI-compatible endpoints
- Speech, transcription and voice providers such as OpenAI audio, ElevenLabs, Azure AI Speech, Google Cloud Speech, Deepgram and other configured compatible services

Do not claim every model supports every feature. Store capability metadata for text, structured output, tool calling, vision, audio input/output, transcription, streaming and embeddings. Validate provider APIs and model capabilities with a real health/test request before marking them healthy.

Provider administration:
- Server-side encrypted API credentials, never exposed in browser responses or logs
- Connection test, model discovery, health status, latency, errors and last-success timestamp
- Enable/disable provider and model per platform, reseller or tenant policy
- Provider routing, per-task model selection, quotas, cost ceilings, timeout, retry and fallback
- Respect provider terms, data residency/privacy requirements and customer data-sharing policy
- Usage accounting for tokens/requests/audio minutes and actual available pricing; label estimates clearly.
- Do not promise unlimited/free usage where the provider imposes limits or charges.

## 7. AI Agent Builder and runtime

Provide an Agent Builder for authorized admins:
- Agent name, purpose, system instructions and version
- Default model/provider and task-specific routing
- Allowed tools/actions from a server-side registry
- Knowledge sources and retrieval scope
- Memory policy and retention
- Tenant scope and permissions
- Per-run, per-user and per-tenant budget/rate limits
- Draft, test, publish, pause and version rollback
- Test playground with redacted traces
- Execution history, structured input/output, duration, provider, token/usage, cost where available, errors and retry status

Suggested built-in agents:
1. Customer Onboarding / Setup Agent
2. Extension Provisioning Agent
3. IVR Configuration Agent
4. CRM Sales Assistant
5. Call Summary and Disposition Assistant
6. Billing Support Assistant
7. PBX Troubleshooting Assistant
8. Call Center / Supervisor Assistant
9. Voice/TTS Assistant
10. Reporting Assistant

An LLM must never receive unrestricted shell, database, FreeSWITCH CLI or arbitrary URL-fetch access. All actions must use narrow, validated backend tools with explicit schemas and authorization.

## 8. Conversational Customer Setup Wizard

Customers may configure their own services through a guided, multi-turn assistant, subject to verification and entitlements.

### Interaction rules
- Ask one main question at a time, with small related subquestions only when useful.
- Maintain resumable drafts and previously answered fields; do not repeat known answers.
- Read existing permitted configuration first and ask only for missing information.
- Offer accessible buttons, select controls, checkboxes, upload controls and free-text answers.
- Show progress, allow back/edit, validate answers and ask clarification when ambiguous.
- Ask only questions relevant to purchased/enabled services.
- Generate a complete preview and list missing dependencies before provisioning.
- Do not invent DID numbers, SIP credentials, tariffs, gateways, credit limits, provider API keys or business identity data.
- Clearly separate a proposed plan from executed operations.

### Conditional question groups
- Customer/company profile and tenant/domain
- Purchased package, enabled services and permitted quotas
- Extension count/range, user assignments and role permissions
- Devices: IP phone, softphone, browser WebRTC or supported combination
- SIP gateway, caller ID, inbound DID and outbound route (only existing/authorized choices)
- Business hours, holiday schedule, IVR menu, destinations, retry/no-answer behavior and uploaded/TTS audio
- Recording scope, retention, storage and playback permissions
- Ring groups, queues, agent count, skills and supervisor options where enabled
- CRM integration, call screen pop and webhook/API integration where available
- Billing mode, assigned tariff, credit/recharge information and notification preferences, restricted by the customer's financial role
- TTS/voice provider, language, voice and budget where enabled
- Validation, preview, approval and final execution.

## 9. Customer verification and service activation

Verification states: Unverified, Email Verified, Mobile Verified, Business Review Pending, Verified, Rejected, Expired and Suspended.

- Email verification via secure, single-use, expiring token/OTP
- Mobile verification via rate-limited, expiring, single-use SMS OTP
- Reseller invitation with expiry, one-time use and revocation
- Optional business verification when required by risk, service policy or applicable regulation
- Avoid collecting government identity documents by default when not necessary
- Avoid account enumeration, brute force, OTP abuse and invitation replay
- Require step-up authentication or explicit approval for sensitive actions.

Verification does not grant unlimited permissions. Service activation must separately check package eligibility, role, tenant scope, gateway readiness, destination rules, billing/credit state and applicable approvals.

## 10. Provisioning engine and execution safety

All AI and non-AI setup paths must use the same validated, rule-based backend provisioning engine.

Operation states: Draft, Pending Validation, Awaiting Approval, Queued, Running, Succeeded, Failed, Partially Succeeded, Rolled Back and Cancelled.

- Validate all fields and dependencies before execution.
- Use idempotency keys and concurrency-safe extension/DID allocation.
- Support resumable workflows and retry only failed safe steps.
- Record per-step result, timestamps, correlation ID, actor, approval and audit event.
- Use compensating rollback for reversible actions; never automatically delete existing data or make unsafe rollback claims.
- Protect live calls from abrupt interruption.
- Verify actual PBX configuration after changes before reporting success.
- Require explicit confirmation/approval for production routing, billing/credit changes, outbound campaigns, destructive actions and other high-impact operations.
- Keep uploaded files tenant-scoped, validate MIME/type/size/duration/codec, store securely and enforce retention.
- Distinguish uploaded IVR prompt audio from actual call recordings.

## 11. Billing, tariff and report boundaries

Respect the existing product's independent tariffs for Voice Calling, Voice SMS, Outbound IVR, Bulk SMS, extensions/subscriptions, call recording and add-ons.

Support configured Prepaid, Postpaid and Hybrid modes without leaking unrelated billing information:
- Prepaid customers see authorized wallet/recharge/usage information, not postpaid-only debt/credit menus.
- Postpaid customers see invoices, due dates, outstanding balances and credit limit only where authorized, not prepaid-only wallet UI unless separately enabled.
- Hybrid customers see the allowed combined view.
- Enforce this on backend APIs, reports, exports, scheduled jobs and notifications, not just menus.
- Keep financial ledgers immutable; correct posted records through audited reversals/adjustments.
- Store the tariff version used for each billable event.
- Keep provider cost/margin hidden from unauthorized users.

## 12. Observability, approvals and audit

- Agent run timeline with input/output redaction and no secrets
- Provider/model health, latency, error rate and configured budget utilization
- Pending approval queue and reviewer history
- Customer provisioning progress and per-step failure details
- Audit actor, tenant, action, before/after summary, correlation ID and timestamp
- Notifications through email/in-app or other channels only when those integrations are actually configured
- Alerts for budget thresholds, repeated failures and provider outages.

## 13. Required acceptance tests

- Role and tenant isolation for every CRM, PBX, recording, AI, export and provisioning endpoint
- CRM caller-ID matching and exact "Unknown" fallback
- Click-to-call, inbound screen pop, call controls and event accuracy
- Extension status and report totals reconcile with actual source records
- Provider connection failures, invalid keys, rate limits, timeouts and fallback behavior
- Model capability mismatch and disabled provider behavior
- Secret redaction and encrypted credential storage
- Prompt injection attempts cannot grant tools or bypass RBAC
- Resumable setup wizard, changed answers, missing fields and invalid dependencies
- OTP expiry, replay, brute force and invitation revocation
- Duplicate provisioning prevention and safe recovery from partial failure
- Prepaid/Postpaid/Hybrid UI and backend isolation
- Recording access, retention and tenant isolation
- Accessibility, responsive layout, lint/build/tests and documented test results.

## 14. Delivery order

1. Audit repository/source access and document existing CRM, PBX, auth, API, database, event and FreeSWITCH capabilities.
2. Establish feature inventory, baseline tests and dependency map.
3. Implement role-aware CRM Sales Agent workspace and real click-to-call using existing supported APIs.
4. Implement real extension/call status and reporting.
5. Implement verified customer self-service wizard and deterministic provisioning engine.
6. Add provider registry and Model Hub with one tested provider first; add other provider adapters incrementally.
7. Add Agent Builder, tool registry, routing/fallback, usage, logs and approvals.
8. Add voice/TTS providers and additional built-in agents as supported.
9. Run tests and provide exact changed files, passed/failed checks, known limitations and rollback notes.

Do not attempt to deliver the entire platform by generating a mock dashboard. Prioritize working end-to-end vertical slices, preserving existing functionality and reporting truthfully what is and is not implemented.
