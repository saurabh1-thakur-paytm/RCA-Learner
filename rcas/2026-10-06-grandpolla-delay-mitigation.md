---
status: Approved
version: v2
source_message_id: 1a10fe9d7cb53403
source_thread_id: 1a10fe9d7cb53403
generated: 2026-10-06
approved_by: saurabh1.thakur@paytm.com
approved_at: 2026-10-06T12:39:56+05:30
---

# RCA: GrandPolla Delay & Mitigation Plan (MasPolla)

## Date
- Original RCA email from Rahul Misra: Thursday, 02 July 2026, 14:09:29
- Follow-up from Tatiana Morisson (LLA): Sat, Jul 4, 2026, 8:20 AM (as shown in the forwarded header)
- Forwarded to Rishabh: 2026-10-06, 12:02 PM IST

## Source
- Forwarded by: Saurabh Thakur <saurabh1.thakur@paytm.com>, "FYI"
- Original RCA author: Rahul Misra <rahul.misra@paytm.com>, Senior Product Manager, Paytm
- Latest message in the forwarded chain: Tatiana Morisson <Tatiana.Morisson@lla.com>
- Subject: "Fwd: [EXT] RCA - Grandpolla Delay & Mitgation plan"
- Attachments: none

## Summary
GrandPolla was a new feature meant to deliver the complete MasPolla experience, with leaderboard-based competition and exact score predictions. Before it, users outside an active pool could only make simple predictions. The market team planned to use leaderboard rankings for offline rewards and gratification. The feature was taken up after alignment with the technical team. However, nobody foresaw its impact on the production database under real-world load, and it affected the overall MasPolla experience. GrandPolla's launch was therefore delayed. The business mitigation is to launch it at the Round of 16 late on Friday night, Panama time.

## Impact
- The production database's behaviour under real-world load affected the overall MasPolla experience.
- The GrandPolla launch was delayed (per subject and mitigation plan).
- Number of affected users, duration and severity: Not stated in source.

## Timeline
- **Before deployment**: The feature was taken up after discussion and alignment with the technical team. No technical risk was identified or anticipated before deployment.
- **After deployment (date not stated)**: Impact on the production database under real-world load affected the overall MasPolla experience.
- **Thu, 02 Jul 2026, 14:09**: Rahul Misra (Paytm) sent the RCA and business mitigation plan to LLA. He said Suneel Yadav could provide a more detailed technical RCA.
- **Sat, 04 Jul 2026, 8:20 AM**: Tatiana Morisson (LLA) replied that it was Friday night and LLA had received neither the plan nor a formal ICM request. The market had communication scheduled for the next morning. She asked for the plan ASAP, or confirmation that the feature would not be available so the market could cancel its notification.

## Root Cause
- Per the source: the impact on the production database under real-world load was unforeseen, and no technical risk was identified or anticipated before deployment.
- Detailed technical root cause (production database behaviour): Not stated in source. Suneel Yadav was named to provide the detailed technical RCA.

## Resolution / Fix
- Business mitigation plan: launch GrandPolla at the Round of 16 late on Friday night, Panama time, so users can still compete for the grand prizes.
- Technical fix: Not stated in source.

## Preventive Actions / Action Items
| # | Action | Owner | Status |
|---|---|---|---|
| 1 | Provide a more detailed technical RCA and mitigation plan, and explain the production database behaviour that caused the issue | Suneel Yadav (suneel.yadav@paytm.com) | Not stated in source |
| 2 | Send the plan and formalize the ICM request, or tell the market the feature won't be available so it can cancel its notification (requested by LLA) | Requested of Rahul Misra | Not stated in source |
| - | Preventive actions to avoid recurrence | Not stated in source | - |

## Open Questions
| # | Question | Owner |
|---|---|---|
| 1 | What was the specific production database behaviour and technical root cause? (Detailed RCA pending from Suneel Yadav.) | Suneel Yadav (suneel.yadav@paytm.com), named in the source to provide the detailed technical RCA |
| 2 | Was the detailed technical RCA / mitigation plan delivered, and was the ICM request formalized? Not stated in source. | Technical RCA: Suneel Yadav (suneel.yadav@paytm.com); plan / ICM request: Rahul Misra (rahul.misra@paytm.com), to whom LLA's request was addressed |
| 3 | Did GrandPolla launch at the Round of 16 as planned? Not stated in source. | Not stated in source (owner to be assigned) |
| 4 | What was the user impact (affected users, duration)? Not stated in source. | Not stated in source (owner to be assigned) |
| 5 | What preventive actions (e.g., load testing before launch) are planned? Not stated in source. | Not stated in source (owner to be assigned) |
