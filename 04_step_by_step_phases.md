# AI Implementation Phases

**CRITICAL INSTRUCTION FOR AI:** You must complete Phase 1 entirely, output the code, and WAIT for the user to say "Proceed" before starting Phase 2.

## Phase 1: Project Scaffolding
- Add dependencies (`sip_ua`, `flutter_webrtc`, `flutter_callkit_incoming`, state manager, permission_handler).
- Create the core folder structure.
- Create an initial "Permissions Check / Onboarding" wrapper that forces the user to grant Camera and Microphone permissions upon first installation before accessing the app.
- Scaffold the `BottomNavigationBar` and empty placeholder screens (Dialpad, Settings).
- Do not implement SIP yet. Ensure UI builds and runs without errors.

## Phase 2: Zoiper-style Account Onboarding & Registration
- Read both `06_account_management_specs.md` and `08_ui_ux_branding_specs.md`.
- Build the Initial Login UI meticulously applying the HOHIT Gold/Dark layout. Provide inputs for IP, Extension, and Password.
- Implement the "Probing" logic to detect WS vs WSS connection success and show live feedback to the user on the UI.
- Implement secure storage for these credentials and auto-register them seamlessly on future app launches.
- Once connected or verified via local storage, route to the Main layout.

## Phase 3: Outgoing Calls & Dialpad
- Build the `DialpadScreen` UI.
- Implement `SipService.makeCall()`.
- Create the `ActiveCallScreen` with Hangup and Mute buttons.
- Handle microphone permissions before calling.

## Phase 4: Incoming Calls & Native UI
- Listen for inbound INVITEs in `SipService`.
- Integrate `flutter_callkit_incoming`.
- Link native Answer/Decline buttons to the `sip_ua` call logic.
- Ensure audio route is established after answering natively.

## Phase 5: Call History & Logging
- Integrate `sqflite` and establish the local database schema.
- Implement SIP state listeners to detect Missed, Completed, and Failed calls.
- Calculate call duration precisely upon hangup.
- Build the `HistoryScreen` UI to display categorized call logs.

## Phase 6: Advanced Call Features (Transfers & Hold)
- Read `07_call_transfers_specs.md`.
- Implement SIP Hold/Unhold toggle gracefully managing media streams.
- Add Blind Transfer logic using SIP REFER.
- Update global state to handle multiple concurrent calls.
- Implement Attended Transfer, ensuring appropriate `Replaces` headers are used to merge calls on the PBX.
