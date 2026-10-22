# AUTOVISTA — High-Performance Automotive Showroom & Dealership Management System

AutoVista is a studio-grade, full-stack automotive showroom and dealership management system engineered for luxury, performance, and electrified mobility. Designed with the aesthetic discipline of an automotive design atelier, it features a public showroom website and a role-based dealership terminal.

---

## 🏎️ Repository Architecture

```
micro project/
├── client/                 # Frontend React 18 + Vite application
│   ├── public/             # Static assets & custom SVG emblem favicon
│   ├── src/
│   │   ├── components/     # Reusable layout & UI components
│   │   │   ├── home/       # Hero, ModelStrip, FeaturedModel, FinanceBand, WhyAutoVista
│   │   │   ├── layout/     # Auto-hiding Navbar, Studio Footer, PageWrapper, ScrollToTop
│   │   │   └── ui/         # SectionHeader, Badge, Button with wipe effect
│   │   ├── context/        # AuthContext (multi-role) & CarsContext (fleet management)
│   │   ├── data/           # High-fidelity mockCars dataset (22 real models with INR pricing)
│   │   ├── hooks/          # useScrollDirection, useScrollReveal, useAnimatedCounter
│   │   ├── pages/          # Home, Models, ModelDetail, Compare, Finance, BookTestDrive, About, Contact, Admin, NotFound
│   │   └── styles/         # tokens.css (color palette, Barlow Condensed & DM Sans typography, easing) & global.css
│   └── vite.config.js      # Vite configuration with /api reverse proxy to backend
├── server/                 # Backend Node.js + Express REST API
│   ├── src/
│   │   ├── middleware/     # JWT authentication & role-based access control (Admin, Sales, Service)
│   │   ├── routes/         # auth, cars, leads, test-drives, sales, invoices, service-jobs, customers, reports
│   │   ├── db.js           # MySQL connector pool with automated high-performance memory fallback
│   │   └── index.js        # Main Express server entrypoint
│   ├── .env                # Server environment configuration
│   └── package.json
└── database/               # Database schemas & seed scripts
    ├── schema.sql          # 12 relational MySQL tables with foreign keys and indexes
    └── seed.sql            # Seed dataset: 22 cars, 15 customers, 12 leads, 10 sales, 8 service jobs, 4 staff, 3 roles
```

---

## ⚡ Tech Stack

- **Frontend**: React 18, Vite, React Router v6, Framer Motion (route transitions only), Plain CSS Modules
- **Design System**: Barlow Condensed (Display uppercase, tight tracking), DM Sans (Body), Near-Black (`#0D0D0E`), Warm Off-White (`#F8F7F4`), Signal Red (`#E11D2A`)
- **Backend**: Node.js, Express, JWT Authentication, bcryptjs, REST API
- **Database**: MySQL 8.0+ (with resilient in-memory seeded store fallback for instant zero-dependency execution)

---

## 🚀 Quick Start Guide

### 1. Prerequisites
- Node.js (v18.0.0 or higher)
- npm (v9.0.0 or higher)
- MySQL 8.0+ (optional, server auto-falls back to high-fidelity seed store if MySQL is not configured)

---

### 2. Backend Setup (`/server`)

```bash
cd server
npm install
npm run dev
```

The Express API server will start on `http://localhost:5000`.

#### Environment Configuration (`server/.env`)
```ini
PORT=5000
NODE_ENV=development
JWT_SECRET=autovista_super_secret_jwt_key_2025_automotive
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your_mysql_password
DB_NAME=autovista_db
```

---

### 3. Database Setup (MySQL)

To create the database and seed it directly in your MySQL server:

```bash
# Create schema
mysql -u root -p < database/schema.sql

# Seed data (22 cars, 15 customers, 12 leads, 10 sales, 8 service jobs, 4 staff, 3 roles)
mysql -u root -p < database/seed.sql
```

---

### 4. Frontend Setup (`/client`)

In a new terminal:

```bash
cd client
npm install
npm run dev
```

The client showroom will start on `http://localhost:5173`.

---

## 🔑 Dealership Terminal Credentials (`/admin`)

The Admin Dashboard includes a **Quick Role Switcher** and pre-seeded credentials:

