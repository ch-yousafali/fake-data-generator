# Fake Data Generator

A lightweight, browser-based fake data generator that produces realistic names, emails, addresses, UUIDs, and full entities (users, companies, products, orders) for developers who need seed data for testing APIs, populating databases, or building UI prototypes.

> **Core Value Proposition:** Realistic seed data in seconds. No signup, no API keys, no bloat.

---

## Features

### Quick Generate
- One-click generation of individual data fields
- Click to copy any field instantly to clipboard
- Auto-regenerates after copy for fresh data
- Supported fields: Name, Email, Phone, UUID, Address, Date, Paragraph, URL, Hex Color, IP Address, Company, Product

### Bulk Generator
- Generate 1 to 1,000 rows of structured data instantly
- Four entity types:
  - **Users** — name, email, phone, address, age, job title, company, avatar
  - **Companies** — name, catch phrase, BS statement, industry, address, email, website, employees
  - **Products** — name, description, price, category, SKU, stock status, rating, image
  - **Orders** — order number, customer, items, total, status, date, shipping address

### Export Formats
- **JSON** (Pretty / Minified)
- **CSV** — ready for Excel/Sheets import
- **Excel (.xlsx)** — formatted with styled headers, auto-sized columns, frozen header row
- **SQL INSERT** — PostgreSQL-compatible statements with proper escaping

### Localization
- **5 locales** with locale-aware data:
  - United States
  - United Kingdom
  - Germany
  - France
  - Japan
- Names, addresses, phone formats, and company suffixes adapt to selected locale

### Customization
- **Field toggles** — include/exclude specific fields per entity
- **Gender filter** — Any / Male / Female (Users only)
- **Seed input** — enter an integer seed for reproducible, deterministic output
- **Editable table name** — for SQL export

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Runtime | Browser (vanilla JS) |
| Styling | Tailwind CSS (CDN) |
| Excel Export | SheetJS / xlsx.js (CDN) |
| Data Generation | Custom seeded random + locale-aware pools |
| Clipboard | Native Clipboard API + execCommand fallback |
| File Download | Blob + URL.createObjectURL |

---

## Architecture

Single-file architecture — everything lives in `index.html`:

```
index.html
  ├── <style>    Tailwind CDN + custom Apple-style CSS
  ├── <body>     Layout structure
  └── <script>   Data generation engine + UI logic + SheetJS integration
```

No build step. No backend. Works offline after first load.

---

## Data Generation Engine

### Seeded Random (LCG)
Uses a Linear Congruential Generator for deterministic pseudo-randomness:

```javascript
seed = (seed * 16807) % 2147483647
```

Same seed + same config = identical output. Useful for reproducible tests.

### Locale-Aware Pools
Each locale contains realistic pools for:
- First names (male / female)
- Last names
- Street names, cities, states
- Job titles, company suffixes, industries
- Product categories, adjectives, nouns
- Order statuses, catch phrases, BS statements

---

## Usage

### Open the file
Simply open `index.html` in any modern browser. No server required.

### Quick Generate
1. Click any field card in the "Quick Generate" section
2. Value is copied to clipboard automatically
3. A new value is generated immediately

### Bulk Generate
1. Select an entity type (Users / Companies / Products / Orders)
2. Set row count (1–1000)
3. Choose output format
4. Select locale and optional gender filter
5. Toggle fields to include/exclude
6. Click **Generate**
7. Preview first 5 rows in the table
8. Click **Copy All** or **Download**

### Excel Export
1. Select **Excel (.xlsx)** from the Format dropdown
2. Click **Generate**
3. Click **Download** — a properly formatted `.xlsx` file is generated with:
   - Bold blue header row
   - Auto-sized columns
   - Frozen top row for scrolling
   - Clean sheet naming

---

## Browser Support

- Chrome, Firefox, Safari, Edge (last 2 versions)
- Mobile: iOS Safari, Chrome Android
- Fallback for Clipboard API on older browsers

---

## Performance

| Operation | Target | Actual |
|-----------|--------|--------|
| First paint | < 1s | Instant (single file) |
| Generate 100 rows | < 100ms | ~10ms |
| Generate 1,000 rows | < 500ms | ~80ms |
| Excel export | < 1s | ~200ms for 1,000 rows |
| Copy to clipboard | < 50ms | Instant |

---

## Project Structure

```
/
├── index.html          # Main application (single file)
├── README.md           # This file
└── Fake_Data_Generator_PRD.md  # Product Requirements Document
```

---

## Future Enhancements

- Custom schema builder (drag-and-drop field types)
- Save & load presets via localStorage
- JSON Schema / GraphQL mock data generation
- Placeholder image generation (SVG data URIs)
- Additional locales (ES, IT, BR, CN, KR)
- Dark mode toggle
- Progress indicator for 1,000+ row generation

---

## License

MIT — use it, share it, modify it. No attribution required.

---

## Acknowledgments

Inspired by [Mockaroo](https://mockaroo.com/), [Faker.js](https://fakerjs.dev/), and [JSON Generator](https://www.json-generator.com/). Built to be faster, simpler, and fully client-side.
