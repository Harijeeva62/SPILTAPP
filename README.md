# SplitApp — Split Expense PWA

A modern, mobile-first Progressive Web App for splitting expenses among friends during trips.

## Tech Stack

- **Frontend:** React.js + Tailwind CSS + Vite
- **Backend:** Node.js + Express
- **Database:** PostgreSQL (Supabase)
- **Auth:** JWT (90-day sessions) + bcrypt
- **PWA:** Service worker + installable

## Project Structure

```
splitapp/
├── server/                 # Express backend (MVC)
│   └── src/
│       ├── config/         # Supabase client, env config, DB schema
│       ├── controllers/    # Auth, Group, Expense, Settlement logic
│       ├── middleware/      # JWT auth, validation
│       ├── routes/          # API route definitions
│       └── index.js         # Entry point
├── client/                 # React frontend
│   ├── public/             # PWA manifest, service worker, icons
│   └── src/
│       ├── components/     # Layout (nav, shell)
│       ├── context/        # AuthContext (React Context)
│       ├── pages/          # Dashboard, Groups, GroupDetail, Profile
│       ├── utils/          # Axios API instance
│       ├── App.jsx         # Router + route definitions
│       └── main.jsx        # Entry point
└── README.md
```

## Setup

### 1. Supabase Database

1. Go to [supabase.com](https://supabase.com) and create a new project.
2. Open the **SQL Editor** and run the contents of `server/src/config/schema.sql`.
3. Copy your **Project URL** and **Service Role Key** from Settings → API.

### 2. Backend

```bash
cd server
cp .env.example .env
# Edit .env with your Supabase credentials and a JWT secret
npm install
npm run dev
```

The server runs on `http://localhost:5000`.

### 3. Frontend

```bash
cd client
npm install
npm run dev
```

The frontend runs on `http://localhost:3000` with API proxy to backend.

### 4. Production Build

```bash
cd client
npm run build
# Serve the dist/ folder with any static server
```

## API Endpoints

| Method | Endpoint                  | Auth | Description          |
|--------|---------------------------|------|----------------------|
| POST   | `/auth/register`          | No   | Create account       |
| POST   | `/auth/login`             | No   | Sign in              |
| GET    | `/auth/profile`           | Yes  | Get current user     |
| GET    | `/groups`                 | Yes  | List user's groups   |
| POST   | `/groups`                 | Yes  | Create group         |
| GET    | `/groups/:id`             | Yes  | Group detail + members |
| POST   | `/groups/:id/members`     | Yes  | Add member by email  |
| DELETE | `/groups/:id/members/:uid`| Yes  | Remove member        |
| POST   | `/expenses`               | Yes  | Add expense          |
| GET    | `/expenses/:groupId`      | Yes  | List group expenses  |
| GET    | `/expenses/:groupId/balances` | Yes | Group balances    |
| GET    | `/expenses/dashboard/me`  | Yes  | Dashboard summary    |
| POST   | `/settle`                 | Yes  | Record settlement    |
| PATCH  | `/settle/:id/complete`    | Yes  | Mark as paid         |
| GET    | `/settle/:groupId`        | Yes  | List settlements     |

## Features

- **Dashboard** — Balance overview, recent transactions
- **Groups** — Create/manage trip groups, add members by email
- **Expenses** — Add expenses with equal split, see who paid what
- **Settlements** — Simplified "who owes whom", settle and mark paid
- **Profile** — View account info, sign out
- **PWA** — Installable, offline data caching, fast loading
- **Mobile-first** — Bottom navigation, responsive cards

## Environment Variables

| Variable             | Description                  |
|----------------------|------------------------------|
| `PORT`               | Server port (default: 5000)  |
| `SUPABASE_URL`       | Supabase project URL         |
| `SUPABASE_SERVICE_KEY`| Supabase service role key   |
| `JWT_SECRET`         | Secret for signing JWTs      |
| `JWT_EXPIRES_IN`     | Token expiry (default: 90d)  |

## PWA Icons

Replace `client/public/icon-192.png` and `client/public/icon-512.png` with your own PNG icons for proper PWA install support.
