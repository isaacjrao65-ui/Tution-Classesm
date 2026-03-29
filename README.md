# BrightPath Tuition Website (MVP)

Production-style Next.js fullstack implementation for Class 6 to 10 tuition management.

## Included Modules
- Public site: Home, About, Courses, Batches, Fees, Contact
- Student auth: register/login/OTP request + verify endpoint scaffold
- Student dashboard: enrollments, payments, invoices, materials, doubt sessions
- Admin dashboard: session scheduling, exports, management endpoints
- Payments: Razorpay order creation, verify endpoint, webhook endpoint
- Integrations: Google Meet session creation, Google Sheets export (manual + nightly)
- Security: JWT cookie auth, RBAC middleware/proxy, API access checks
- Data model: Supabase SQL migration for all core entities

## Tech Stack
- Next.js 16 (App Router)
- TypeScript + Tailwind CSS
- Supabase (Postgres + Storage ready)
- Razorpay
- Google Calendar API (Meet links)
- Google Sheets API

## 1) Setup
```bash
npm install
cp .env.example .env.local
```

Fill `.env.local` values.

## 2) Database
Run `supabase/migrations/0001_init.sql` in Supabase SQL editor.

Create at least one admin user manually:
1. Insert row into `users` table with `role='admin'`.
2. `password_hash` must be bcrypt hash (you can register as student then promote role to admin).

## 3) Run
```bash
npm run dev
```
Open `http://localhost:3000`.

## 4) API Surface
- Auth: `/api/auth/register`, `/api/auth/login`, `/api/auth/request-otp`, `/api/auth/verify-otp`
- Catalog: `/api/classes`, `/api/classes/{id}/subjects`, `/api/batches`
- Enrollment: `/api/enrollments`
- Payments: `/api/payments/create-order`, `/api/payments/verify`, `/api/webhooks/razorpay`
- Student: `/api/students/me/enrollments`, `/api/students/me/payments`, `/api/students/me/invoices/{id}`, `/api/students/me/sessions`, `/api/students/me/doubts`
- Materials: `/api/materials`, `/api/materials/{id}/download`
- Admin: `/api/admin/classes`, `/api/admin/subjects`, `/api/admin/batches`, `/api/admin/fee-plans`, `/api/admin/materials`, `/api/admin/sessions`
- Exports: `/api/admin/exports/sheets/manual`, `/api/internal/exports/sheets/nightly`

## 5) Production Notes
- OTP routes are scaffolded; connect SMTP/SMS provider for real delivery.
- Material download currently returns stored file path; wire signed Supabase Storage URLs.
- Nightly export endpoint expects `x-cron-secret` header matching `CRON_SECRET`.
- Deploy to Vercel and configure env vars in project settings.

## 6) Verification
```bash
npm run lint
npm run build
```
Both pass with this implementation.
