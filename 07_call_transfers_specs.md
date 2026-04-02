# Call Transfers (Blind & Attended)

To properly clone Zoiper / Enterprise Softphone functionalities, the app must handle SIP REFER methods for transferring calls.

## 1. Blind Transfer (Cold Transfer)
A Blind Transfer routes the active call directly to a third party without giving the user a chance to speak to them first.

**Implementation Logic (`sip_ua`):**
- UI: In the `ActiveCallScreen`, tap "Blind Transfer". Pop up a Dialpad or Number input dialog.
- Action: The user inputs the target extension (e.g., `1002`).
- SIP Command: Trigger `activeCall.refer('sip:1002@[PBX_IP]')`.
- Asterisk handles the SIP REFER and disconnects the original caller from you. Listen to the call state to gracefully close the UI.

## 2. Attended Transfer (Warm Transfer)
An Attended Transfer puts the original caller on hold, calls the third party privately, discusses the handoff, and then connects the two parties together.

**Implementation Logic (`sip_ua`):**
- **State Requirement:** The Global State Manager (Riverpod) MUST support handling a `List<Call>` rather than just one active call.
- **Step 1 (Hold):** User presses "Attended Transfer". Execute `callA.hold()` (This sends a SIP Re-INVITE with `sendonly` or `inactive` SDP).
- **Step 2 (Call Target):** Open Dialpad, create a *new* call (`callB`) to `sip:1002@[PBX_IP]`. WebRTC audio routes to `callB`.
- **Step 3 (Execute Transfer):** User speaks to the new target. To finalize the transfer, use the `refer` method on `callA`, passing `callB` as the target context (Asterisk requires the `Replaces` header to merge them). *Syntax depends strictly on `sip_ua`'s implementation of Attended Refer.*
- **Step 4 (Cleanup):** Hang up local calls once Asterisk acknowledges the REFER with a 202 Accepted / NOTIFY.

## UI Requirements
- The `ActiveCallScreen` needs a prominent **Hold** toggle.
- When multiple calls are active, the UI must show "Call 1 (On Hold)" and "Call 2 (Active)", letting the user quickly swap between them via `hold()` and `unhold()`.
- Add visual toasts for "Transfer Successful" or "Transfer Failed".
