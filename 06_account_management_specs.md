# Account Management & Onboarding Flow

## Account Credentials Model
Store the account securely. The user model should include:
- `serverIp` (IP or FQDN of the PBX)
- `extension` (The SIP user)
- `password`
- `transportProtocol` (enum or string: WS, WSS)

## The First-Time Login (Zoiper Flow)
If no local account exists in local storage, route the user to the Account Onboarding UI rather than the dialpad:

1. **Input Phase:** 
   - Provide clean inputs for **Hostname/IP**, **Extension**, and **Password**.
   - No need to ask the user for "WSS URI" or port configs; the app should do the heavy lifting.

2. **Probing Phase (Zoiper style):** 
   - When the user hits "Login", show a loading state mimicking Zoiper's transport network detection.
   - Take the `serverIp` and proactively attempt to build connections (since `sip_ua` relies on WebSockets, try `wss://[serverIp]:8089/ws` for TLS, and if that fails, try `ws://[serverIp]:8088/ws` for unencrypted TCP/UDP fallback).
   - Attempt SIP registration via `sip_ua`.

3. **Detection Feedback:** 
   - While connecting, show UI labels to the user: e.g., "Probing TLS/WSS..." or "Probing WS...".
   - If the registration succeeds via WSS, display a green check text: "Success: Secured via TLS".
   - If it succeeds via WS, display a yellow check text: "Success: Unsecure UDP/TCP WebSocket".
   - If all probes fail, show a red error: "Registration failed. Check IP/Credentials or ensure the Asterisk server is configured for WebSockets".

4. **Completion:** 
   - Upon successful registration, securely save the credentials and the *successful* transport route to `shared_preferences` / `flutter_secure_storage`.
   - Automatically navigate to the Main App (`BottomNavigationBar` -> Dialpad).

5. **Auto-Login:** 
   - The next time the app opens, it should bypass the login screen completely.
   - It should auto-hydrate the `SipService` and connect in the background globally.
