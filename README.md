# Vizchain — Crypto Market Tracker

![React](https://img.shields.io/badge/React-18.3-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-5.5-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-5.4-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-3-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-Backend-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)
![CoinMarketCap](https://img.shields.io/badge/CoinMarketCap-API-17181B?style=for-the-badge&logo=coinmarketcap&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge)

A full-stack cryptocurrency market tracking application with real-time prices, a personal portfolio, watchlists, market analytics, and crypto news — built with React, TypeScript, and Supabase.

## ✨ Features

- **Live Market Data** — real-time cryptocurrency prices, market cap, volume, and 24h change, sourced from the CoinMarketCap API and refreshed automatically every 30 seconds
- **Authentication** — email/password sign-up and sign-in via Supabase Auth, with protected routes for all authenticated pages
- **Portfolio Tracking** — add holdings with purchase price/date, and track live profit & loss per asset
- **Watchlist** — save and monitor favorite coins in one place
- **Top Movers** — surface the biggest gainers and losers across the market
- **Market Research** — search and explore any cryptocurrency by name or symbol
- **Crypto News** — latest crypto news headlines pulled from CryptoPanic
- **Analytics Dashboard** — visual charts and market insights
- **Dark/Light Theme** — theme toggle with persisted user preference
- **Row-Level Security** — user-scoped data access enforced at the database level (Supabase RLS), so portfolios and watchlists are private to each user

## 🧱 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 + TypeScript, Vite |
| Styling | Tailwind CSS + shadcn/ui (Radix UI primitives) |
| Routing | React Router v6 |
| Data Fetching | TanStack Query (React Query) |
| Charts | Recharts |
| Backend | Supabase (Postgres, Auth, Row-Level Security, Edge Functions) |
| Market Data | CoinMarketCap API |
| News Data | CryptoPanic API |

## 📊 Architecture

```
React (Vite) client
   │
   ├── Supabase Auth ──── email/password authentication
   ├── Supabase Postgres ─ profiles, portfolio, watchlist (RLS-protected)
   └── Supabase Edge Functions (Deno)
         ├── crypto-data     → CoinMarketCap /listings/latest
         ├── search-crypto   → CoinMarketCap /cryptocurrency/map
         └── crypto-news     → CryptoPanic /posts
```

API keys (e.g. `COINMARKETCAP_API_KEY`) are kept server-side as Supabase Edge Function secrets — they are never exposed to the browser.

## 🚀 Getting Started

### Prerequisites

- Node.js (LTS) and a package manager (npm, pnpm, or bun — a `bun.lockb` is included)
- A [Supabase](https://supabase.com/) project
- A [CoinMarketCap API key](https://coinmarketcap.com/api/)

### Installation

```bash
git clone https://github.com/vizartid/Vizchain-Crypto-Market-Tracker.git
cd Vizchain-Crypto-Market-Tracker

# install dependencies
npm install
# or: bun install
```

### Environment Setup

1. Create a Supabase project and note your project URL and anon key.
2. Configure `src/integrations/supabase/client.ts` (or the relevant env vars) with your Supabase credentials.
3. Run the SQL migration in `supabase/migrations/` against your Supabase project to create the `profiles`, `watchlist`, and `portfolio` tables with RLS policies.
4. Deploy the Edge Functions in `supabase/functions/` (`crypto-data`, `search-crypto`, `crypto-news`) to your Supabase project.
5. Set the `COINMARKETCAP_API_KEY` secret on your Supabase project (used only by the Edge Functions, never client-side).

### Run locally

```bash
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

### Build for production

```bash
npm run build
npm run preview
```

## 📁 Project Structure

```
├── src/
│   ├── components/
│   │   ├── AppSidebar.tsx        # Main navigation sidebar
│   │   ├── Header.tsx            # Top header bar
│   │   ├── CryptoCard.tsx        # Individual coin display card
│   │   ├── CryptoDashboard.tsx   # Dashboard market overview
│   │   ├── ProtectedRoute.tsx    # Auth-gated route wrapper
│   │   ├── ThemeProvider.tsx     # Dark/light theme context
│   │   └── ui/                   # shadcn/ui components
│   ├── contexts/
│   │   └── AuthContext.tsx       # Supabase auth state management
│   ├── hooks/
│   │   └── useCryptoData.ts      # Market data + search hooks
│   ├── integrations/supabase/    # Supabase client & generated types
│   ├── pages/
│   │   ├── Landing.tsx
│   │   ├── Auth.tsx
│   │   ├── Dashboard.tsx
│   │   ├── Portfolio.tsx
│   │   ├── Watchlist.tsx
│   │   ├── TopMovers.tsx
│   │   ├── MarketResearch.tsx
│   │   ├── News.tsx
│   │   ├── Analytics.tsx
│   │   └── Settings.tsx
│   └── App.tsx                   # Routing configuration
└── supabase/
    ├── functions/                 # Edge Functions (Deno)
    │   ├── crypto-data/
    │   ├── crypto-news/
    │   └── search-crypto/
    └── migrations/                # Database schema & RLS policies
```

## 🔐 Security Notes

- All database tables use **Row-Level Security (RLS)**, so users can only read/write their own portfolio and watchlist data.
- Third-party API keys are stored as server-side secrets and accessed only from Supabase Edge Functions — they are never bundled into the client.

## 📄 License

This project is licensed under the [MIT License](LICENSE).
