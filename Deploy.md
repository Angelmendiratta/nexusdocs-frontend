# Deploying the NexusDocs frontend

This is a plain HTML/CSS/JS app — no build step, no framework, no npm install needed.
It talks directly to your Solarch backend's REST API via `fetch()`.

## 1. Point it at your backend

Edit `config.js`:
```js
window.__NEXUSDOCS_API_BASE__ = "https://your-backend-url.up.railway.app";
```

## 2. Test locally first

Just open `index.html` in a browser (or run a tiny local server: `npx serve .`), with your
Solarch backend running locally on port 8090 (config.js defaults to `localhost:8090`).

Try signing up a new user, creating a workspace, adding a document, and commenting — confirm
the whole flow works before deploying.

## 3. Deploy to Vercel

1. Push this folder to a GitHub repo (can be a `frontend/` folder inside your existing
   `nexusdocs` repo, or its own separate repo — either works).
2. Go to vercel.com → "New Project" → import the repo.
3. Framework preset: choose "Other" (it's a static site, no build command needed).
4. Deploy.

Vercel will give you a public URL like `https://nexusdocs.vercel.app`.

## 4. CORS — important

Your Solarch backend needs to allow requests from your Vercel domain, or the browser will
block every API call. See the backend README for the CORS settings update needed
(`ALLOWED_ORIGINS` env var or `/api/settings` CORS config, depending on what Solarch exposes —
check `dist/apis/middlewares_cors.js` in the backend source to confirm the exact mechanism).

## What this frontend does

- Sign up / log in (talks to `/api/collections/users/*`)
- List and create workspaces
- List and create documents within a workspace
- View a document and its comments, add new comments
- Logs activity to `activity_log` at the application layer (mirrors the backend's own
  workaround for the broken `pb_hooks` feature — see backend README)

## What it doesn't do (by design, for time)

- No realtime/WebSocket wiring (backend supports it — see backend README — but the frontend
  currently just re-fetches after each action rather than subscribing live)
- No file upload UI (backend has the file field ready on `documents`)
- No vector/semantic search UI (backend has the field ready)