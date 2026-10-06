---
status: Approved
version: v1
source_message_id: 1a10fe78d700bc48
source_thread_id: 1a10fe78d700bc48
generated: 2026-10-06
approved_by: saurabh1.thakur@paytm.com
approved_at: 2026-10-06T13:02:14+05:30
---

# RCA: Jul 30 Super App Login Services Degradation (Panama)

## Date
- Incident date: 30-Jul-2026
- Initial RCA shared by LLA: Sat, Aug 1, 2026, 7:15 AM (as shown in the forwarded header)
- Forwarded to Rishabh: 2026-10-06, 12:00 PM IST

## Source
- Forwarded by: Saurabh Thakur <saurabh1.thakur@paytm.com>, "FYI"
- Original sender: Camilo Franceschi <Camilo.Franceschi@lla.com>, Digital Platforms Operations Sr. Manager, Liberty Latin America
- Subject: "Fwd: RCA - Jul 30 Super App Login services degradation"
- Attachment: InitialRCA_LoginDegratation-Jul30.docx ("Incident Summary – Initial RCA")
- Tracking ticket: DSDT-4523
- Note from source: this is the **initial** RCA and not yet final. Paytm is still reviewing the proposed corrective and preventive actions, and LLA said it would share a revised version.

## Summary
An automated infrastructure upgrade triggered a chain of failures that made the MetaC SuperApp login service unavailable for over 12 hours to Panama customers without an active session. A P1 alert fired in Dynatrace within minutes, but it was dismissed and not acted on for about 10.5 hours. After LLA opened an incident bridge, the proxy services were redeployed and login was restored.

| Field | Value |
|---|---|
| Incident Date | 30-Jul-2026 |
| Market | Panama (PA) |
| Severity | P1 — Critical |
| Duration | 12 hours 8 minutes (00:12 AM – 12:20 PM EST) |
| Service Impacted | MetaC SuperApp — Login & Registration |
| Status | Resolved |

## Impact
- No Panama customer who tried to sign in to the SuperApp during the outage window could access their account. Users who entered their credentials got an error message with no sign of a service disruption. This left them in a loop, without access to any service behind login, for the full 12-hour outage.

| Metric | Value |
|---|---|
| Total user sessions during degradation | 12.53k |
| Login attempts blocked / failed during outage | 7991 |
| Estimated unique users affected during outage window | TBD |

## Timeline
(Times are as given in the source. The source labels the outage window as EST.)
- **00:12 AM**: Dynatrace generated an automated P1 alert for active service degradation (incident start).
- **00:23 AM**: Someone briefly reviewed the alert and dismissed it by mistake because pods looked healthy at the infrastructure level. Nobody confirmed the login flow worked for customers.
- **00:23 AM – ~11:00 AM**: No escalation, no follow-up action or tracking, and no continued monitoring. The alert stayed unresolved and untracked for about 10.5 hours.
- **11:00 AM**: The LLA team opened an incident bridge.
- **~11:00 AM – 12:20 PM**: The proxy services were redeployed from the last released branch. The fix went in within 80 minutes.
- **12:20 PM EST**: Full service availability for Panama confirmed.

## Root Cause
- **Trigger:** An automated infrastructure upgrade triggered a sequence of failures that made the login service unavailable. The source does not describe the exact technical failure mechanism. Its follow-up actions mention RDS minor-version auto-upgrades and an "RDS auto-upgrade misconfiguration", and say that proxy services needed a full redeployment to reload their configuration correctly.
- **Primary failure, per the source (extended duration):** The source says the root cause of the long outage was the failure to detect and respond, not the infrastructure failure itself. A P1 alert fired within minutes but was dismissed based on pod health alone. Nobody escalated, tracked, or monitored it for about 10.5 hours.
- **Still under investigation:** Why a full redeployment, rather than a simple pod restart, was needed to fix the configuration issue. This will be reproduced in a lower environment.

## Resolution / Fix
- After escalation at 11:00 AM, the proxy services were redeployed from the last released branch. This made them reload their configuration correctly and restored the login flow. Full availability was confirmed at 12:20 PM EST.

## Preventive Actions / Action Items
| # | Action | Owner | Target Date | Status |
|---|---|---|---|---|
| 1 | Mandatory alert ownership & closure tracking: a Slack bot re-notifies the on-call engineer every 15 min until the P1 alert is acknowledged and resolved. | Paytm Support | 15-Aug-2026 | Open |
| 2 | Disable RDS minor-version auto-upgrades across all environments. Allow upgrades only in planned, approved maintenance windows. | PayTM DevOps team | TBD | Open |
| 3 | Add post-restart functional validation (login flow smoke test) to the restart runbook, not just infrastructure health-check probes. | Paytm Support | 07-Aug-2026 | Open |
| 4 | Reproduce and root-cause why a full proxy redeployment, rather than a pod restart, was needed to reload configuration correctly. Recertify in a lower environment. | Paytm DevOps / Dev | TBD | Open |
| 5 | Scan all markets and environments for the same RDS auto-upgrade misconfiguration and fix it. | PayTM DevOps team | TBD | Open |

## Open Questions
- Estimated unique users affected: TBD in source.
- Committed target dates for actions 2, 4 and 5: TBD in source.
- Why a full redeployment rather than a pod restart was needed: under investigation.
- Exact technical failure mechanism of the infrastructure upgrade: not stated in source beyond the references to RDS auto-upgrades.
- Final/revised RCA version: the source says Paytm is still reviewing corrective/preventive actions and a revised version will follow. Whether it has been shared is not stated in source.
- Time zone for the 00:12 / 00:23 / 11:00 timestamps: the source only labels the window and 12:20 PM as EST.
