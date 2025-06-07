
# Prompts for Claude – Triage Diagnostics Web Project

This file contains all the prompts you can use with Claude (5.7 or later) to build and integrate various parts of the Triage Diagnostics website.

---

## 🧱 1. Project Scaffold

**Prompt**:
```
Scaffold a modern licensing website using Next.js (TypeScript), Tailwind CSS, and NextAuth.js with Credentials Provider. Set up PostgreSQL using Prisma. Use the following structure:

- /pages for routes
- /components for UI elements
- /lib/prisma.ts for DB connection
- /lib/auth.ts for auth config
- /styles/globals.css for Tailwind setup
- Stripe integration (Checkout + Customer Portal)
- LicenseSpring (Business Starter) integration after purchase
```

---

## 🎨 2. Tailwind Branding Setup

**Prompt**:
```
Configure Tailwind CSS with this custom theme for medical and diagnostic branding:

- sterileWhite: #F8F9FA
- scrubGreen: #B2D8C4
- surgicalBlue: #AED9E0
- stethoscopeGrey: #A0A4A8
- antisepticAqua: #D1FAF0
- pulseNavy: #2A4D69
- emergencyRed: #C73624
- ambulanceWhite: #E7E4DD

Apply these to buttons, backgrounds, cards, text, and borders.
```
Make generous use of Google Materials Icons.
---

**Prompt**

Write up pages and copy for the website based on the benefits of the software and the value it offers IT services desks and their clients.


## 🔐 3. Authentication (NextAuth.js)

**Prompt**:
```
Set up NextAuth.js using Credentials Provider with Prisma. Create:
- `/pages/api/auth/[...nextauth].ts`
- `lib/auth.ts`
- `User` model with email, hashed password, and createdAt
- Registration and login forms
- Protected `/dashboard` route

Use secure cookie sessions and error handling for login failures.
```

---

## 💳 4. Stripe Integration

**Prompt**:
```
Integrate Stripe Checkout into the app. After a successful payment:
- Store the purchase in PostgreSQL with userId, amount, and sessionId
- Trigger a webhook to create a license via LicenseSpring
- so add Stripe Customer Portal access from the dashboard for reoccuring billing and licence and entitlement changes - this should be a fully self-service portal
```

---

## 🔁 5. LicenseSpring Webhook Integration

**Prompt**:
```
Create a webhook handler for Stripe events. On successful checkout:
- Use LicenseSpring’s API to provision a new license
- Store the returned licenseKey and related fields in PostgreSQL
- Connect it to the user’s account

Use `LICENSESPRING_API_KEY` and `PRODUCT_CODE` from env.
Implement error handling and retry logic for API failures.
```

---

## 🗃️ 6. Prisma Schema

**Prompt**:
```
Write a Prisma schema for PostgreSQL using UUIDs. Models:
- User: email, password, createdAt
- License: userId, licenseKey, stripeCustomerId, productCode, expiresAt
- Entitlement: deviceId, licenseId, assignedAt
- Purchase: stripeSessionId, amount, status, userId

Include relations and timestamps. Generate seed and migration files.
```

---

## 🧪 7. Environment Configuration

**Prompt**:
```
Generate a `.env.example` file with the following:

DATABASE_URL=
NEXTAUTH_SECRET=
NEXTAUTH_URL=
STRIPE_SECRET_KEY=
STRIPE_PUBLISHABLE_KEY=
STRIPE_WEBHOOK_SECRET=
STRIPE_PRICE_ID=
LICENSESPRING_API_KEY=
LICENSESPRING_PRODUCT_CODE=
```

---

## 🧑‍💻 8. User Dashboard

**Prompt**:
```
Create a responsive user dashboard using Tailwind:
- Display user's license(s), licenseKey, and expiry
- Link to Stripe Customer Portal
- Show list of device entitlements with assignedAt date
- Include a button to "Buy License" (calls Stripe Checkout)
```

---

## 🧾 9. Deployment Scripts

**Prompt**:
```
Write two shell scripts:

1. deploy-dev.sh:
- git pull from dev
- npm install
- npm run build
- pm2 restart triage-dev

2. deploy-prod.sh:
- git pull from main
- export NODE_ENV=production
- npm install
- npm run build
- pm2 restart triage-prod

Output logs to /var/log/triage-dev.log and triage-prod.log
```

---

## 🚀 10. Testing & Local Webhook

**Prompt**:
```
Add a local test endpoint at `/api/license/provision` for webhook simulation.
Send mock Stripe data and call LicenseSpring API to verify license provisioning.
Return success or error as JSON.
```

---

## 📦 11. To Do Checklist

**Prompt**:
```
Generate a GitHub-friendly checklist for the Triage project:

- [ ] Setup Tailwind with custom theme
- [ ] Auth with NextAuth (credentials)
- [ ] Stripe Checkout & Portal integration
- [ ] LicenseSpring webhook + API calls
- [ ] Prisma schema and migrations
- [ ] Build user dashboard
- [ ] Write deployment scripts
- [ ] Configure .env and PM2
```
