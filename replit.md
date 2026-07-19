# VELTIX — Tap-to-Earn Web Game

## Overview

VELTIX is a standalone, publicly accessible tap-to-earn browser game with a neon futuristic aesthetic. Anyone with the website link can create an account and play — no Telegram or any third-party messenger required.

## Architecture

Pure static HTML site with no build step. Served by Python's built-in HTTP server on port 5000.

```
login.html      — Landing page (Logo + LOGIN / CREATE ACCOUNT buttons)
signin.html     — Email/nickname + password login form
register.html   — Full 5-field registration form
dashboard.html  — Main game page (tap coin, energy system, stats, nav)
index.html      — Root redirect → /login.html
logoveltix.png  — VELTIX coin logo asset
```

### Auth flow

```
/index.html  →  /login.html
                    ├─ /signin.html  →  (success) /dashboard.html
                    └─ /register.html → (success) /signin.html
/dashboard.html — guarded; unauthenticated users are sent to /login.html
```

## Stack

- **Frontend**: Vanilla HTML + CSS + JavaScript (ES modules)
- **Auth**: Firebase Authentication (email + password, email verification required)
- **Database**: Firebase Firestore — `players/{uid}` stores `{ coin, level, energy }`
- **Fonts**: Orbitron + Rajdhani (Google Fonts)
- **Server**: `python3 -m http.server 5000`

## Firebase project

Project ID: `veltix-game`  
Auth domain: `veltix-game.firebaseapp.com`

> **Note**: Firestore may return `"unavailable"` errors if the database is not enabled or security rules are blocking reads. The game gracefully falls back to defaults (`coin=0, level=1, energy=500`).

## Game mechanics

| Mechanic | Value |
|---|---|
| Energy max | 500 |
| Energy regen | +1 per 3 seconds |
| Tap cost | 1 energy per tap |
| Tap power | `floor((level - 1) / 10) + 1` — scales every 10 levels |
| Firestore save debounce | 1500 ms (batches rapid taps) |
| Tap debounce | 80 ms (prevents touch + click double-fire) |

## Unbuilt sections

Bottom-nav tabs Earn, Shop, and Profile show a "COMING SOON" toast and are not yet implemented.

## User Preferences

- Port must be 5000 (Replit webview requirement)
- Never add Telegram dependencies — the game is strictly web-only
- Firebase auth logic must not be removed or altered
- Keep the VELTIX neon futuristic design (Orbitron + Rajdhani, cyan/gold palette, dark background)
- Redirect successful logins to `/dashboard.html`, not `/index.html`
