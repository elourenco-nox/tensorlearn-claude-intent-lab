# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18, the current approval),
`inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md`
(2026-08-02) is superseded by PRD N-17 and is historical context only: nothing in this intent is
approved on its authority.

## Problem

- **I-1** The fictional clinic receives avoidable calls from patients checking whether their
  results are ready. PRD: *"The fictional clinic receives avoidable calls from patients checking
  whether results are ready."*
- **I-2** Today patients have no signal that a reviewed result is waiting for them, so they phone
  to ask. These status-only calls are distinct from calls with clinical questions (support lead,
  discovery notes).

## Proposed outcome

- **I-3** The patient is notified when a result is ready for their review in the existing
  authenticated portal. PRD: *"Notify the patient when a result is ready for their review in the
  existing authenticated portal."*
- **I-4** "Ready" means a doctor has reviewed **and** released the result. Lab upload alone never
  makes a result ready. PRD: *"A doctor must review and release the result before any notification
  is eligible."* Product owner: *"'Ready' must not mean that the laboratory upload merely finished."*
- **I-5** The message only says that an update is available and directs the patient to the
  existing authenticated portal.

### How success will be observed

- **S-1** Results-status calls, counted separately from clinical questions, decrease compared with
  a measured baseline. Support lead: *"count status-only calls separately from clinical questions;
  compare against a measured baseline."*
- **S-2** Delivery failures are measured and tracked. PRD: *"Measure reduction in results-status
  calls and delivery failures."*
- **S-3** No baseline or numeric target is approved yet. PRD: *"The baseline and numerical target
  will be agreed during discovery; none is approved yet."* The 90% figure in the superseded request
  is not a target. See OPEN-3.

## Affected users and systems

- **Patients**: the only approved recipients of the notification.
- **Doctors**: their review-and-release step is the trigger gate.
- **Support staff**: handle the calls being reduced and classify status-only calls versus
  clinical questions.
- **Existing authenticated portal and its result route**: where the patient views the result.
  Engineering lead: *"the existing portal path is adequate."*
- **Notification delivery and its operational logs**: the channel itself is not yet chosen (OPEN-1).
- **Product, privacy, clinical, engineering and support owners**: decision owners named in the
  sources.

## Out of scope

- **X-1** Portal redesign (PRD scope).
- **X-2** A new or rebuilt patient dashboard. Engineering lead: *"A new patient dashboard is a
  separate project."*
- **X-3** Appointment scheduling (PRD scope).
- **X-4** Clinical interpretation (PRD scope).
- **X-5** Notifying third parties (PRD scope). Notifying the referring doctor is not approved and
  stays open (OPEN-2). It is excluded from this intent until it is decided.
- **X-6** Everything that only the superseded request asked for: sending on lab upload, putting the
  test result in an SMS, copying the referring doctor, rebuilding the dashboard, and a 90%-in-one-week
  target.

## Constraints

- **C-1 Review before notification.** No notification is eligible until a doctor has reviewed and
  released the result (PRD; product owner).
- **C-2 No clinical detail.** The message and the notification logs contain no diagnosis, test
  result, clinical measurement or treatment detail. They only say that an update is available and
  point to the portal. Privacy owner: *"no sensitive content in messages or operational logs."*
- **C-3 Authenticated access only.** The result is viewed only after authentication in the existing
  portal. No attachments and no public result link (PRD).
- **C-4 No duplicates.** There is at most one notification per released result version. Delivery
  failures are handled without sending the patient duplicate messages (PRD).
- **C-5 Existing route.** Use the existing portal result route (PRD scope; engineering lead).
- **C-6 Recipients.** The patient is the only approved recipient. No other recipient may be added
  without the decision in OPEN-2.

## Open questions

- **OPEN-1 Channel** (SMS, email or portal push). **Owners: product owner and privacy owner**,
  after consent and delivery review. PRD: *"product owner and privacy owner must decide after
  consent/delivery review."* Privacy owner: *"Consent and the actual channel are still under
  review."*
- **OPEN-2 Referring-doctor notification**: whether the referring doctor may be notified, and how
  that recipient is authorized. **Owners: clinical owner and privacy owner.** It is not approved.
  PRD: *"It is not approved by this PRD."* Clinical owner: *"do not infer approval for
  referring-doctor notifications from the old request."*
- **OPEN-3 Baseline and numeric target** for the reduction in status-only calls. Per the PRD, these
  will be agreed during discovery. None is approved, and the sources do not name a specific owner
  for this decision.

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
