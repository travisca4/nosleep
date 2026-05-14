# Stay Awake 🌙

A lightweight, single-file browser app that prevents your Mac from sleeping and displays your Google Calendar schedule for the day — no installation required.

Built for use on managed work Macs where installing native apps isn't permitted.

---

## Features

- **Wake Lock** — keeps your Mac awake using the Web Screen Wake Lock API, with a video fallback for unsupported browsers
- **Live Clock** — displays current time with seconds and today's date
- **Google Calendar Integration** — shows today's meetings with live "in progress" indicators, countdown timers, Google Meet links, and event duration
- **Session Tracking** — uptime counter and total hours across sessions
- **Auto re-acquire** — reclaims the wake lock if you switch tabs and come back

---

## Files

```
Stay Awake/
├── stay-awake-v2.html   # The app — this is all you need
└── README.md
```

---

## Requirements

- macOS (any version)
- Python 3 (pre-installed on macOS)
- Google Chrome or Safari 16+
- A Google Cloud project with the Calendar API enabled (for calendar features)

---

## Running the App

Because Chrome blocks external scripts on `file://` URLs, the app must be served over localhost.

**1. Start the local server**

Open Terminal and run:

```bash
cd "~/Desktop/Automations/Stay Awake" && python3 -m http.server 8080
```

Leave this Terminal window open — it keeps the server running.

**2. Open the app in Chrome**

Go to:

```
http://localhost:8080/stay-awake-v2.html
```

Bookmark this URL for quick access.

> The Terminal window can be minimized. The server uses essentially no resources while idle.

---

## Google Calendar Setup

This is a one-time setup that takes about 3 minutes.

### 1. Create a Google Cloud Project

- Go to [console.cloud.google.com](https://console.cloud.google.com)
- Create a new project (or use an existing one)

### 2. Enable the Calendar API

- Navigate to **APIs & Services → Library**
- Search for and enable **Google Calendar API**

### 3. Configure the OAuth Consent Screen

- Go to **APIs & Services → OAuth consent screen**
- Set user type to **External**
- Fill in an app name (e.g. "Stay Awake")
- Add your Google account as a **Test User**

### 4. Create OAuth Credentials

- Go to **APIs & Services → Credentials → Create Credentials → OAuth Client ID**
- Application type: **Web application**
- Under **Authorized JavaScript Origins**, add:
  ```
  http://localhost:8080
  ```
- Copy the generated **Client ID** (ends in `.apps.googleusercontent.com`)

### 5. Connect in the App

- Open the app at `http://localhost:8080/stay-awake-v2.html`
- Paste your Client ID into the setup screen and click **Connect**
- Click **Connect Calendar** and sign in with your Google account

Your Client ID is saved to `localStorage` — you only need to enter it once.

---

## Browser Compatibility

| Browser | Wake Lock | Calendar |
|---|---|---|
| Chrome | ✅ Native Wake Lock API | ✅ |
| Safari 16+ | ✅ Native Wake Lock API | ✅ |
| Firefox | ⚠️ Video fallback only | ✅ |

---

## Notes

- Calendar data is read-only — the app never modifies your calendar
- No data is sent to any server other than Google's own APIs
- The OAuth token is stored in `sessionStorage` and cleared when you close the tab
- The Client ID is stored in `localStorage` for convenience
