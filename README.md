# Shivya's Nail Studio

A premium nail art booking website built with Next.js. The app lets customers sign in, choose nail services and enhancements, pick an available appointment time, and submit a booking. It also includes an admin area for managing services, bookings, reports, and studio operations.

Live deployment: [https://shivyas-nail-art-weld.vercel.app/](https://shivyas-nail-art-weld.vercel.app/)

## Project Overview

Shivya's Nail Studio is designed as a full booking experience for a luxury nail artist. The public site introduces the studio, displays curated services, and guides customers into a services-first booking flow.

Main capabilities:

- Luxury home page with service highlights, gallery sections, booking steps, and branded visuals.
- Customer login and signup using Supabase Auth when configured, with a local demo fallback for development.
- Services page with multi-service selection and optional treatment enhancements.
- Booking page with calendar, time-slot availability, customer details, total duration, and total price.
- Booking conflict checks so overlapping appointments cannot be created for the same date and time.
- Booking confirmation flow and optional email notifications.
- Admin dashboard for bookings, services, reports, and revenue overview.
- Supabase-backed API routes for services, bookings, availability, sitemap, robots, reminders, and email.
- Jest and Playwright test setup for unit, integration, and end-to-end testing.
- Vercel-ready configuration.

## Tech Stack

- Next.js 14
- React 18
- TypeScript
- Tailwind CSS and CSS modules
- Supabase
- Framer Motion
- Lucide React icons
- Jest and Testing Library
- Playwright
- Vercel

## Important Links

- Production site: [https://shivyas-nail-art-weld.vercel.app/](https://shivyas-nail-art-weld.vercel.app/)
- Home: `/`
- Login and signup: `/login`
- Services: `/services`
- Booking: `/book`
- Booking confirmation: `/book/confirmed`
- Contact: `/contact`
- Admin dashboard: `/admin`
- Admin bookings: `/admin/bookings`
- Admin services: `/admin/services`
- Admin reports: `/admin/reports`

## Project Structure

```text
.
|-- pages/                  # Next.js pages and API routes
|-- components/             # Shared UI, booking flow, admin UI, and layout components
|-- lib/                    # Supabase clients, auth helpers, content, SEO, mappers, utilities
|-- styles/                 # Global styles and page-level CSS modules
|-- public/                 # Static assets, images, robots file, service worker
|-- database/               # Supabase/Postgres schema
|-- scripts/                # Environment validation scripts
|-- utils/                  # Admin helper types and formatters
`-- __tests__/              # Jest, integration, component, and Playwright tests
```

## Getting Started

### Prerequisites

- Node.js 18 or newer
- npm
- Supabase project
- Vercel account for deployment

### Installation

```bash
npm install
```

Create a local environment file:

```bash
cp .env.example .env.local
```

Add your Supabase keys and admin settings to `.env.local`, then start the app:

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

## Environment Variables

The starter variables are listed in `.env.example`.

Required for the database-backed app:

```bash
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key
```

Required for admin access and production validation:

```bash
ADMIN_SECRET=your_admin_password
ADMIN_EMAIL=admin@example.com
NEXT_PUBLIC_IMAGE_DOMAIN=yourdomain.supabase.co
```

Recommended for deployment URLs, SEO, email links, and generated sitemap URLs:

```bash
NEXT_PUBLIC_APP_URL=https://shivyas-nail-art-weld.vercel.app
NEXT_PUBLIC_SITE_URL=https://shivyas-nail-art-weld.vercel.app
```

Optional integrations:

```bash
NEXT_PUBLIC_WHATSAPP_NUMBER=1234567890
NEXT_PUBLIC_INSTAGRAM_URL=https://instagram.com/your-profile
NEXT_PUBLIC_FACEBOOK_URL=https://facebook.com/your-page
NEXT_PUBLIC_TWITTER_URL=https://twitter.com/your-profile

NEXT_PUBLIC_EMAIL_PROVIDER=resend
RESEND_API_KEY=your_resend_key
SENDGRID_API_KEY=your_sendgrid_key
NEXT_PUBLIC_EMAIL_FROM=onboarding@resend.dev
NEXT_PUBLIC_EMAIL_REPLY_TO=hello@shivyasnailstudio.com

CRON_SECRET=your_cron_secret
NEXT_PUBLIC_ANALYTICS_ID=your_analytics_id
NEXT_PUBLIC_SENTRY_DSN=your_sentry_dsn
```

AI helper files are present in the repo. If you enable those features, add the matching AI provider keys used by `lib/ai-ready.js`.

Validate required variables:

```bash
npm run validate-env
```

## Database Setup

The database source of truth is:

```text
database/schema.sql
```

To set up Supabase:

1. Create a Supabase project.
2. Open `Project Settings -> API`.
3. Copy the project URL, anon key, and service role key into `.env.local`.
4. Open the Supabase SQL Editor.
5. Run the contents of `database/schema.sql`.
6. Confirm that the `services` and `bookings` tables exist.

Main tables:

- `services`: stores service title, description, duration, price, image URL, and category.
- `bookings`: stores customer contact details, selected services, booking date, booking time, status, notes, and total price.

The availability API checks existing bookings for conflicts. It also tries to read a `workingHours` table if available, and falls back to `09:00` to `18:00` when that table is not configured.

If appointment reminders are enabled, add a `reminder_sent` boolean column to `bookings` because the cron reminder route reads and updates that field.

More database details are available in `DATABASE_INTEGRATION.md`.

## Available Scripts

```bash
npm run dev              # Start the local Next.js dev server
npm run build            # Build the app for production
npm run start            # Start the production build
npm run lint             # Run ESLint
npm run lint:fix         # Fix lint issues where possible
npm run type-check       # Run TypeScript checks
npm run test             # Run Jest tests
npm run test:watch       # Run Jest in watch mode
npm run test:coverage    # Generate Jest coverage report
npm run e2e              # Run Playwright tests
npm run e2e:ui           # Open Playwright UI mode
npm run e2e:debug        # Debug Playwright tests
npm run e2e:report       # Open the Playwright report
npm run test:all         # Run lint, coverage, and e2e tests
npm run pre-deploy       # Run tests and environment validation
npm run build-analyze    # Analyze production bundle size
npm run deploy           # Deploy to Vercel production
npm run deploy-preview   # Deploy a Vercel preview
```

## API Routes

Customer and public routes:

- `GET /api/services`
- `GET /api/services/[id]`
- `POST /api/bookings`
- `GET /api/available-times?date=YYYY-MM-DD&duration=60`
- `POST /api/send-email`
- `GET /api/sitemap`
- `GET /api/robots`

Optional AI helper routes:

- `POST /api/ai/chat`
- `POST /api/ai/caption`
- `POST /api/ai/recommendations`

Cron and debugging routes:

- `GET /api/cron/reminders`
- `GET /api/cron/debug`

Admin-protected routes:

- `GET /api/bookings`
- `PUT /api/bookings/[id]`
- `DELETE /api/bookings/[id]`
- `POST /api/services`
- `PUT /api/services/[id]`
- `DELETE /api/services/[id]`

Admin API requests must send:

```text
Authorization: Bearer your_admin_secret
```

## Deployment

This project is deployed on Vercel:

```text
https://shivyas-nail-art-weld.vercel.app/
```

For Vercel deployment:

1. Push the project to GitHub.
2. Import the repository in Vercel.
3. Add all required environment variables in Vercel project settings.
4. Set production URLs:

```bash
NEXT_PUBLIC_APP_URL=https://shivyas-nail-art-weld.vercel.app
NEXT_PUBLIC_SITE_URL=https://shivyas-nail-art-weld.vercel.app
```

5. Deploy.

You can also deploy from the CLI:

```bash
npm run deploy
```

## Testing

Run the main test suite:

```bash
npm run test
```

Run browser end-to-end tests:

```bash
npm run e2e
```

Run the full pre-deployment check:

```bash
npm run pre-deploy
```

## Admin Notes

The admin pages live under `/admin`. The API layer checks `ADMIN_SECRET` through the `Authorization` header.

Before sharing admin access in production, keep the admin UI password and backend `ADMIN_SECRET` aligned, and avoid committing real secrets to the repository.

## Content Notes

The customer-facing Shivya service menu is currently defined in:

```text
lib/shivyaContent.ts
```

This file controls the main service list, enhancements, booking steps, navigation links, and site name used by the public pages.

Supabase service records are used by the API and admin management flows. Keep the static content and database records consistent when changing prices, durations, or service names.

## License

Private project for Shivya's Nail Studio.
