# Online Booking System

## Overview

Atelier Bookings is a secure appointment operations dashboard for service businesses. Customers book available staff, while staff and administrators see the complete schedule and capacity picture.

## Features

- Responsive schedule, utilization, revenue, team-capacity, and booking dashboard
- Database-backed sessions and administrator, staff, and customer roles
- Validated booking creation with service duration, advance-notice, and overlap checks
- Customer-owned booking views and organization-wide staff/admin views
- PostgreSQL schema, migration, seed data, tests, Docker, and CI

## Technology Stack

Next.js 16 App Router, React 19, TypeScript 5, Node.js 24, PostgreSQL 18, Prisma ORM 7, Zod 4, and Vitest 5.

## Requirements

Node.js 24+, npm 11+, and PostgreSQL 16+ or Docker Desktop.

## Installation

```bash
git clone https://github.com/Arjunren/online-booking-system.git
cd online-booking-system
npm install
cp .env.example .env
```

## Environment Variables

Set `DATABASE_URL` for runtime database access and `DIRECT_URL` for migrations. `SESSION_TTL_HOURS` is optional and defaults to 12. Never commit real secrets.

## Database Setup

```bash
npm run prisma:generate
npm run db:migrate
npm run db:seed
```

Use `npm run db:deploy` for production migrations.

## Running the Application

Run `npm run dev` and open `http://localhost:3000`, or use `docker compose up --build`.

## Running Tests

Run `npm test` for unit tests. `npm run check` generates Prisma Client, type-checks, tests, and builds production assets.

## Default Development Accounts

Development only. All accounts use `BookingDemo!2026`: administrator `admin@booking.local`, specialist `specialist@booking.local`, and customer `customer@booking.local`.

## API Endpoints

| Method | Endpoint | Access | Purpose |
| --- | --- | --- | --- |
| POST | `/api/auth/login` | Public | Create an HTTP-only session |
| POST | `/api/auth/logout` | Signed in | Revoke the active session |
| GET | `/api/bookings` | Signed in | Customer-owned or staff-wide schedule |
| POST | `/api/bookings` | Signed in | Create a validated booking |

## Folder Structure

`app/` provides App Router pages and APIs; `components/` contains the interactive schedule; `lib/` holds server-only security and validation; `repositories/` implements Prisma persistence; `services/` implements availability rules; `prisma/` contains schema, migration, and seed; `tests/` contains Vitest suites; and `types/` defines repository contracts.

## Security Notes

Passwords use salted scrypt hashes and timing-safe comparison. Session cookies are HTTP-only, same-site, secure in production, and backed by hashed tokens. Each route authenticates on the server; customer reads are ownership-scoped. Zod validates identifiers, timestamps, and notes. Availability is checked server-side and exact starts have a database uniqueness constraint. Production should additionally use serializable booking transactions, rate limiting, CSRF origin validation, audit logging, and a managed pooled database.

## Remaining Limitations

The dashboard uses representative preview data. Payments, recurring appointments, waitlists, external calendars, reminders, timezone preference management, and cancellation policies are outside this starter scope.
