# SIP & WebRTC Implementation Details

## SIP Configuration (`sip_ua`)
- Create a `SipService` singleton/provider wrapping `SIPUAHelper`.
- Configuration requires:
  - `WebSocket URL` (wss://...)
  - `SIP URI` (sip:user@pbx)
  - `Password`
  - `Display Name`
- Set `session_timers` and enable `keep_alive_interval` (or options ping) to keep the NAT/Firewall port open.

## Audio Routing & WebRTC (`flutter_webrtc`)
- When creating a call, set `mediaConstraints` to `{'audio': true, 'video': false}`.
- WebRTC handles RTP streams automatically. Bind the generic remote stream to the local audio output.
- Implement an `AudioService` (or handle inside `SipService`) to switch between Earpiece (default for calls) and Speakerphone.
- **Permissions (First Launch):** Upon the VERY FIRST app launch (before loading the main UI and SIP service), automatically request `Permission.camera` and `Permission.microphone` using the `permission_handler` package. You will also need Android 13+ Notification/Phone state permissions. If denied, display a rationale explaining why they are needed for VoIP.