| Role | Email | Password | Assigned Permissions |
| :--- | :--- | :--- | :--- |
| **Admin** | `admin@autovista.com` | `admin123` | Full Access: Overview, Inventory CRUD, Leads Kanban, Test Drives, Sales, Invoices, Service, Reports & CSV Export |
| **Sales** | `sales@autovista.com` | `sales123` | Overview, Inventory, Leads Kanban, Test Drive Bookings, Sales & Invoices, Reports |
| **Service** | `service@autovista.com` | `service123` | Overview, Workshop Service Job Cards, Inventory View |

---

## ✨ Part A: Public Showroom Features

1. **Home Page**:
   - **Hero Section**: Car image slides in from the right (`translateX(8%)` to `0`) while fading in over `1000ms`, with slow continuous zoom (`scale(1)` to `scale(1.06)` over `14s`). Line-by-line headline reveal with `120ms` stagger.
   - **Model Strip**: Horizontal scroll-snap carousel with thin progress line that grows dynamically on scroll. Card hover lifts `6px`, image scales to `1.05`, and "View Details" arrow slides in from the left.
   - **Featured Model**: Spotlight vehicle with **400ms cross-fading color swatches** and scaling swatch rings.
   - **Finance Band**: Live interactive monthly EMI calculation with down payment and amortization sliders.
   - **"Why AutoVista"**: Section with **1.4s ease-out animated counters** counting up when entering the viewport.
2. **Models Catalog**:
   - Filter by category (Sedans, SUVs, Electric, Coupes), powertrain, and search by make/model.
   - Quick "Add to Compare" badge.
3. **Model Detail**:
   - Large image stage with cross-fading color swatches.
   - Spec tabs: **Performance & Powertrain**, **Dimensions & Chassis**, **Bespoke Features & Tech**.
   - Photographic gallery and similar vehicle recommendations.
   - Direct CTA links to "Book Private Test Drive" and "Get Customized EMI Quote".
4. **Compare Matrix**:
   - Side-by-side technical benchmarking for up to 3 vehicles.
5. **Book Test Drive**:
   - 3-step interactive booking form with validation.
   - Automatically pipes reservations as leads into the Admin Leads Pipeline.
   - Celebratory confirmation screen with appointment pass.
6. **Finance & EMI Calculator**:
   - 3-step bespoke financing structuring with real-time amortization, tax lease optimization, and celebratory quotation dossier.
7. **About & Contact**:
   - Studio heritage, national showroom directory (Mumbai, Delhi NCR, Bengaluru, Hyderabad), and contact forms.

---

## 🛠️ Part B: Role-Based Admin Dashboard (`/admin`)

- **Overview**: Real-time KPI cards (Fleet Value Delivered, Inventory, Leads Pipeline, Workshop Service), recent inquiries stream, and scheduled test drives.
- **Inventory Management**: Search & filter fleet, add new vehicle modal, update status (`In Stock`, `Reserved`, `Sold`), and decommission vehicles.
- **Leads Pipeline**: 6-stage Kanban board (`New`, `Contacted`, `Qualified`, `Negotiation`, `Won`, `Lost`) with one-click stage progression and public source attribution tags.
- **Test Drive Bookings**: Track schedule management and status completion.
- **Sales & Invoices**: Transaction ledger with **Printable Luxury Tax Invoice Dossier** modal calculating 12% integrated GST.
- **Service Jobs**: High-voltage battery balancing and track dyno calibration job cards with status progression.
- **Reports & Export**: KPI overview with 4 direct CSV export downloads (Sales CSV, Fleet CSV, Leads CSV, Service CSV).
- **Role-Based Access Control**: Top-bar role switcher enforcing strict privilege separation.

---

## 📋 Animation & Quality Verification

- **Easing**: `cubic-bezier(0.22, 1, 0.36, 1)` across all entrances.
- **Durations**: 600–900ms entrances, 200–300ms hovers, 350ms route transitions.
- **Buttons**: Hover fill wipe across (pseudo-element `scaleX(0)` to `1`, left origin), 3px focus ring.
- **Navbar**: Transparent over hero; solid dark background with backdrop blur and 2px shadow after 80px scroll; auto-hides on scroll down and shows on scroll up.
- **Reduced Motion**: Full support for `prefers-reduced-motion: reduce`.
- **Performance**: 60fps hardware-accelerated animations animating only `transform` and `opacity`.
