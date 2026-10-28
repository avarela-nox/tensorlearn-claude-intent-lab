# Intent: results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded history and approves nothing here.

## Problem

I-1. The fictional clinic receives avoidable calls from patients who only want to know whether their results are ready. Patients have no signal telling them when a result is available for their review.

## Proposed outcome

I-2. When a result is ready for a patient's review, that patient is told an update is available and is directed to the existing authenticated portal, so they no longer need to call to ask.

I-3. A result is eligible for notification only after a doctor has reviewed it and released it. "Ready" does not mean the laboratory upload has finished; upload completion alone never triggers a notification.

I-4. The message says only that an update is available. It contains no diagnosis, test name, test result, clinical measurement or treatment detail, and none of these appear in operational logs either. Results are not attached and there is no public result link; the patient authenticates in the existing portal.

I-5. The same released result version never produces duplicate notifications, and handling a delivery failure must not send the patient duplicate messages.

I-6. How success will be observed (no number is approved):
- Calls that are only about results status go down. Support counts these separately from clinical questions and compares them against a measured baseline.
- Delivery failures are tracked and observed.
- The baseline and any numerical target are still to be agreed during discovery. None is approved, and the sources assign no owner for setting them.

## Affected users and systems

I-7. Patients receiving the notification; the clinic's support staff who take the status calls; doctors, whose review and release gates every notification; and the product, privacy, clinical and engineering owners named below.

I-8. Systems: the existing authenticated portal and its existing result route, and the notification delivery path. Nothing new is built for viewing results.

I-9. The only approved recipient is the patient the result belongs to. Any other recipient, including the referring doctor, is not approved and is not silently approved by this intent. See OPEN-2.

## Out of scope

I-10. Excluded from this approved scope:
- Portal redesign.
- A new patient dashboard, which engineering treats as a separate project.
- Appointment scheduling.
- Clinical interpretation.
- Notifying third parties.
- Sending on lab upload, putting a test result in the message, and the 90% call-reduction goal in one week. These come only from the superseded early request and are not approved.

## Constraints

I-11. A doctor must review and release the result before any notification is eligible.

I-12. No sensitive or clinical content in messages or operational logs.

I-13. Access is authenticated in the existing portal. No attachments and no public result link.

I-14. Duplicate notifications for the same released result version are avoided, and delivery failures are handled without duplicate patient messages.

I-15. The existing portal result route is used.

I-16. Consent and the actual channel are still under review, so nothing may assume a channel yet.

## Open questions

OPEN-1. Channel (SMS, email or portal push). Owners: product owner and privacy owner, after the consent and delivery review. Until decided, no channel is approved.

OPEN-2. Notification to the referring doctor. Owners: clinical owner and privacy owner, who must decide recipient authorization. It is not approved by the PRD and must not be inferred from the superseded request. Until decided, no one other than the patient receives a notification.

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
