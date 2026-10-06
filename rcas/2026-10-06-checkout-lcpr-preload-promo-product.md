---
status: Approved
version: v1
source_message_id: 1a10fe88098a271a
source_thread_id: 1a10fe88098a271a
generated: 2026-10-06
approved_by: saurabh1.thakur@paytm.com
approved_at: 2026-10-06T12:29:12+05:30
---

# RCA: Preload Promotional Product in the Checkout LCPR Experiment

## Date
- Original RCA email: Wednesday, August 12, 2026, 1:01 PM (Manish Negi)
- LLA decision: Thu, Aug 13, 2026, 3:41 AM (Fabian Beltran)
- Paytm update: Thu, Aug 13, 2026, 7:36 PM (Manish Negi)
- Forwarded to Rishabh: 2026-10-06, 12:00 PM IST

## Source
- Forwarded by: Saurabh Thakur <saurabh1.thakur@paytm.com>, "FYI"
- Original RCA author: Manish Negi <manish1.negi@paytm.com>, Associate, Paytm
- Counterpart: Fabian Beltran <fabian.l.beltran@lla.com>, Digital Experience Associate Manager, Liberty Latin America
- Subject: "Fwd: [EXT] RCA & Proposed Solutions – Preload Promotional Product in the Checkout LCPR experiment."
- Related references: DSM-1468 (flow), Kameleoon experiment 408559 (reference experiment), variant 20260724_NEW_FIX
- Attachments: one inline image (img-fcefbad5-….png). It was not used for this document.

## Summary
After the latest deployment, the Digital team changed the data flow architecture. Most journey-related data now comes directly from APIs and the Redux store instead of Local Storage. The Preload Promotional Product experiment in Checkout LCPR was built to depend on Local Storage values, so it can no longer get the journey data it needs. This breaks parts of the experiment, especially Plan Switching. Only this direct-to-checkout experiment is affected, because its users skip the PLP and Add to Cart steps where the journey data is normally initialized. LLA approved Option 1: simulate cart creation / journey data initialization and remove the Change Plan functionality. Paytm implemented it as the new variant 20260724_NEW_FIX.

## Impact
- Required journey data is no longer consistently available in Local Storage.
- The experiment cannot access the expected values during execution.
- Plan Switching and other dependent functionality are affected.
- The experiment can behave inconsistently or fail at certain stages of the journey.
- Scope: only the direct-to-checkout experiment (Landing Page / Meta Ads → Direct Checkout). Experiments on the standard journey (PLP → Select Plan → Add to Cart → Checkout) are not affected.
- Number of affected users/sessions and business impact: Not stated in source.

## Timeline
- **"Latest deployment" (date not stated)**: The Digital team changed the data flow architecture from Local Storage to APIs + Redux.
- **Wed, 12 Aug 2026, 1:01 PM**: After a discussion with Fabian, Manish Negi shared the RCA and proposed solution, and asked for a decision on removing Change Plan.
- **Thu, 13 Aug 2026, 3:41 AM**: Fabian Beltran approved Option 1, shared reference experiment Kameleoon 408559, and asked for an estimated timeline.
- **Thu, 13 Aug 2026, 7:36 PM**: Manish Negi confirmed Option 1 was implemented, with the QA link and evidence video posted on the Jira ticket, and the new variant 20260724_NEW_FIX ready for review. He also noted how the reference experiment's journey differs.

## Root Cause
- The experiment was built to depend on values stored in Local Storage. After the latest deployment, the Digital team moved most journey-related data to API responses and the Redux store, so several of those values are no longer in Local Storage.
- The experiment sends users straight from Landing Page / Meta Ads to Checkout, skipping the PLP and Add to Cart steps. Under the new architecture, those steps are where plan/product journey data gets initialized. So that data is missing when the experiment starts running on the Checkout page.

## Resolution / Fix
- **Option 1 (approved by LLA, implemented by Paytm):** API/Redux integration / simulate cart creation. Manually simulate the cart creation process by triggering the required API endpoints in the background. The source also describes another workaround: route Meta Ads users to the Flow Plan page, load it hidden in the background, and use JavaScript events to simulate the button clicks, so journey data is initialized before Checkout.
- **Limitation:** With this approach, the existing Change Plan functionality cannot be supported in its current form and has to be removed. The source flagged this as a market approval blocker, since it removes user-facing functionality the market had already approved. LLA (Fabian Beltran) explicitly approved proceeding with Option 1, including removal of Switch Plan / Change Plan.
- **Status:** New variant 20260724_NEW_FIX implemented. QA link and evidence video posted on the Jira ticket for LLA to check and confirm.
- **Reference experiment review:** Paytm reviewed Kameleoon 408559. In that experiment, users start at the PLP, go to Personal Information, and skip Mobile Line configuration. In this use case (DSM-1468), users come directly from Meta into checkout with the promotional product already preloaded.

## Preventive Actions / Action Items
| # | Action | Owner | Status |
|---|---|---|---|
| 1 | Build the new variant that simulates cart creation / journey data initialization and removes Change Plan (Option 1) | Paytm team (Manish Negi) | Done: variant 20260724_NEW_FIX ready, per source |
| 2 | Review the QA link and evidence video on the Jira ticket and confirm | LLA (Fabian Beltran) | Pending, per source |
| 3 | Assess whether a similar approach to reference experiment Kameleoon 408559 could be applied | Paytm team | Reviewed; key journey difference identified |
| - | Preventive actions to avoid recurrence (e.g., experiments depending on Local Storage after architecture changes) | Not stated in source | - |

## Open Questions
- Did LLA confirm the 20260724_NEW_FIX variant after QA review? Not stated in source.
- Exact date of the "latest deployment" that changed the data flow architecture: Not stated in source.
- Impact metrics (affected sessions, conversion impact, duration): Not stated in source.
- Preventive measures for other experiments that may depend on Local Storage: Not stated in source.
- Jira ticket ID for the QA link/evidence: not explicitly stated (DSM-1468 is mentioned as the flow).
