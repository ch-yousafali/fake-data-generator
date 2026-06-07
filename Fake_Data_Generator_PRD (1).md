# Product Requirements Document (PRD)

## Fake Data Generator

**Version:** 1.0  
**Date:** June 2026  
**Status:** Draft  
**Scope:** 1-Hour Build (Single-File, Client-Side)  

---

## 1. Executive Summary

A lightweight, browser-based fake data generator that produces realistic names, emails, addresses, UUIDs, and full entities (users, companies, products, orders) for developers who need seed data for testing APIs, populating databases, or building UI prototypes. No backend, no accounts, no rate limits — pure client-side generation with one-click copy and export.

**Core Value Proposition:**  
*"Realistic seed data in seconds. No signup, no API keys, no bloat."*

---

## 2. Problem Statement

### 2.1 Current Pain Points

| Pain Point | Current Solution | Why It Sucks |
|------------|-----------------|--------------|
| Need 50 fake users for a database seed | Mockaroo / Faker API | Rate limits, requires account, slow for quick tests |
| Need a single realistic email for a form test | Manually typing "test@test.com" | Not realistic, breaks validation sometimes |
| Need varied product data for an e-commerce prototype | Hardcoding 5 products in a JSON file | Tedious, not scalable, looks fake |
| Need UUIDs for relational seed data | uuidgenerator.net | One at a time, no context |
| Need CSV for importing into Excel/Sheets | Writing a Python script | Overkill for a quick task |

### 2.2 Root Cause

Existing tools are either:
- **API-dependent** (rate-limited, requires network, can break)
- **Account-gated** (Mockaroo free tier = 1,000 rows max, paid for more)
- **Over-engineered** (schema builders, saved projects, team sharing — you just need data)
- **Single-purpose** (one site for UUIDs, another for names, another for addresses)

### 2.3 Opportunity

Build a **zero-config, zero-backend, single-page tool** that:
- Generates any amount of data instantly (1–1000 rows)
- Produces realistic, contextual data (not random strings)
- Copies individual fields or exports full datasets
- Works offline after first load
- Requires zero setup

---

## 3. Target Users

### 3.1 Primary Personas

#### Persona 1: "Backend Dev Seeding" — Alex
- **Role:** Backend developer building a new API
- **Goal:** Generate 100 realistic user records to test pagination, filtering, and search
- **Frustration:** Writing seed scripts takes 30 minutes; Mockaroo requires login
- **Success Criteria:** Paste a URL, get 100 JSON objects in 10 seconds

#### Persona 2: "Frontend Prototyper" — Jamie
- **Role:** Frontend developer building UI components
- **Goal:** Fill a table, card grid, or form with realistic-looking data
- **Frustration:** "Lorem ipsum" names and "user1@email.com" look unprofessional in demos
- **Success Criteria:** Copy-paste realistic names and avatars into Figma or code

#### Persona 3: "QA Tester" — Priya
- **Role:** QA engineer testing form validation
- **Goal:** Generate edge-case data: very long names, special characters, international emails
- **Frustration:** Manually typing test data is slow and inconsistent
- **Success Criteria:** Generate 50 varied records with one click, including edge cases

### 3.2 Secondary Users

- **Data Analyst:** Needs sample CSVs to test Excel formulas or BI dashboards
- **Student:** Learning SQL and needs data to practice JOINs and aggregations
- **Designer:** Needs realistic content for mockups and client presentations

---

## 4. Functional Requirements

### 4.1 Core Features (MVP — 1 Hour Build)

#### FR-001: Entity Type Selection
- **Description:** User selects what kind of fake data to generate.
- **Acceptance Criteria:**
  - Four entity types available in a tab or dropdown:
    - **User** (name, email, phone, address, avatar URL, age, job title, company)
    - **Company** (name, catch phrase, BS statement, industry, address, email, website, employees count)
    - **Product** (name, description, price, category, SKU, in stock, rating, image URL)
    - **Order** (order ID, customer name, items count, total, status, date, shipping address)
  - Default selection: "User"
  - Changing entity type instantly updates the preview and output

#### FR-002: Field-Level Generation (Quick Actions)
- **Description:** Generate individual fields on demand for quick copy-paste.
- **Acceptance Criteria:**
  - Display a grid of common fields as buttons:
    - Name, Email, Phone, UUID, Address, Date, Paragraph, URL, Hex Color, IP Address
  - Clicking a button generates a new value and copies it to clipboard
  - Show a toast/notification: "Copied: john.doe@example.com"
  - Each button shows the current generated value inline (so you can see before copying)

