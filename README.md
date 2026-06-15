# CampusPulse — Smart Campus Companion

A working front-end prototype built for the **Amazon HackOn** brief: one app that brings
together a student's timetable, attendance tracking (with the 75% rule), classroom
deadline reminders, the hostel mess menu (updatable from a single photo), and a campus
FAQ chatbot.

This is a **React + Vite + Tailwind CSS** single-page app. All data is mocked / seeded
locally (see `src/data/mockData.js`) and your changes (attendance marks, submitted
assignments, mess menu edits) are saved to your browser's `localStorage`, so the app
feels alive without needing a backend.

## Features

- **Dashboard** — "Daily Strip" of today's classes; mark each one Present / Absent /
  Cancelled and watch your attendance percentage update live. Also shows attendance at a
  glance, upcoming deadlines, and today's mess menu.
- **Timetable & Attendance** — full weekly grid plus a subject-wise breakdown showing how
  many classes you can afford to skip (or must attend) to stay above 75%.
- **Classroom** — assignments, lab records and quizzes with due dates. CampusPulse flags a
  reminder when an item is due within 3h / 2h / 1h and hasn't been submitted.
- **Mess Menu** — full weekly menu table, plus an "Update from a photo" flow that
  simulates the AI step of reading a printed mess menu photo and filling in the table for
  review (no spreadsheet required).
- **Campus Bot** — a quick-FAQ chatbot answering questions about library hours, hostel
  rules, Wi-Fi, fees, exams, mess timings, and a live summary of your attendance status.

## Tech stack

- React 18 + Vite 7
- Tailwind CSS v4 (via `@tailwindcss/vite`)
- React Router
- Recharts (available for future analytics views)
- lucide-react (icons)

---

## Running on macOS (MacBook Air, M1)

### 1. Install Node.js (one-time setup)

Apple Silicon Macs run Node natively. The easiest way is via [nvm](https://github.com/nvm-sh/nvm):

```bash
# install nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# restart your terminal, then:
nvm install 20
nvm use 20
```

(Alternatively, install Node 20 LTS directly from https://nodejs.org — the "Apple
Silicon" `.pkg` installer.)

Verify:

```bash
node -v   # should print v20.x.x
npm -v
```

### 2. Unzip the project and install dependencies

```bash
cd ~/Downloads
unzip campus-pulse.zip
cd campus-pulse
npm install
```

This installs everything into a local `node_modules/` folder (a few hundred MB —
normal for a Vite project). It takes 30–60 seconds on an M1.

### 3. Start the dev server

```bash
npm run dev
```

Vite will print a local URL, typically:

```
  ➜  Local:   http://localhost:5173/
```

Open that link in your browser (Chrome, Safari, or Edge all work). The app supports
hot-reload — any code changes you make will refresh instantly.

### 4. (Optional) Build a production bundle

```bash
npm run build      # outputs static files into dist/
npm run preview    # serves the production build locally for a final check
```

The contents of `dist/` are plain static files — you can deploy them to any static host
(Vercel, Netlify, GitHub Pages, S3, etc.) for a live demo link.

---

## Project structure

```
campus-pulse/
├── src/
│   ├── components/      Sidebar, TopBar, Layout, AttendanceRing
│   ├── context/          AppContext.jsx — shared state + localStorage persistence
│   ├── data/              mockData.js — subjects, timetable, assignments, mess menu, FAQ
│   ├── pages/             Dashboard, Timetable, Classroom, MessMenu, Chatbot
│   ├── utils/             date helpers
│   ├── App.jsx            route definitions
│   ├── index.css          Tailwind + design tokens (colors, fonts)
│   └── main.jsx           app entry point
├── index.html
├── vite.config.js
└── package.json
```

## Resetting demo data

Attendance marks, submitted assignments, and mess-menu edits are stored under the
`campuspulse_state_v1` key in `localStorage`. To reset to the seeded demo data, open your
browser's dev tools console on the app and run:

```js
localStorage.removeItem('campuspulse_state_v1')
```

then refresh the page.

## Extending this prototype

- **Real timetable / attendance data**: replace `src/data/mockData.js` with calls to your
  college ERP API (e.g. fetch the weekly timetable and attendance counters per student).
- **Push notifications**: wire the reminder logic in `TopBar.jsx` to the Web
  Notifications API or a mobile push service (FCM) for the 3h/2h/1h classroom alerts.
- **Mess menu photo → table**: replace the simulated `setTimeout` in
  `pages/MessMenu.jsx` with a real call to a vision-capable AI model (e.g. the Anthropic
  API with an image input) that returns structured JSON for the week's menu.
- **Smarter chatbot**: swap the keyword-matching in `pages/Chatbot.jsx` for a call to an
  LLM, using the FAQ list as grounding context / guardrails.
