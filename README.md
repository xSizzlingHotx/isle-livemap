# 🗺️ isle-livemap

**Self-hostable Livemap & Heatmap for The Isle: Evrima servers running PrimalCore.**

Show your players their real-time position on a web map, and visualize server activity with a heatmap.

## Features

- 🗺️ **Live Map** – Real-time player position, HP, hunger, thirst, growth (auto-refresh every 5s)
- 🔥 **Heatmap** – See where players spend their time (refreshes every 5 min)
- 🔐 **Discord OAuth** – Players log in with Discord, linked to their Steam account via PrimalCore
- 🌍 **i18n** – English & German included, easy to add more
- 🎨 **Customizable** – Swap your map image, adjust colors, configure bounds

## Requirements

- Node.js 18+
- The Isle: Evrima server with **PrimalCore** mod
- PrimalCore MySQL database access
- Discord OAuth app

## Quick Start

```bash
git clone https://github.com/xSizzlingHotx/isle-livemap
cd isle-livemap
npm install
cp .env.example .env.local
# Edit .env.local with your credentials
npm run dev
```

## Configuration

Copy `.env.example` to `.env.local` and fill in:

| Variable | Description |
|----------|-------------|
| `DISCORD_CLIENT_ID` | From discord.com/developers |
| `DISCORD_CLIENT_SECRET` | From discord.com/developers |
| `NEXTAUTH_SECRET` | Any random string (32+ chars) |
| `NEXTAUTH_URL` | Your domain, e.g. `https://map.yourserver.com` |
| `PRIMALCORE_DB_HOST` | MySQL host |
| `PRIMALCORE_DB_USER` | MySQL user |
| `PRIMALCORE_DB_PASSWORD` | MySQL password |
| `PRIMALCORE_DB_NAME` | Database name (usually `primalcore`) |

## Map Setup

1. Place your server's map image at `public/map.jpg`
2. Update world bounds in `lib/mapConfig.js` to match your map

```js
export const MAP_CONFIG = {
  imageWidth: 4096,
  imageHeight: 4096,
  worldMinX: -100000,
  worldMaxX: 100000,
  worldMinZ: -100000,
  worldMaxZ: 100000,
};
```

## How It Works

```
The Isle Server (PrimalCore)
        │ writes player data
        ▼
PrimalCore MySQL DB
        │ queried via API routes
        ▼
Next.js App (this repo)
        │ renders on Leaflet.js
        ▼
Player Browser
```

PrimalCore tables used:
- `idsync` — Discord ID ↔ Steam ID link
- `playerstats` — pos_x, pos_z, hp, hunger, thirst, growth
- `server_info` — day/night, online count

## Deployment

### VPS (Recommended)
```bash
npm run build
pm2 start npm --name "isle-livemap" -- start
```

### Vercel
```bash
vercel
```
Add env variables in Vercel dashboard. Note: DB must be publicly accessible.

## License

MIT — free to use, modify and self-host. PRs welcome!

## Credits

Built for [Blood & Bones](https://bloodandbones.de) Evrima server.  
Compatible with any The Isle: Evrima server running PrimalCore.