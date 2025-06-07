
# Triage Diagnostics Website

A modern licensing and customer portal for **Triage Diagnostics**, a professional diagnostic utility built for IT service desks.  
This site powers the marketing, licensing, and user dashboard functionality for Triage customers.

---

## 🧩 The Problem: Supporting Remote Users at Scale
Modern IT helpdesks increasingly support remote, hybrid, and distributed users — often across various environments, devices, and connectivity conditions. Diagnosing technical issues remotely introduces real challenges:

### 🛑 Common Problems for IT Support Teams
- 🔍 **Lack of Visibility**: Users describe symptoms vaguely ("it's slow", "nothing works"), offering little actionable detail.
- 🔗 **Fragmented Tools**: Gathering logs, screenshots, and network data requires juggling multiple apps or guiding users step-by-step.
- 🕒 **Time Delays**: The back-and-forth process of gathering diagnostics is time-consuming — especially with non-technical users.
- ❌ **Inconsistent Data**: Reports vary in structure and completeness, slowing down triage.
- 🔐 **Limited Permissions**: Remote users may lack admin rights or the knowledge to use diagnostic tools correctly.

### ✅ The Solution: Triage Diagnostics
Triage Diagnostics gives helpdesk teams a powerful, one-click tool to collect consistent, professional-grade diagnostics — improving both the user experience and support team efficiency.

### 🔧 How It Helps
- 🖥️ **Complete System Snapshot**: Captures CPU, RAM, OS, software, disk usage, and more.
- 🌐 **Built-in Network Diagnostics**: Tests DNS, WAN, and internal targets with visual graphs.
- 🧾 **Standardized PDF Reports**: Includes system data, screenshots, and event logs in a clean, branded format.
- 📨 **Auto-Email to Support**: Diagnostic packages are automatically sent to the support inbox.
- ⚙️ **Customizable Deployment**: Deploy silently via RMM, InTune, or GPO — no user setup required.
- 🎛️ **Preference-Driven Capture**: Collect only the data you need per environment or use case.

### 🚀 Key Benefits

#### 🧑‍💻 For IT Teams
- 🔄 **Reduces back-and-forth**: No more chasing logs or screenshots
- 🕵️ **Improves accuracy of diagnosis**: Data is standardized and complete
- 🧠 **Empowers Level 1 staff**: Frontline engineers can make informed decisions faster — or escalate with full context
- 🚀 **Speeds time to resolution**: Issues can be diagnosed before even speaking to the user

#### 🙂 For End Users
- ✅ **No technical steps required**: Just click "Run" — or it runs silently
- ⏳ **Less downtime and frustration**: Users don't have to wait or explain symptoms repeatedly
- 🎯 **Faster resolution**: Accurate diagnostics = quicker fixes

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
3. **Stripe Webhook** creates License via LicenseSpring API; sends email to user with license key and instructions
4. License key and entitlements stored in PostgreSQL and shown on user dashboard
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