#### FR-003: Bulk Generation (Structured Data)
- **Description:** Generate N rows of structured data for the selected entity.
- **Acceptance Criteria:**
  - Count input: 1 to 1000 rows (default: 10)
  - Slider + number input for quick adjustment
  - Output formats: JSON (pretty), JSON (minified), CSV
  - Format toggle: JSON / CSV / SQL INSERT statements
  - Live preview: Show first 5 rows in a table/cards as a preview
  - "Generate" button re-randomizes all data
  - "Copy All" button copies the full output to clipboard
  - "Download" button saves as `.json`, `.csv`, or `.sql` file

#### FR-004: SQL Export
- **Description:** Generate SQL INSERT statements for direct database import.
- **Acceptance Criteria:**
  - Auto-generate table name from entity type (e.g., `users`, `companies`)
  - Editable table name field
  - Output format: `INSERT INTO users (name, email, ...) VALUES (...);`
  - Handle string escaping (quotes, newlines)
  - One statement per row (simpler than multi-row inserts for MVP)

#### FR-005: Customization Options
- **Description:** Allow basic customization of generated data.
- **Acceptance Criteria:**
  - Locale/region selector: US (default), UK, DE, FR, JP (affects names, addresses, phone formats)
  - Gender option for names: Random, Male, Female (applies to User entity only)
  - Include/exclude fields per entity (checkboxes):
    - User: toggle avatar URL, job title, company, age
    - Product: toggle SKU, image URL, rating
    - etc.
  - Seed option: Optional integer seed for reproducible random data (same seed = same output)

### 4.2 Future Features (Post-1-Hour)

#### FR-006: Custom Schema Builder
- Drag-and-drop field types to build a custom entity
- Field types: String, Number, Boolean, Date, Enum, UUID, Email, URL, Paragraph, Name, Address, Phone, Image URL, Lorem Ipsum, Custom Regex

#### FR-007: Save & Load Presets
- Save frequently used configurations to localStorage
- Name presets (e.g., "E-commerce Users", "SaaS Companies")

#### FR-008: GraphQL / JSON Schema Output
- Generate data matching a provided JSON Schema
- Generate GraphQL mock data for a given type definition

#### FR-009: Image Placeholders
- Generate placeholder avatar/product images using UI Faces / DiceBear API
- Or generate SVG data URIs for colored placeholders

---

## 5. Non-Functional Requirements

### 5.1 Performance

| Requirement | Target | Notes |
|-------------|--------|-------|
| First meaningful paint | < 1 second | Single HTML file, minimal CSS |
| Generate 100 rows | < 100ms | Client-side only, no network |
| Generate 1000 rows | < 500ms | May show progress indicator |
| Copy to clipboard | < 50ms | Native Clipboard API |
| Download file | < 100ms | Blob + anchor download |

### 5.2 Reliability

- **Offline capable:** Works without internet after initial load (no external API calls for data generation)
- **No data loss:** All generation is ephemeral by design — no user data stored
- **Deterministic with seed:** Same seed + same config = identical output (useful for reproducible tests)

### 5.3 Browser Compatibility

- Chrome, Firefox, Safari, Edge (last 2 versions)
- Mobile: iOS Safari, Chrome Android
- Fallback for Clipboard API on older browsers (execCommand)

### 5.4 Accessibility

- WCAG 2.1 AA for color contrast
- Keyboard navigation for all interactive elements
- Screen reader labels for copy buttons
- Focus indicators visible

---

## 6. User Stories

### Story 1: Quick Field Copy
> **As a** developer testing a form,  
> **I want to** click a button to generate a realistic email and have it copied to my clipboard,  
> **So that** I can paste it into a registration form without typing.  
> **Acceptance:** One click → clipboard contains a valid-looking email. Toast confirmation appears.

### Story 2: Bulk JSON for API Testing
> **As a** backend developer,  
> **I want to** generate 50 user objects in JSON format,  
> **So that** I can paste them into my API test suite or seed script.  
> **Acceptance:** Select "User", set count to 50, click Generate, click Copy All. JSON is valid and contains varied realistic data.

### Story 3: CSV for Spreadsheet Import
> **As a** data analyst,  
> **I want to** generate 200 product records as a CSV file,  
> **So that** I can import them into Excel to test pivot tables and charts.  
> **Acceptance:** Select "Product", set count to 200, choose CSV format, click Download. File opens correctly in Excel with proper headers.

### Story 4: SQL for Database Seeding
> **As a** developer setting up a new project,  
> **I want to** generate SQL INSERT statements for 20 companies,  
> **So that** I can run them directly in my database client to populate a table.  
> **Acceptance:** Select "Company", set count to 20, choose SQL format, table name auto-set to `companies`. Output is valid SQL with proper escaping.

