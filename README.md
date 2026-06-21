# BuddyDoro

A Pomodoro productivity web app with a virtual companion. Earn rewards, unlock companions, and let AI build your study plan — all while your buddy cheers you on.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML, CSS, ES Modules (no framework) |
| Backend | Node.js + Express.js |
| Database | MongoDB (via Mongoose) |
| Auth | JWT (jsonwebtoken + bcrypt) |
| Payments | Stripe |
| PDF Parsing | pdf.js (degree pathway feature) |
| Music | Spotify Web Playback SDK + built-in catalog |
| Animations | anime.js |
| Fonts | Nunito, Plus Jakarta Sans |

---

## Pages

| File | Purpose |
|---|---|
| `landing.html` | Public marketing page |
| `login.html` | User login |
| `signup.html` | User registration |
| `onboarding.html` | New-user onboarding tour |
| `index.html` | Main app dashboard |
| `store.html` | Companion store |
| `pathway.html` | Degree pathway planner |
| `resources.html` | Resources & notes |
| `aiplan.html` | AI study plan generator |
| `history.html` | Session history viewer |
| `account.html` | Account settings |
| `profile.html` | Profile settings |

---

## Features

### Pomodoro Timer
- Focus, Short Break, and Long Break modes
- Configurable durations (focus 1–120 min, breaks 1–60 min)
- Animated circular ring countdown
- Float mode — detach the timer panel and drag it anywhere on screen
- Settings saved to backend per user

### Task & Goal Management
- Create multiple goals, each with its own task list
- Add, rename, and delete goals
- Add subtasks within each task
- Check off individual tasks during a session
- Todo quick-access dialog for at-a-glance task tracking

### Study Sessions
- Start a session tied to a specific goal
- Collapsible session panel tracks tasks in real time
- Task-complete prompt appears mid-session to pick next task
- End session at any time; summary logged to history

### Virtual Companion
- Companion (Dragon by default) lives in the scene and reacts to your activity
- Health, happiness, and thirst status bars
- Companion displays thought bubbles based on current state
- Companion can die if neglected — revival requires a potion or choosing a new companion
- Available companions: Dragon, Axolotl, Frog, Capybara, and more

### AI Study Plan Generator
- Describe any goal in plain text — AI breaks it into actionable tasks and subtasks
- Edit generated tasks before saving them to your goal list
- Accessible from the sidebar or directly at `aiplan.html`

### Degree Pathway Planner
- Upload a PDF or paste text of your degree plan / curriculum sheet
- AI parses the content and auto-generates study goals from your courses
- Available at `pathway.html`

### Doro Currency
- Earn Doros for every completed focus session
- Spend Doros in the Companion Store

### Diamond Currency
- Premium currency purchasable via Stripe (test card: `4242 4242 4242 4242`)
- Used for premium items like the Revival Potion

### Companion Store
- Categories: Food, Play, Water, Medicine, Skins, Backgrounds
- Purchased items go to your Inventory
- Apply skins to change your companion's appearance
- Apply backgrounds to change the scene

### Inventory
- View owned items organized by category
- Use items directly from the inventory panel

### Music Player
- Built-in track catalog with ambient and focus music
- Spotify integration (switch source in music settings)
- Controls: play/pause, next, previous, progress bar with timestamps
- Settings panel to toggle source and choose playlist

### History
- View completed session stats across five time ranges: Daily, Weekly, Monthly, Yearly, Lifetime

### Resources & Notes
- Three tabs: Discover (curated links), Saved (bookmarked resources), Notes (personal notes)

### Day / Night System
- Background automatically switches between day and night scenes based on local time (night = 8 PM – 7 AM)
- Moon overlay appears at night

### Rain Effects
- Random rain showers triggered ~10 times per day, each lasting 5 minutes
- Configurable density, speed, and wind

### User Accounts
- Signup / login with hashed passwords (bcrypt) and JWT session tokens
- Account settings page (email, password)
- Profile settings page (display name, avatar preferences)

---

## Layout — Main App (`index.html`)

### Background Scene
- Full-screen scene image (day or night variant, auto-selected)
- Companion sprite centered in the scene
- Rain overlay and moon overlay rendered on top of the scene
- "Companion has died" button appears when companion health reaches zero

### Topbar (fixed, full-width)
- **Left group**
  - Hamburger button — opens sidebar
  - Store icon — links to `store.html`
  - Inventory icon — opens inventory dialog
  - Pathway icon — links to `pathway.html`
  - Resources icon — links to `resources.html`
- **Center group**
  - Timer button — slides open the timer panel on the right
  - Music button — opens the music mini panel below it
- **Right group**
  - Doros currency pill (balance display)
  - Diamonds currency pill (balance + "Buy Diamonds" dropdown)

### Tasks Area (left, main content)
- One or more goal panels stacked vertically
- **Each goal panel contains:**
  - Goal title with inline rename and delete buttons
  - List of tasks (checkable)
  - "Create a Task" button at the bottom

### Timer Panel (right, slides in)
- Header with title and close/expand button
- Settings row: Settings gear button + Float button
- **Settings panel (toggled):**
  - Input fields for Focus, Short Break, and Long Break durations
  - Apply button
- Mode chips: Focus / Short / Long
- Circular ring timer with large time display
- Button row: Reset | Start/Pause | Skip

### Sidebar (slides in from left)
- **Profile header:**
  - App title ("BuddyDoro")
  - Account settings icon link
  - Profile icon link
  - Logout button
  - Close button
  - Greeting and username
