# Mood Journal

A journaling app where you write your thoughts or mood and receive a response from Claude AI. Backend (Node.js, Express, Prisma, PostgreSQL) runs in Docker; frontend (Vite, Vanilla JS) runs on the host.

## Prerequisites

- **Docker** and **Docker Compose** – [Install Docker](https://docs.docker.com/get-docker/)
- **Node.js** and **npm** – only needed to run the frontend dev server

No need to install PostgreSQL or run the backend manually; Docker runs the database and API.

## One-time setup

1. Copy the example env file and set your Anthropic API key:

   ```bash
   cp backend/.env.example backend/.env
   ```

   Edit `backend/.env` and set `ANTHROPIC_API_KEY` (get a key from [Anthropic Console](https://console.anthropic.com/)). Leave `DATABASE_URL` and `PORT` as in the example when using Docker.

## Run the app

1. **Start the backend and database** (from repo root):

   ```bash
   docker compose up --build
   ```

   On first run or with a fresh database, apply migrations in another terminal:

   ```bash
   docker compose exec backend npx prisma migrate deploy
   ```

2. **Start the frontend** (new terminal, from repo root):

   ```bash
   cd frontend && npm install && npm run dev
   ```

   Open the URL Vite prints (e.g. http://localhost:5173). The app proxies `/api` to the backend at http://localhost:3000.

## Verify

- Backend health: `curl http://localhost:3000/health` → `{"status":"ok"}`
- Entries: `curl http://localhost:3000/entries` → JSON array of entries

## Team onboarding

A team member can clone the repo and have the app running without installing Node or Postgres on the host (only Docker and npm for the frontend).

1. `git clone https://github.com/ViperClaw1/mood-journal.git && cd mood-journal`
2. `cp backend/.env.example backend/.env` and set `ANTHROPIC_API_KEY`
3. `docker compose up --build`. Wait until backend and postgres are up.
4. (Optional) `docker compose exec backend npx prisma migrate deploy`
5. `cd frontend && npm install && npm run dev` and open the dev URL
6. Smoke test: submit an entry and confirm history and AI response

## Stakeholder preview with ngrok

To give clients or stakeholders a temporary public URL to try the app:

1. Install [ngrok](https://ngrok.com/) (e.g. download or `choco install ngrok` / `brew install ngrok`).
2. Start the app: `docker compose up -d` and `cd frontend && npm run dev` (Vite on port 5173).
3. In another terminal: `ngrok http 5173`
4. Share the HTTPS URL ngrok prints (e.g. `https://abc123.ngrok.io`). Stakeholders open it in a browser; Vite proxies API calls to the backend.
5. When done, stop ngrok and stop the frontend and Docker stack (`docker compose down`).

**Caveats:** Your machine must stay on and connected. Free ngrok URLs are temporary and change each run. Do not leave ngrok running unattended with sensitive data.

## Publishing this repo to GitHub

1. Create a **private** repository on GitHub (e.g. [ViperClaw1/mood-journal](https://github.com/ViperClaw1)); do not add a README or .gitignore when creating it.
2. From the repo root:

   ```bash
   git init
   git add .
   git commit -m "Initial commit: Mood Journal app with Docker backend"
   git remote add origin https://github.com/ViperClaw1/mood-journal.git
   git branch -M main
   git push -u origin main
   ```

   Replace `ViperClaw1/mood-journal` with your actual repo URL if different.
# mood-journal