### Story 5: Reproducible Test Data
> **As a** QA engineer,  
> **I want to** use a fixed seed number to generate the same dataset every time,  
> **So that** my automated tests have consistent inputs.  
> **Acceptance:** Enter seed "42", generate 10 users. Clear, enter seed "42" again, generate 10 users. Output is identical.

---

## 7. UI/UX Requirements

### 7.1 Layout (Desktop)

```
┌─────────────────────────────────────────────────────────────┐
│  🎲 Fake Data Generator                              [🌙]  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐     │
│  │  👤 User │ │ 🏢 Company│ │ 📦 Product│ │ 📋 Order │     │
│  │  (active)│ │           │ │           │ │          │     │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘     │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  QUICK GENERATE (Individual Fields)                 │   │
│  │                                                     │   │
│  │  [👤 John Doe] [📧 john@example.com] [📱 (555)...] │   │
│  │  [🔑 a1b2-c3d4...] [🏠 123 Main St] [📅 2026-06-07]│   │
│  │  [🌐 https://...] [🎨 #3B82F6] [🌐 192.168.1.1]    │   │
│  │                                                     │   │
│  │  Click any field to copy to clipboard               │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  BULK GENERATOR                                     │   │
│  │                                                     │   │
│  │  Count: [━━●━━━━ 50] [50]                           │   │
│  │  Format: [JSON ▼]  Locale: [US ▼]  Gender: [Any ▼] │   │
│  │  Seed: [________] (optional)                        │   │
│  │                                                     │   │
│  │  Include fields: ☑ name ☑ email ☑ phone ☐ avatar    │   │
│  │                  ☑ address ☑ age ☑ job ☐ company     │   │
│  │                                                     │   │
│  │  [🔄 Generate] [📋 Copy All] [💾 Download .json]     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  PREVIEW (first 5 rows)                             │   │
│  │                                                     │   │
│  │  ┌──────┬─────────────────┬──────────────────────┐  │   │
│  │  │ Name │ Email           │ Phone                │  │   │
│  │  ├──────┼─────────────────┼──────────────────────┤  │   │
│  │  │ John │ john@example.com│ (555) 123-4567      │  │   │
│  │  │ Jane │ jane@example.com│ (555) 987-6543      │  │   │
│  │  └──────┴─────────────────┴──────────────────────┘  │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  OUTPUT                                             │   │
│  │  ┌──────────────────────────────────────────────┐   │   │
│  │  │ [                                            │   │   │
│  │  │   { "name": "John Doe", ... },               │   │   │
│  │  │   { "name": "Jane Smith", ... }              │   │   │
│  │  │ ]                                            │   │   │
│  │  └──────────────────────────────────────────────┘   │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 7.2 Layout (Mobile)

- Stacked layout: Entity tabs → Quick Generate (2-column grid) → Bulk Generator → Preview (horizontal scroll table) → Output (full-width textarea)
- Collapsible sections for Quick Generate and Bulk Generator
- Floating action button for "Generate" on mobile

### 7.3 Design Tokens

| Token | Value | Usage |
|-------|-------|-------|
| Primary | `#6366f1` (Indigo 500) | Buttons, active tabs, accents |
| Primary Hover | `#4f46e5` (Indigo 600) | Button hover states |
| Success | `#10b981` (Emerald 500) | Copy success, toast |
| Background | `#f8fafc` (Slate 50) | Page background |
| Card | `#ffffff` | Content cards |
| Text Primary | `#0f172a` (Slate 900) | Headings, primary text |
| Text Secondary | `#64748b` (Slate 500) | Labels, descriptions |
| Border | `#e2e8f0` (Slate 200) | Card borders, dividers |
| Code BG | `#1e293b` (Slate 800) | Output textarea background |
| Code Text | `#e2e8f0` (Slate 200) | Output text color |

### 7.4 Typography

- **Font:** Inter (Google Fonts) — weights 400, 500, 600, 700
- **Base size:** 16px (1rem)
- **Headings:** H1 1.75rem/700, H2 1.25rem/600, H3 1rem/600
- **Code:** JetBrains Mono or system monospace — 13px

### 7.5 Interactions

- **Button hover:** Slight lift (translateY -1px) + shadow increase
- **Copy feedback:** Button briefly shows "✓ Copied!" in green, then reverts
- **Generate animation:** Preview table rows fade in staggered (50ms delay each)
- **Toast notifications:** Slide in from bottom-right, auto-dismiss after 2 seconds
- **Dark mode toggle:** Instant theme switch, no page reload

---

## 8. Technical Architecture

