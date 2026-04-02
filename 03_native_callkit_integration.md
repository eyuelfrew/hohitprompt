# Native CallKit & ConnectionService

Building a softphone without native integration results in a poor user experience. The app MUST route incoming calls natively to Android ConnectionService (TelecomManager).

## The Integration Flow
1. **Background/Foreground INVITE:** `sip_ua` receives an incoming call event from the WebSockets socket.
2. **Trigger CallKit:** Your app triggers `FlutterCallkitIncoming.showCallkitIncoming()` with the caller's ID and name.
3. **User Action:** The OS takes over the screen.
   - If User Accepts natively: Listened via `FlutterCallkitIncoming.onEvent` (`ACTION_CALL_ACCEPT`). Call `sip_ua`'s `call.answer()`.
   - If User Declines natively: Listen for `ACTION_CALL_DECLINE`. Call `sip_ua`'s `call.hangup()`.
   - If App UI Hangs up: Call `FlutterCallkitIncoming.endCall()`.

## Important Caveat
- Background push notifications (FCM) are out-of-scope for the early phases, but ensure your `SipService` logic is cleanly detached so it can eventually be triggered by a headless background isolate via FCM in the future.
