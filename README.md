
# Triage Diagnostics Website

A modern licensing and customer portal for **Triage Diagnostics**, a professional diagnostic utility built for IT service desks.  
This site powers the marketing, licensing, and user dashboard functionality for Triage customers.

---

## 🌐 Live Site

[https://www.thetriageapp.com](https://www.thetriageapp.com)  
Hosted on a self-managed **aaPanel** Node.js server with PM2 process control.

---

## 🛠️ Tech Stack

| Layer              | Technology                              |
|-------------------|------------------------------------------|
| Frontend           | Next.js (TypeScript, React)              |
| Styling            | Tailwind CSS                            |
| Auth               | NextAuth.js (Credentials provider)      |
| Payments           | Stripe Checkout + Stripe Customer Portal|
| Licensing          | LicenseSpring (Business Starter)        |
| Database           | PostgreSQL + Prisma ORM                 |
| Hosting            | aaPanel (Node.js + PM2)                 |

---

## 🎨 Branding & Colours

Custom Tailwind theme using the following palette:

| Name              | Hex       |
|-------------------|-----------|
| Sterile White     | `#F8F9FA` |
| Scrub Green       | `#B2D8C4` |
| Surgical Blue     | `#AED9E0` |
| Stethoscope Grey  | `#A0A4A8` |
| Antiseptic Aqua   | `#D1FAF0` |
| Pulse Navy        | `#2A4D69` |
| Emergency Red     | `#C73624` |
| Ambulance White   | `#E7E4DD` |

---

## 🔐 User Flow

1. **Sign Up / Login** via NextAuth.js (Credentials)
2. **Purchase License** via Stripe Checkout
3. **Stripe Webhook** creates License via LicenseSpring API
4. License key stored in PostgreSQL and shown on user dashboard
5. **Customer Portal** link allows users to manage billing

---

## 🗃️ Prisma Models

```
model User {
  id        String   @id @default(uuid())
  email     String   @unique
  password  String
  createdAt DateTime @default(now())
  licenses  License[]
  purchases Purchase[]
}

model License {
  id             String   @id @default(uuid())
  licenseKey     String
  productCode    String
  userId         String
  user           User     @relation(fields: [userId], references: [id])
  stripeCustomerId String
  createdAt      DateTime @default(now())
  expiresAt      DateTime
  entitlements   Entitlement[]
}

model Entitlement {
  id         String   @id @default(uuid())
  licenseId  String
  deviceId   String
  assignedAt DateTime @default(now())
  license    License  @relation(fields: [licenseId], references: [id])
}

model Purchase {
  id              String   @id @default(uuid())
  stripeSessionId String
  amount          Int
  status          String
  userId          String
  user            User     @relation(fields: [userId], references: [id])
  createdAt       DateTime @default(now())
}
```

---

## ⚙️ Deployment

### Dev Script
`deploy-dev.sh`
```bash
git pull origin dev
npm install
npm run build
pm2 restart triage-dev
```

### Prod Script
`deploy-prod.sh`
```bash
git pull origin main
export NODE_ENV=production
npm install
npm run build
pm2 restart triage-prod
```

---

## 📁 .env.example

```
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/triage

# Auth
NEXTAUTH_SECRET=your_secret
NEXTAUTH_URL=https://www.thetriageapp.com

# Stripe
STRIPE_SECRET_KEY=sk_test_...
STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
STRIPE_PRICE_ID=price_...

# LicenseSpring
LICENSESPRING_API_KEY=your_api_key
LICENSESPRING_PRODUCT_CODE=your_product_code
```

---

## 📦 To Do

- [ ] Dashboard UI
- [ ] Stripe Checkout integration
- [ ] Stripe Webhook handler
- [ ] LicenseSpring API calls
- [ ] PM2 ecosystem config
- [ ] Responsive Tailwind layout

---

## 🧠 About

Built and maintained by [Kahu Software](https://github.com/simonfalconer1979).  
Licensed software product. All rights reserved.
