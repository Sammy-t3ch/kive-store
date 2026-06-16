<div align="center">

<img src="public/assets/logo.png" alt="KiveStores Logo" width="120" />

# KiveStores

**Port Harcourt's #1 Gadget Hub — Online.**

A full-stack e-commerce storefront for premium new and foreign-used gadgets,  
built with React, Vercel Serverless Functions, and Vercel KV.

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/Sammy-t3ch/kivestores&project-name=kivestores&repository-name=kivestores)

[![GitHub](https://img.shields.io/badge/GitHub-Sammy--t3ch-181717?style=flat&logo=github)](https://github.com/Sammy-t3ch/kivestores)
[![Vercel](https://img.shields.io/badge/Vercel-sammietech--s--projects-000000?style=flat&logo=vercel)](https://vercel.com/sammietech-s-projects)

</div>

---

## Overview

KiveStores is the digital storefront for a gadget retail business based in Port Harcourt, Rivers State, Nigeria. Customers browse phones, laptops, game consoles, accessories, and wearables — then place orders instantly via WhatsApp. A password-protected admin dashboard (accessible via 5 rapid logo clicks) lets the store owner manage inventory in real time.

---

## Features

- **Product catalogue** — phones, laptops, games, accessories, wearables
- **Variant pricing** — storage tiers (64GB / 128GB / 256GB / 512GB / 1TB) each carry their own Naira price
- **Condition labels** — New · Foreign Used Grade A · Local Used
- **WhatsApp-first checkout** — enquiry links pre-fill the product name, storage, and price into a WhatsApp message
- **Admin dashboard** — add, edit, delete products; live image preview; reset to starter inventory
- **Secret admin access** — click the logo 5× within 2 seconds to open the admin panel (no visible link)
- **Persistent storage** — Vercel KV (Redis) keeps inventory across deployments and serverless cold starts
- **Fully responsive** — dark cyber-themed UI built with Tailwind CSS v4

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 19 + Vite 6 + TypeScript |
| Styling | Tailwind CSS v4 + custom CSS variables |
| Animation | Motion (Framer Motion) |
| Icons | Lucide React |
| API | Vercel Serverless Functions (Node.js) |
| Database | Vercel KV (Upstash Redis) |
| Hosting | Vercel |

---

## Project Structure

```
kivestores/
├── api/
│   └── products/
│       ├── index.ts      # GET /api/products · POST /api/products
│       ├── [id].ts       # GET · PUT · DELETE /api/products/:id
│       └── reset.ts      # POST /api/products/reset
├── src/
│   ├── App.tsx           # Root: routing, state, API calls
│   ├── types.ts          # Product types + STARTER_INVENTORY seed data
│   ├── main.tsx
│   ├── index.css
│   ├── components/
│   │   ├── Navbar.tsx
│   │   ├── Footer.tsx
│   │   └── WhatsAppFloating.tsx
│   └── pages/
│       ├── Home.tsx
│       ├── Shop.tsx
│       ├── ProductDetail.tsx
│       ├── About.tsx
│       ├── Contact.tsx
│       └── Admin.tsx
├── public/
│   └── assets/logo.png
├── vercel.json           # API rewrites + SPA fallback routing
├── vite.config.ts
├── package.json
└── .env.example
```

---

## API Reference

All routes are implemented as Vercel Serverless Functions under `/api/`.

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/products` | Return all products |
| `POST` | `/api/products` | Create a new product |
| `GET` | `/api/products/:id` | Get a single product |
| `PUT` | `/api/products/:id` | Update a product |
| `DELETE` | `/api/products/:id` | Delete a product |
| `POST` | `/api/products/reset` | Reset inventory to starter seed |

---

## Getting Started

### Prerequisites

- Node.js 18+
- A [Vercel account](https://vercel.com) (free tier works)
- [Vercel CLI](https://vercel.com/docs/cli): `npm i -g vercel`

### 1. Clone & install

```bash
git clone https://github.com/Sammy-t3ch/kivestores.git
cd kivestores
npm install
```

### 2. Create a Vercel KV store

1. Go to your [Vercel Dashboard](https://vercel.com/sammietech-s-projects) → **Storage** → **Create Database** → **KV**
2. Give it a name (e.g. `kivestores-kv`) and click **Create**
3. In the KV dashboard, click **Connect Project** and select this project

### 3. Pull environment variables

```bash
vercel env pull .env.local
```

This writes the `KV_URL`, `KV_REST_API_URL`, `KV_REST_API_TOKEN`, and `KV_REST_API_READ_ONLY_TOKEN` variables locally.

### 4. Run locally

```bash
vercel dev
```

> Use `vercel dev` — not `vite` — so that the serverless functions in `/api` run alongside the frontend. Plain `vite` works for UI-only changes but `/api/*` calls will 404.

Open [http://localhost:3000](http://localhost:3000)

### 5. Deploy to production

```bash
vercel --prod
```

Or connect the GitHub repo in the Vercel dashboard for automatic deployments on every push.

---

## Environment Variables

| Variable | Description |
|---|---|
| `KV_URL` | Redis connection URL (auto-injected by Vercel KV) |
| `KV_REST_API_URL` | Upstash REST API endpoint |
| `KV_REST_API_TOKEN` | Read-write API token |
| `KV_REST_API_READ_ONLY_TOKEN` | Read-only API token |

These are all created and injected automatically when you link a Vercel KV store to the project. See `.env.example` for the shape.

---

## Admin Panel

The admin dashboard is intentionally unlisted. To access it:

1. Navigate to the storefront
2. **Click the KiveStores logo 5 times within 2 seconds**
3. The admin panel opens — enter the store password to log in

From the admin panel you can:
- Add new products with variants, images, category, and condition
- Edit or delete any existing product
- Toggle stock availability
- Reset the entire inventory back to the original starter catalogue

---

## Customisation

### Changing the WhatsApp number

Search for `2349025376059` in the codebase and replace with your number (include country code, no `+` or spaces):

```
src/pages/Contact.tsx
src/components/WhatsAppFloating.tsx
src/pages/Home.tsx
```

### Updating the seed inventory

Edit `STARTER_INVENTORY` in `src/types.ts`. These products are loaded into Vercel KV on first run (when the KV key doesn't exist yet). To force a re-seed, hit **Reset Inventory** in the admin panel.

### Admin password

The password check is in `src/pages/Admin.tsx`. Search for the string comparison and replace the hardcoded value with an environment variable for production use:

```ts
// In Admin.tsx — replace with your own logic or an env var check via /api
if (password === import.meta.env.VITE_ADMIN_PASSWORD) { ... }
```

Add `VITE_ADMIN_PASSWORD` to your Vercel project environment variables.

---

## Contact

**KiveStores**  
4 Abeokuta Street, opp. Garrison Flyover, Elechi  
Port Harcourt, Rivers State, Nigeria  
📞 +234 902 537 6059  
💬 [WhatsApp](https://wa.me/2349025376059)

---

<div align="center">

Built with ❤️ in Port Harcourt.

</div>