### 8.1 Stack

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| **Runtime** | Browser (vanilla JS) | Zero backend, instant load, offline capable |
| **Data Generation** | Custom faker + seedrandom | No external API dependency, deterministic with seed |
| **UI Framework** | Vanilla HTML/CSS/JS | 1-hour scope, no build step needed |
| **Styling** | Tailwind CSS (CDN) | Rapid styling without writing CSS files |
| **Icons** | Heroicons (SVG inline) | No icon font dependency, crisp at all sizes |
| **Clipboard** | Clipboard API + fallback | Native modern API with execCommand fallback |
| **File Download** | Blob + URL.createObjectURL | Native browser API, no library needed |

### 8.2 File Structure (Single File)

```
index.html          ← Everything in one file:
  ├── <style>       Tailwind CDN + custom styles
  ├── <body>        Layout structure
  └── <script>      Data generation engine + UI logic
```

### 8.3 Data Generation Engine (Pseudocode)

```javascript
// Seeded random number generator (seedrandom or simple LCG)
function seededRandom(seed) { ... }

// Locale-aware data pools
const DATA = {
  en: {
    firstNames: { male: [...], female: [...] },
    lastNames: [...],
    domains: ['example.com', 'test.org', 'demo.net'],
    streets: ['Main St', 'Oak Ave', 'Maple Rd'],
    cities: ['Springfield', 'Riverside', 'Franklin'],
    jobTitles: ['Software Engineer', 'Product Manager', ...],
    companySuffixes: ['Inc', 'LLC', 'Corp'],
    industries: ['Technology', 'Healthcare', 'Finance'],
    productCategories: ['Electronics', 'Clothing', 'Home'],
    productAdjectives: ['Premium', 'Ergonomic', 'Wireless'],
    productNouns: ['Headphones', 'Keyboard', 'Monitor'],
    orderStatuses: ['Pending', 'Processing', 'Shipped', 'Delivered', 'Cancelled']
  }
  // ... de, fr, jp, etc.
};

// Generators per field type
function generateName(locale, gender) { ... }
function generateEmail(firstName, lastName, domain) { ... }
function generatePhone(locale) { ... }
function generateUUID() { ... }  // crypto.randomUUID() or polyfill
function generateAddress(locale) { ... }
function generateCompany(locale) { ... }
function generateProduct(locale) { ... }
function generateOrder(locale) { ... }
function generateDate(start, end) { ... }
function generateParagraph(sentences) { ... }
function generateNumber(min, max, decimals) { ... }
function generateBoolean() { ... }
function generateEnum(values) { ... }
function generateURL() { ... }
function generateHexColor() { ... }
function generateIPAddress() { ... }

// Entity builders
function buildUser(locale, gender, fields) {
  const firstName = generateName(locale, gender);
  const lastName = generateName(locale, 'last');
  return {
    name: fields.includes('name') ? `${firstName} ${lastName}` : undefined,
    email: fields.includes('email') ? generateEmail(firstName, lastName) : undefined,
    phone: fields.includes('phone') ? generatePhone(locale) : undefined,
    address: fields.includes('address') ? generateAddress(locale) : undefined,
    age: fields.includes('age') ? generateNumber(18, 80, 0) : undefined,
    jobTitle: fields.includes('jobTitle') ? generateEnum(DATA[locale].jobTitles) : undefined,
    company: fields.includes('company') ? generateCompany(locale) : undefined,
    avatar: fields.includes('avatar') ? `https://i.pravatar.cc/150?u=${generateUUID()}` : undefined
  };
}

// Similar for buildCompany, buildProduct, buildOrder

// CSV conversion
function toCSV(rows, fields) {
  const headers = fields.join(',');
  const lines = rows.map(row => fields.map(f => `"${row[f]}"`).join(','));
  return [headers, ...lines].join('\n');
}

// SQL conversion
function toSQL(rows, fields, tableName) {
  return rows.map(row => {
    const cols = fields.join(', ');
    const vals = fields.map(f => typeof row[f] === 'string' ? `'${row[f].replace(/'/g, "''")}'` : row[f]).join(', ');
    return `INSERT INTO ${tableName} (${cols}) VALUES (${vals});`;
  }).join('\n');
}
```

### 8.4 Seed Implementation

Use a simple Linear Congruential Generator (LCG) for reproducibility:

```javascript
function createSeededRandom(seed) {
  let s = seed || Date.now();
  return function() {
    s = (s * 16807 + 0) % 2147483647;
    return (s - 1) / 2147483646;
  };
}