- **Plan section:**
  - AI Plan button — opens `aiplan.html`
- **Goals section:**
  - "Goals" label + "Add goal" button
  - All goal panels rendered inline (same panels as the main tasks area)
  - "Start Session" button at the bottom
- **App section:**
  - History button — opens history dialog
  - Music button — opens music mini panel

### Music Mini Panel (dropdown from topbar center)
- Settings gear button (opens source/playlist settings)
- Animated boombox illustration
- Now-playing title and subtitle
- Progress bar with elapsed / total time
- Controls: Previous | Play/Pause | Next

### Session Panel (fixed left, active only during a session)
- Session label and collapse button
- Goal title for the active session
- Task list with checkboxes
- "End Session" button

### Session Next Overlay (full-screen, mid-session)
- Icon, title, and body message (task complete or goal complete)
- Task picker to select next task
- "End Session" button

### Dialogs (modal overlays)
- **Inventory** — category tabs + item grid, close button
- **History** — period tabs (Daily / Weekly / Monthly / Yearly / Lifetime) + stat content
- **Diamond Store** — diamond pack list, current balance display, Stripe checkout
- **Dead Companion** — tabs for Potions (buy Revival Potion) and New Companion (choose a free replacement)
- **Todo** — quick task list with "Add Goal" and Cancel

---

## Project Structure

```
buddydoro/
├── server/                   # Express.js backend (legacy entry)
│   ├── models/               # Mongoose models (User, Task, etc.)
│   ├── routes/               # API route handlers
│   ├── authMiddleware.js
│   └── server.js
├── apps/
│   ├── api/                  # TypeScript API server (apps/api/src/index.ts)
│   └── web/
│       ├── public/           # HTML pages (served at root)
│       ├── js/
│       │   ├── main.js       # App entry point
│       │   ├── api/          # API client & service modules
│       │   ├── features/     # Feature modules (timer, tasks, music, companion, etc.)
│       │   ├── pages/        # Page-specific scripts
│       │   └── utils/        # Shared utilities (notifications, preferences)
│       └── styles/           # CSS files (one per feature/component)
├── .env.example
├── package.json
├── serve.mjs                 # Static dev server (localhost:9090)
└── screenshot.mjs            # Puppeteer screenshot helper
```

---

## Prerequisites

- **Node.js** v16 or higher
- **MongoDB Community Edition**

---

## Setup

### 1. Install MongoDB

**Windows:**
1. Download MongoDB Community Edition from https://www.mongodb.com/try/download/community
2. Run the installer with default settings
3. Select "Install MongoDB as a Service" during setup
4. Verify: `mongod --version` in PowerShell

**Mac (Homebrew):**
```bash
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb-community
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt-get install -y mongodb-org
sudo systemctl start mongod
sudo systemctl enable mongod
```

### 2. Configure Environment

Copy the example env file and fill in your values:

```bash
cd buddydoro
copy .env.example .env   # Windows
# or
cp .env.example .env     # Mac/Linux
```

Required values in `buddydoro/.env`:

```
MONGO_URI=mongodb://localhost:27017/buddydoro
JWT_SECRET=your_secret_key_change_this_in_production
STRIPE_SECRET_KEY=sk_test_...
STRIPE_PUBLISHABLE_KEY=pk_test_...
```

### 3. Install Dependencies

```bash
cd buddydoro
npm install
```

### 4. Start the Backend

```bash
node server/server.js
```

The API listens on `http://localhost:3000`. You should see:
```
Server listening on port 3000
MongoDB connected
```

### 5. Start the Frontend

From the repo root:

```bash
node serve.mjs
```

Open: `http://localhost:9090/public/login.html`

---

## API Endpoints

### Auth
| Method | Route | Description |
|---|---|---|
| POST | `/api/auth/signup` | Create a new user |
| POST | `/api/auth/login` | Login and receive JWT |

### Tasks *(requires auth)*
| Method | Route | Description |
|---|---|---|
| GET | `/api/tasks` | Get all tasks for the authenticated user |
| POST | `/api/tasks` | Create a new task |
| PUT | `/api/tasks/:id` | Update a task |
| DELETE | `/api/tasks/:id` | Delete a task |

### User Settings *(requires auth)*
| Method | Route | Description |
|---|---|---|
| PATCH | `/api/user/settings` | Save timer durations and preferences |

### Payments
| Method | Route | Description |
|---|---|---|
| POST | `/api/stripe/create-checkout-session` | Create a Stripe checkout session for diamond packs |

---

## Payments — Test Mode

Use Stripe test card `4242 4242 4242 4242` with any future expiry date, any 3-digit CVC, and any valid ZIP code.

Each teammate needs their own Stripe test account. Add your own `STRIPE_SECRET_KEY` and `STRIPE_PUBLISHABLE_KEY` from the Stripe dashboard (Test mode).

---

## Troubleshooting

**MongoDB connection refused**
- Windows: open Services and verify the "MongoDB" service is running
- Mac/Linux: `brew services list` or `sudo systemctl status mongod`

**Port 3000 already in use**
- Change the port in `server/server.js`, or kill the process using that port

**JWT errors / logged out unexpectedly**
- Clear `localStorage` in the browser dev tools
- Confirm `buddydoro/.env` contains a valid `JWT_SECRET`

**Stripe payment not completing**
- Confirm you are using a test-mode key (starts with `sk_test_`)
- Use the Stripe test card `4242 4242 4242 4242`
