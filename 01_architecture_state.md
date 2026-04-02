# Architecture & State Management

## Directory Structure
Adopt a Clean Architecture / Feature-first approach.

```text
lib/
├── core/
│   ├── network/        # WSS network & connectivity monitors
│   ├── services/       # SipService, AudioService, CallKitService
│   ├── providers/      # Global state providers (SIP state, Call state)
│   └── utils/          # Helpers, audio permission handlers
├── features/
│   ├── dialpad/        # Dialpad UI, digit entry
│   ├── history/        # Call history logs
│   ├── settings/       # Form for SIP credentials (WSS URI, username, password)
│   └── active_call/    # In-call UI (Timer, Mute, Speaker, Hangup)
└── main.dart           # Initialization, CallKit background handlers
```

## State Management Rules
- **SIP State:** Expose registration states (`Unregistered`, `Registering`, `Registered`, `RegistrationFailed`). The UI (like an AppBar icon) must react to this stream globally.
- **Call State:** Expose the active Call object. Different phases (`CALL_INITIATION`, `RINGING`, `CONNECTED`, `ENDED`) must trigger navigation (e.g., popup the active call screen).
