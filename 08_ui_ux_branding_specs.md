# UI/UX & Branding Specifications

The app must strictly adhere to the "HOHIT Contact Center" branding principles to match the overarching admin dashboard. 

## Color Palette (`ThemeData`)
The core aesthetic is a high-visibility **Dark and Gold/Yellow** theme. The AI must define these specific colors in the Flutter theme context:
- **Background (Main):** Deep dark blue/almost black (approx `#0B0E14` or `#0F131F`). Used for the main app background layout.
- **Primary Accent (Gold/Yellow):** Bright Yellow/Gold (approx `#FFC107` or `#FFB800`). Used for primary buttons, highlighted text, and the branding banner.
- **Input Fields:** Dark gray/blue (approx `#1A2235`) with rounded corners and subtle lighter borders.
- **Text:** White/Light Gray for body text, Yellow for headers and active states, Black when placed on top of the Yellow background.

## Login Screen (First-Time Onboarding) Layout
Since mobile screens are vertical, the desktop side-by-side design should be adapted into a Top/Bottom arrangement:

**Top Section (The "Gold" Panel):**
- **Background:** Solid Gold/Yellow. Allow it to curve at the bottom if desired.
- **Hero Image:** A white circle containing the HOHIT Headset / Building logo.
- **Title:** "Contact Center" centrally aligned in black under the logo.
- **Grid Tiles:** Directly below the title, place a 2x2 grid of small, rounded rectangles (tinted slightly darker yellow/brown) containing these specific texts:
  - `24/7` (top) `Support` (bottom)
  - `99.9%` (top) `Uptime` (bottom)
  - `Real-time` (top) `Analytics` (bottom)
  - `Smart` (top) `Routing` (bottom)

**Bottom Section (The "Dark" Panel):**
- **Background:** Dark Blue/Black.
- **Title:** "Welcome Back" (in Gold) followed by a subtitle `Sign in to your dashboard` (in light gray).
- **Input Fields:** 
  - Follow the exact styling of the HOHIT admin: Dark background, rounded borders. 
  - Fields needed for VoIP: **Hostname/Server IP**, **Extension**, and **Password**.
- **Submit Button:** A massive, full-width Gold/Yellow button labeled `Login / Access Dashboard` with Black text.
- **Footer:** Add tiny text at the very bottom: `Secure • Reliable • Professional`.

## App-Wide Theming Rules
The HOHIT theme must extend beyond the login page:
- The `Dialpad` should feature a dark background with Yellow text for the numbers, and a massive Yellow circular "CALL" button.
- The `BottomNavigationBar` must be dark, with icons glowing Yellow when active.
- The `ActiveCallScreen` should maintain the dark aesthetic with Yellow action buttons (Mute, Speaker, Hold).