// All generators use this random function when a seed is provided
```

---

## 9. Data Model / Output Schemas

### 9.1 User Entity

```json
{
  "id": "uuid",
  "name": "John Doe",
  "email": "john.doe@example.com",
  "phone": "(555) 123-4567",
  "address": "123 Main St, Springfield, IL 62701",
  "age": 34,
  "jobTitle": "Software Engineer",
  "company": "Acme Inc",
  "avatar": "https://i.pravatar.cc/150?u=uuid"
}
```

### 9.2 Company Entity

```json
{
  "id": "uuid",
  "name": "Acme Inc",
  "catchPhrase": "Revolutionary tertiary synergy",
  "bs": "streamline turn-key platforms",
  "industry": "Technology",
  "address": "456 Oak Ave, Riverside, CA 92501",
  "email": "contact@acme.com",
  "website": "https://acme.com",
  "employees": 250
}
```

### 9.3 Product Entity

```json
{
  "id": "uuid",
  "name": "Premium Wireless Headphones",
  "description": "High-quality wireless headphones with noise cancellation.",
  "price": 149.99,
  "category": "Electronics",
  "sku": "ELEC-001-ABC",
  "inStock": true,
  "rating": 4.5,
  "image": "https://via.placeholder.com/300x200?text=Headphones"
}
```

### 9.4 Order Entity

```json
{
  "id": "uuid",
  "orderNumber": "ORD-2026-0001",
  "customerName": "John Doe",
  "items": 3,
  "total": 299.97,
  "status": "Shipped",
  "date": "2026-06-07",
  "shippingAddress": "123 Main St, Springfield, IL 62701"
}
```

---

## 10. Success Metrics

Since this is a client-side tool with no analytics backend, success is measured by:

| Metric | How to Measure | Target |
|--------|---------------|--------|
| **Personal utility** | You use it within 24 hours of building | 100% |
| **Shareability** | You send the link to a teammate | 1+ person |
| **Build time** | Time from start to working tool | < 60 minutes |
| **Page load** | Lighthouse performance score | > 90 |
| **Functionality** | All 4 entity types generate correctly | 100% |
| **Export formats** | JSON, CSV, SQL all work | 100% |

---

## 11. 1-Hour Build Plan

| Time | Task | Deliverable |
|------|------|-------------|
| 0:00–0:05 | Scaffold HTML file with Tailwind CDN | `index.html` with layout structure |
| 0:05–0:15 | Build data generation engine (seeded random + name/email/address generators) | Working `generateUser()` function |
| 0:15–0:25 | Add remaining entities (Company, Product, Order) + field generators | All 4 entity types generate |
| 0:25–0:35 | Build UI: entity tabs, quick generate grid, bulk controls | Interactive HTML with working tabs |
| 0:35–0:45 | Connect UI to engine: generate button, preview table, output textarea | Click Generate → see data |
| 0:45–0:52 | Add export: Copy All, Download JSON/CSV/SQL | Working file downloads |
| 0:52–0:58 | Polish: copy toasts, dark mode, mobile responsiveness | Feels finished |
| 0:58–1:00 | Test all 4 entities × all 3 formats | Ship it |

---

## 12. Open Questions

1. **External image services:** Use Pravatar/DiceBear for avatars (requires network) or generate colored SVG placeholders (offline)?
2. **International data:** How many locales for MVP? Suggest: US only for 1-hour build, add UK/DE in v2.
3. **SQL dialect:** Which SQL dialect for INSERT statements? Suggest: PostgreSQL-compatible (standard SQL) for MVP.
4. **Large datasets:** Should 1000+ rows show a progress bar or just freeze UI briefly? Suggest: cap at 1000 for MVP, add virtual scrolling later.
5. **Custom schemas:** Is drag-and-drop schema builder worth a 2-hour build, or save for v2? Suggest: v2.

---

## 13. Appendix

### 13.1 Glossary

| Term | Definition |
|------|-----------|
| **Seed** | An integer input that makes random generation deterministic (same seed = same output) |
| **LCG** | Linear Congruential Generator — a simple algorithm for pseudo-random numbers |
| **Entity** | A type of data object (User, Company, Product, Order) |
| **Locale** | A region/language setting that affects data format (names, addresses, phone numbers) |
| **SKU** | Stock Keeping Unit — a unique product identifier |
| **BS Statement** | A satirical business-speak phrase ("streamline turn-key platforms") |

### 13.2 Reference Products

- **Mockaroo** — Powerful but account-gated and rate-limited
- **Faker.js** — The library this tool is inspired by; this is a UI wrapper + extra features
- **JSON Generator** — JSON-focused, no CSV/SQL export
- **GenerateData.com** — Dated UI, limited customization

### 13.3 Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-06-07 | Product Team | Initial PRD |

---

**End of Document**
