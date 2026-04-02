# Call History & Logging Specs

## Local Storage
Use `sqflite` to persist call logs locally on the Android device. 

## Call Log Data Model
Create a solid data model. Each call log entry MUST track the following fields:
- `id` (Primary Key)
- `phoneNumber` / `sipUri` (The remote party's number or name)
- `direction` (Enum: `incoming` or `outgoing`)
- `status` (Enum: `missed`, `completed`, `failed`)
- `timestamp` (DateTime of when the call arrived/was initiated)
- `duration` (Integer representing seconds connected. Should be `0` if missed/failed)

## Logging Triggers (Inside `SipService`)
The call state monitor must intercept SIP events to create these logs securely:
- **Completed Calls:** When a call transitions from `CONNECTED` to `ENDED`. Use the difference between answer time and end time to calculate `duration`.
- **Missed Calls:** If an incoming call transitions from `RINGING` to `ENDED` without ever being answered, log as `missed` with `duration: 0`.
- **Failed Calls:** If an outgoing call fails or receives a busy signal, log as `failed`.

## UI Requirements (History Tab)
- The Call History screen should display a list grouping calls by day (e.g., "Today", "Yesterday").
- Use visual indicators: 
  - Red text or icon for **Missed calls**.
  - Small diagonal arrows to indicate **Incoming** vs **Outgoing**.
- Tapping a history entry should instantly initiate a new outgoing call to that number.
