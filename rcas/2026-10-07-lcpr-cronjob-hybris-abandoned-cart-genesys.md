---
status: Approved
version: v2
approved_by: saurabh1.thakur@paytm.com
approved_at: 2026-10-07T12:11:46+05:30
source_message_id: 1a11511b8c46dd5d
source_thread_id: 1a11511b8c46dd5d
generated: 2026-10-07
---

# RCA: Faulty CronJob Run on Hybris – LCPR Abandoned Cart Genesys Notification (METAC-29236)

## Date
- Incident window (production): 13 Jul 2026, 22:54 EST – 14 Jul 2026, 14:15 EST (~15 hours)
- CronJob disabled in Production: 14 Jul 2026 ~13:55 EST (per LLA-facing RCA sequence); containment confirmed as of RCA date 19 Jul 2026
- Original internal RCA threads: Jul 20, 2026 (Rajdeep → leadership; Suneel reply with LLA RCA doc)
- Forwarded to Rishabh: 2026-10-07, 12:03 PM IST (approx.)

## Source
- Forwarded by: Saurabh Thakur <saurabh1.thakur@paytm.com>
- Subject: "Fwd: RCA for cronjob issue"
- Original chain authors: Rajdeep Singh Bisen <rajdeep.bisen@paytm.com>; Suneel Yadav <suneel.yadav@paytm.com> (pointed to LLA-facing RCA doc)
- Ticket: METAC-29236
- Market / tenant: LCPR
- Linked docs used: [RCA_CronJob_Hybris](https://docs.google.com/document/d/1Kq8VyPx-tsZmc_0sk3oc9ue1h9TDHbjuHgO7upg99qw) (Suneel / LLA); [RCA for CronJob Hybris METAC-29236](https://docs.google.com/document/d/1Izd14rER6ujkY66fvrDSLSo5oYnmI-FPlSU_qGRJ6Zs) (Rajdeep format; prepared by Saurabh Thakur, 19 Jul 2026)
- Attachments on the forwarded mail: none (content in the Docs above)

## Summary
The `lcprAbandonedCartGenesysNotificationCronJob` on Hybris implements the LCPR–Genesys abandoned-cart notification process. It was meant to run hourly and select only carts inactive for more than 30 minutes since the last successful run (based on `timeModified`), without deleting carts from Hybris. After production deploy on 13 Jul 2026, defective date-boundary / last-run logic caused every execution to re-select the full historical abandoned-cart backlog (back to 23 Sep 2023) and push those records to the Genesys S3 bucket. The job also ran about every 2 hours instead of hourly. Over ~15 hours and 8 executions, ~299,833 records were pushed—mostly duplicate/stale leads—until the Contact Center escalated customer complaints and the CronJob was disabled in Production. Fix identified; pending S1 validation before re-enable in Production.

## Impact
- Severity: P1 – Critical (per source)
- Market: LCPR
- Offer creation, targeting, campaign execution, and customer eligibility: not impacted (isolated to abandoned-cart Genesys notification)
- ~299,833 records pushed to Genesys S3 during the faulty window
- Contact Center: high volume of duplicate/stale leads; customer confusion (“why was I contacted”); avoidable agent workload; reputational risk in LCPR
- Back-office/CMS UI: not affected

## Timeline
- **13 Jul 2026, 22:50 EST**: CronJob deployed to Production (hourly run, 30-minute inactivity window).
- **13 Jul 2026, 22:54 EST**: Job begins executing on a ~2-hour cadence and re-selects full historical backlog on each run.
- **13 Jul 2026 22:54 EST – 14 Jul 2026 14:15 EST**: ~299,833 records pushed to Genesys S3.
- **14 Jul 2026, ~13:25 EST**: Production support email; Contact Center customer complaints; investigation escalated.
- **14 Jul 2026, ~13:45 EST**: Defective date-boundary logic found (last successful run marker not tracked/applied).
- **14 Jul 2026, ~13:55 EST**: CronJob disabled in Production; faulty run ends after ~15 hours.
- **19 Jul 2026**: Formal RCA dated / prepared (per internal doc header).
- **20 Jul 2026**: Rajdeep shares Annicka-format RCA; Suneel points to LLA-facing RCA doc for sharing with LLA.

## Root Cause
Defective date-boundary logic in the CronJob’s cart-selection query: it did not correctly track or apply the “last successful run” marker, so every run treated the entire historical cart dataset as eligible instead of only carts abandoned since the previous execution. Logic defect (not infrastructure, performance, or capacity).

## Contributing Factors
- Unit testing did not cover `timeModified` / last-run boundary conditions or historical-data re-run behavior.
- No integration testing on S1 before production deployment.
- Requirements and testing scope not clearly communicated to QA for thorough edge-case coverage.
- No post-deployment monitoring for abnormal run frequency or record volume; issue surfaced via Contact Center complaints.

## Resolution / Fix
- Immediate: CronJob disabled in Production (containment).
- Faulty query logic identified (incorrect last-run / date-boundary tracking).
- Pending: fix deploy to S1 for validation, then re-enable in Production after monitored validation.
- Status per source: partially resolved / CronJob disabled; fix pending S1 then Production.

## Preventive Actions / Action Items
| # | Action | Owner | Status |
|---|---|---|---|
| 1 | Fix CronJob query to select only carts where `timeModified` exceeds 30 minutes since last successful run | Development Team | In Progress (fix identified) |
| 2 | Add unit tests for boundary/edge scenarios (fresh carts, under/over 30-min threshold, re-run after failure, historical data) | Development Team | Not Started (before S1) |
| 3 | Full integration testing on S1 before future production deploy of this cron | Development Team / QA Team | Not Started |
| 4 | Share detailed requirements and test scenarios with QA; require QA sign-off before deployment | Development Lead | Not Started (process change) |
| 5 | Mandatory post-deployment monitoring checklist (first 24–48 hours) for new/changed cron jobs | Development Team | Not Started (process change) |
| 6 | Architecture/design review for high-volume batch/cron jobs covering date-boundary and re-run logic | Development Team | Not Started |
| 7 | Code-review checklist: verify last-successful-run tracking and boundary logic for scheduled jobs | Development Team | In Progress |
| 8 | Monitoring/alerting on cron execution frequency and record-count-per-run | Development Team | Not Started |

## Open Questions
| # | Question | Owner |
|---|---|---|
| 1 | Has the date-boundary fix been validated on S1 and re-enabled in Production since the Jul 2026 RCA? Not stated in source. | Dev Team (per Saurabh Thakur review) |
| 2 | Was Genesys / Contact Center cleanup of the ~299,833 stale leads completed? Not stated in source. | Support team (per Saurabh Thakur review) |
| 3 | Exact record volume expected for a healthy hourly run after the fix? Not stated in source. | Support team (per Saurabh Thakur review) |
