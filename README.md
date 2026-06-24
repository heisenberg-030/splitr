# Splitr — Settle Smarter 💸

Splitr is a full-stack AI-powered expense splitting web app. Add shared expenses, split them equally or by custom amounts, track who owes who, and settle up — all in real time. It also sends automated payment reminders by email and generates monthly AI spending insights.

---

## What it does

- **Create groups** — Organise friends, roommates, or trip buddies into groups
- **Add expenses** — Log shared bills with a description, amount, category, and date
- **Split 3 ways** — Equal split, percentage split, or exact amount split
- **Real-time balances** — Dashboard updates instantly as expenses are added (powered by Convex)
- **Settle up** — Record payments between members to clear balances
- **Payment reminders** — Automated daily emails sent to users with outstanding debts
- **AI spending insights** — Monthly email with a Gemini AI analysis of your spending patterns
- **Authentication** — Secure sign-up and login with Clerk

---

## Tech stack

| Layer | Technology |
|---|---|
| Framework | Next.js 15 (App Router) |
| Database | Convex (real-time) |
| Auth | Clerk |
| AI | Google Gemini 1.5 Flash |
| Background jobs | Inngest |
| Email | Resend |
| UI components | Shadcn UI + Radix UI |
| Styling | Tailwind CSS v4 |
| Forms | React Hook Form + Zod |
| Charts | Recharts |

---

## Getting started

### 1. Clone the repo and install dependencies

```bash
git clone https://github.com/your-username/splitr.git
cd splitr
npm install --legacy-peer-deps
```

> The `--legacy-peer-deps` flag is needed because `react-day-picker` has a peer dependency conflict with `date-fns v4`.

### 2. Set up external services

You will need free accounts on the following:

| Service | Purpose | Link |
|---|---|---|
| Convex | Real-time database | [convex.dev](https://convex.dev) |
| Clerk | Authentication | [clerk.com](https://clerk.com) |
| Resend | Email sending | [resend.com](https://resend.com) |
| Google AI Studio | Gemini API key | [aistudio.google.com](https://aistudio.google.com) |

### 3. Create your `.env.local` file

Create a file called `.env.local` in the root of the project and fill in the following:

```env
# Convex — auto-filled when you run `npx convex dev`
CONVEX_DEPLOYMENT=
NEXT_PUBLIC_CONVEX_URL=

# Clerk
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=
NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in
NEXT_PUBLIC_CLERK_SIGN_UP_URL=/sign-up
CLERK_JWT_ISSUER_DOMAIN=

# Resend
RESEND_API_KEY=

# Google Gemini
GEMINI_API_KEY=

# Inngest (use these values for local development)
INNGEST_EVENT_KEY=local
INNGEST_SIGNING_KEY=local
```

### 4. Run the app

You need **three terminals** running at the same time:

**Terminal 1 — Convex database**
```bash
npx convex dev
```
This will auto-populate `CONVEX_DEPLOYMENT` and `NEXT_PUBLIC_CONVEX_URL` in your `.env.local`.

**Terminal 2 — Next.js app**
```bash
npm run dev
```

**Terminal 3 — Inngest background jobs**
```bash
npx inngest-cli@latest dev
```

Open [http://localhost:3000](http://localhost:3000) to see the app.
Open [http://localhost:8288](http://localhost:8288) to see the Inngest dev dashboard.

---

## Project structure

```
splitr/
├── app/
│   ├── (auth)/          # Sign-in and sign-up pages (Clerk)
│   ├── (main)/          # Authenticated app pages
│   │   ├── dashboard/   # Balance overview, charts, groups
│   │   ├── contacts/    # Friends list and group creation
│   │   ├── expenses/    # Add new expense form
│   │   ├── groups/      # Group detail and balances
│   │   ├── person/      # One-on-one expense history
│   │   └── settlements/ # Record a payment/settlement
│   ├── api/inngest/     # Inngest background job endpoint
│   └── page.jsx         # Public landing page
├── components/          # Shared UI components
├── convex/              # Database schema, queries, and mutations
├── lib/
│   └── inngest/         # Background job functions (reminders + AI insights)
└── public/              # Static assets
```

---

## Known issues & notes

- Inngest requires v3.54.0 or later due to a security vulnerability in older versions
- If `npm install` fails, use `npm install --legacy-peer-deps`
- The Gemini API key must be set before starting the dev server — the AI module initialises on startup

---

*Built with Next.js, Convex, Clerk, Inngest, Resend, and Google Gemini AI.*
