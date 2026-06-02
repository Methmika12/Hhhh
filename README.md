# ZENX - Crypto Testnet Rewards Platform

A modern, production-ready crypto testnet rewards platform built with Next.js 14, Supabase, and Web3.

## Features

- **Authentication**: Email-based signup/login with Supabase Auth
- **Wallet Integration**: MetaMask connection with address storage
- **Task System**: Complete social tasks to earn ZENX points
- **Referral Program**: Earn 100 ZENX per successful referral
- **Leaderboard**: Global ranking system with top 100 users
- **Admin Panel**: User management, task management, balance adjustments
- **Dark Theme**: Professional Web3 design with purple neon accents
- **Responsive**: Mobile and desktop optimized

## Tech Stack

- **Frontend**: Next.js 14 (App Router), React, TypeScript, Tailwind CSS
- **Backend**: Next.js API Routes, Server Actions
- **Database**: Supabase (PostgreSQL)
- **Auth**: Supabase Auth with Row Level Security (RLS)
- **Web3**: Ethers.js for MetaMask integration
- **State**: Zustand for global state management
- **UI**: Framer Motion animations, Lucide icons

## Prerequisites

- Node.js 18+
- Supabase account
- MetaMask browser extension (for testing)

## Setup

### 1. Clone and Install

```bash
git clone <repository-url>
cd zenx-platform
npm install
```

### 2. Environment Variables

Create `.env.local` file:

```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_APP_NAME=ZENX
ADMIN_SECRET_KEY=your-admin-secret-key
```

### 3. Supabase Setup

1. Create a new Supabase project
2. Run the SQL migrations in `supabase/migrations/`
3. Enable Email auth in Authentication settings
4. Set up Row Level Security policies (included in migrations)

### 4. Database Functions

Run these SQL functions in Supabase SQL Editor:

```sql
-- Function to increment balance
CREATE OR REPLACE FUNCTION increment_balance(user_id uuid, amount integer)
RETURNS void AS $$
BEGIN
  UPDATE profiles SET balance = balance + amount WHERE id = user_id;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;

-- Function to increment referral count
CREATE OR REPLACE FUNCTION increment_referral_count(user_id uuid)
RETURNS void AS $$
BEGIN
  UPDATE profiles SET total_referrals = total_referrals + 1 WHERE id = user_id;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;

-- Function to increment completed tasks
CREATE OR REPLACE FUNCTION increment_completed_tasks(user_id uuid)
RETURNS void AS $$
BEGIN
  UPDATE profiles SET completed_tasks_count = completed_tasks_count + 1 WHERE id = user_id;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;
```

### 5. Run Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

## Deployment

### Vercel Deployment

1. Push code to GitHub
2. Import project in Vercel
3. Add environment variables
4. Deploy

### Supabase Production Setup

1. Link project: `npx supabase link`
2. Push migrations: `npx supabase db push`
3. Configure auth providers

## Project Structure

```
zenx-platform/
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── (auth)/            # Auth routes (login, register)
│   │   ├── (dashboard)/       # Dashboard routes
│   │   │   ├── dashboard/     # Main dashboard
│   │   │   ├── tasks/         # Tasks page
│   │   │   ├── referral/      # Referral page
│   │   │   ├── leaderboard/   # Leaderboard page
│   │   │   ├── profile/       # Profile page
│   │   │   └── admin/         # Admin panel
│   │   ├── api/               # API routes
│   │   ├── layout.tsx         # Root layout
│   │   ├── page.tsx           # Landing page
│   │   └── globals.css        # Global styles
│   ├── components/
│   │   ├── ui/               # Reusable UI components
│   │   └── layout/           # Layout components
│   ├── hooks/                 # Custom React hooks
│   ├── lib/                   # Utilities & Supabase clients
│   ├── types/                 # TypeScript types
│   └── middleware.ts          # Auth middleware
├── supabase/
│   └── migrations/            # Database migrations
├── public/                    # Static assets
└── package.json
```

## API Routes

| Route | Method | Description |
|-------|--------|-------------|
| `/api/tasks/complete` | POST | Complete a task |
| `/api/wallet/connect` | POST | Connect wallet |
| `/api/referral/stats` | GET | Get referral stats |
| `/api/leaderboard` | GET | Get leaderboard |
| `/api/admin/users` | GET/PATCH | Admin user management |
| `/api/admin/tasks` | GET/POST/PATCH/DELETE | Admin task management |

## Security Features

- Row Level Security (RLS) on all tables
- Session-based authentication
- Duplicate task claim prevention
- Wallet address validation
- Admin route protection
- Input validation on all API routes

## License

MIT License
