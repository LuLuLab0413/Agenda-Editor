# 📅 Agenda Editor

A live, multi-event agenda editor built for delegation and programme teams. Create events, schedule daily activities, add organisation introductions, and share a live agenda with your team — all from a single HTML file powered by Firebase.

---

## ✨ Features

- **Multi-event home screen** — create and manage multiple events from one place
- **Live sync** — all changes sync in real-time to every viewer via Firebase Realtime Database
- **Desktop calendar views** — week overview and day-by-day timeline
- **Mobile daily briefing** — auto-detects mobile, shows a clean card-based daily agenda
- **Org introductions** — add a short description of each organisation being visited, shown prominently on mobile
- **Event types** — Networking, Company Visit, Meeting, Workshop, Mentor Meeting, Public Event, Cultural, Logistics, Commute, Meal
- **Participants & mentors** — assign startups and mentors to each event, with logo support
- **Password-protected editing** — viewers see a read-only live view; editors unlock with a password
- **Per-event auth** — each event has its own password, remembered per browser
- **Swipe navigation** — swipe left/right on mobile to change days
- **Overlap handling** — simultaneous events tile side-by-side in the calendar

---

## 🖥 Screenshots

| Home Screen | Week Overview | Mobile Briefing |
|---|---|---|
| Create and manage events | Full 10-day calendar grid | Card-based daily view |

---

## 🚀 Quick Start

### Option A — Just use it (no setup needed)
1. Download `agenda-editor.html`
2. Open it in any browser
3. Create your first event
4. Share the file or deploy to Netlify

### Option B — Deploy to Netlify (recommended for sharing)
1. Go to [netlify.com/drop](https://netlify.com/drop)
2. Drag and drop `agenda-editor.html`
3. Netlify gives you a public URL instantly
4. Share the URL with your team

### Option C — Connect GitHub to Netlify (auto-deploy on update)
1. Fork or clone this repo
2. Go to [netlify.com](https://netlify.com) → **Add new site → Import from Git**
3. Select this repository
4. Set **Publish directory** to `/`
5. Click **Deploy** — every push to GitHub auto-redeploys

---

## 📖 How to Use

### Creating an event
1. Open the app — you land on the **Home Screen**
2. Click **Create New Event**
3. Fill in:
   - **Event name** (e.g. *China Market Discovery 2026*)
   - **Start and end dates**
   - **First/last day tags** (optional, e.g. *Arrival* / *Departure*)
   - **Editor password** — viewers browse freely, editors need this to make changes
   - **Logo** — upload an image or paste a URL (shown on the home screen and in the header)
4. Click **Create Event** — you're taken straight into the agenda

### Adding events to the agenda
1. Click **🔐 Switch to Edit** and enter your password
2. Click **＋ Event** (top right)
3. Fill in the event details:
   - Title, start/end time, type, location
   - Select which startups/participants are attending
   - Add an **Org Introduction** — this appears as a gold-highlighted block in the mobile briefing view, helping the delegation understand who they're visiting
   - Add notes for logistics or context
4. Click **Save** — syncs live to all viewers instantly

### Adding participants
1. In edit mode, click **＋ Participant**
2. Enter name, select type (Startup or Mentor), optionally upload a logo
3. Participants can then be assigned to events

### Sharing with your team
- **Editors** — share the URL and give them the password
- **Viewers** — share the URL, they see live updates automatically with no password needed
- **Mobile users** — the app auto-detects phone screens and switches to the mobile briefing view

---

## 🔧 Firebase Setup

This app uses **Firebase Realtime Database** for live sync. The Firebase config is already included and points to the original project database.

**To use your own Firebase database:**
1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Create a new project → Add a Realtime Database
3. Set rules to allow read/write (for now):
   ```json
   {
     "rules": {
       ".read": true,
       ".write": true
     }
   }
   ```
4. Open `agenda-editor.html` in a text editor
5. Find the `firebaseConfig` block (~line 600) and replace with your own config:
   ```js
   const firebaseConfig = {
     apiKey: "YOUR_API_KEY",
     authDomain: "YOUR_PROJECT.firebaseapp.com",
     databaseURL: "https://YOUR_PROJECT-default-rtdb.firebaseio.com",
     projectId: "YOUR_PROJECT",
     storageBucket: "YOUR_PROJECT.appspot.com",
     messagingSenderId: "YOUR_SENDER_ID",
     appId: "YOUR_APP_ID"
   };
   ```

---

## 📁 Data Structure

All data is stored in Firebase under:

```
event_index/
  {event-id}/
    name
    startDate
    endDate
    logo

events/
  {event-id}/
    config/
      name
      startDate
      endDate
      password
      firstTag
      lastTag
      logo
    events/
      {date}/         ← e.g. "2026-04-20"
        [ array of event objects ]
    participants/
      [ array of participant objects ]
```

---

## 🗂 Event Object Structure

```json
{
  "id": "e1234567890",
  "title": "Visit to Li Auto",
  "start": "09:00",
  "end": "11:00",
  "type": "company_eco",
  "location": "Beijing",
  "orgIntro": "Li Auto is a Chinese EV manufacturer...",
  "notes": "Bring business cards",
  "participants": ["p_fibrecoat", "p_kraftvolt"],
  "mentors": []
}
```

---

## 🎨 Event Types

| Type | Colour | Icon |
|------|--------|------|
| Networking / Pitch | Red | 🤝 |
| Company / Eco Visit | Green | 🏢 |
| Business Meeting | Purple | 💼 |
| Workshop | Navy | 🛠 |
| Mentor Meeting | Violet | 🎓 |
| Public Event | Amber | 🌐 |
| Cultural | Mauve | 🏯 |
| Logistics | Grey | ✈️ |
| Commute | Brown | 🚌 |
| Meal / Dining | Teal | 🍜 |

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML, CSS, JavaScript (single file) |
| Database | Firebase Realtime Database |
| Fonts | Google Fonts (Playfair Display, DM Sans, DM Mono) |
| Hosting | Netlify (recommended) |
| Auth | Password stored in Firebase config, persisted in localStorage per event |

No build step. No npm. No framework. Just one HTML file.

---

## 📱 Mobile Support

On screens ≤ 768px wide, the app automatically switches to **Mobile Briefing mode**:
- Full-screen dark UI optimised for phones
- Horizontal day strip for quick navigation
- Swipe left/right to change days
- Card-based layout with org introductions prominently displayed
- **🖥 Desktop** button to switch to the full editor
- **🔐 Edit** button to unlock editor mode (requires password)

---

## 🔒 Security Notes

- The editor password is stored in plain text in Firebase. This is intentional — it's designed for trusted team use, not public-facing security.
- Firebase rules are currently open (`read: true, write: true`). For production, consider restricting write access.
- Auth state is stored in `localStorage` keyed by event ID, so each browser remembers its own access level per event.

---

## 📄 License

MIT — free to use, modify, and deploy.

---

## 🙏 Built with

Built iteratively with [Claude](https://claude.ai) by Anthropic.
