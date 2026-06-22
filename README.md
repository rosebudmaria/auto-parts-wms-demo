# PartTrack WMS — Used Auto Parts Warehouse Management (Interactive Demo)

**Live demo:** https://rosebudmaria.github.io/auto-parts-wms-demo/

An interactive, clickable prototype of a Warehouse Management System built for a
used auto parts business that sells on eBay. It demonstrates the **workflows,
screens, and data model** proposed for the full build described in the project
brief — barcode put-away, scan-to-locate, complete movement history, inventory
aging, eBay order pick lists, reporting, and more.

> This is a **front-end prototype**. All data is realistic *sample* data generated
> in the browser. Put-away, move, pick, status-change and retrieval-request
> actions are real and persist locally (via `localStorage`) so you can drive the
> full workflow — but there is no live eBay connection or server database. Use
> **Settings → Reset demo** (or the link on the dashboard) to restore the sample set.

---

## Try these in the demo

| Workflow | How |
|---|---|
| **Scan to Locate** | Sidebar → *Scan to Locate* → tap a sample barcode (e.g. `210200137`). Returns the part photo, full warehouse location, status, days-in-inventory, eBay item ID/SKU, and the **complete movement history**. Try scanning a **box** barcode (`BX-1187`) to see its contents. |
| **Receive / Put-away** | *Receive* → tap an "awaiting put-away" part → tap a box. Records part, box, full location, timestamp and employee. |
| **Move Inventory** | *Move* → scan a part → scan a destination box. Adds a `Moved` row to the audit trail. |
| **eBay Orders / Pick List** | *eBay Orders* → see imported sold orders with the exact pick location, then **Pick → Ship**. |
| **Reports & CSV** | *Reports* → aging, movement, sales/fulfillment, per-employee activity. Every report exports to CSV. |
| **Multi-user attribution** | Top-right user menu → *Act as* another employee, then perform an action and watch the attribution change in the history. |

A barcode scanner (Zebra, etc.) behaves as a keyboard that types the code and
presses **Enter** — exactly how the scan fields in this demo work, so the same
screens run unchanged on a handheld scanner, phone, or tablet.

---

## How the demo maps to the project requirements

| Requirement (from the brief) | Where it's shown |
|---|---|
| Barcode scanning workflow (works with existing scanners/labels) | Scan-to-Locate, Receive, Move — Enter-to-submit scan fields |
| Inventory placement: *scan part → scan box → place* | **Receive / Put-away** |
| Records part, box, location, date/time, employee | Every action writes a history row |
| Fast lookup by barcode → location + status + history | **Scan to Locate** |
| Box / building / row / section / shelf / bin tracking | Location model + part detail |
| Complete movement history (all previous + current locations) | Part detail timeline |
| Days in inventory / inventory aging reports | Dashboard + Reports |
| Inventory statuses (Available, Sold, Reserved, Scrapped, Not Listed) | Status badges + status control |
| Receive / place / move / pick / ship / discard + full audit trail | Warehouse Ops screens |
| eBay: import sold orders, show pick location, prevent overselling | **eBay Orders / Pick List** |
| Photo per part, synced from the eBay listing | Part detail (placeholder image in demo) |
| Multi-user access + employee activity tracking | Users & Activity, user switcher |
| Reporting + dashboard with key metrics | Dashboard + Reports (with CSV export) |
| Daily backups, retention, restore, failure alerts | **Settings → Backups** |
| Retrieval requests (optional feature) | **Retrieval Requests** |
| Donor-vehicle / VIN tracking (nice-to-have) | **Donor Vehicles** |
| Mobile-friendly (Zebra / Android / iPhone / iPad) | Responsive layout (resize the window) |

---

## Proposed production stack (not part of this static demo)

Chosen for longevity, easy hiring, clear documentation and **no vendor lock-in** —
all standard, widely used, open technologies. Full source ownership transfers to
the client.

- **Front end:** React + TypeScript, installable **PWA** so the same app runs on
  desktop, Android and iOS/iPad and pairs with any HID barcode scanner.
- **Back end:** Node.js (NestJS) + TypeScript REST API, documented with
  **OpenAPI/Swagger**. (Python/Django is an equally valid alternative.)
- **Database:** **PostgreSQL** — relational, well-suited to an append-only
  movement/audit history with a clear, documented schema.
- **Photos & files:** object storage (e.g. S3-compatible), one primary image per
  part, synced from the eBay listing.
- **eBay integration:** eBay **Sell APIs** (Inventory, Fulfillment, Browse) over
  OAuth — scheduled order import, quantity sync to prevent overselling, and
  listing/photo import.
- **Backups & data protection:** automated nightly database dumps to **local
  NAS + offsite (S3)**, 30+ day retention, manual backup + restore tooling, and
  alerting if a scheduled backup fails.
- **Auth & access:** role-based access control (Admin / Office / Warehouse);
  every create/move/pick/status change is stamped with the signed-in employee.
- **Hosting:** cloud-first and containerized (Docker) for easy scaling and
  migration. An **open REST API + webhooks** keeps future integrations simple.

### Indicative data model

`vehicles` · `parts` · `boxes` · `locations` · `movements` (audit trail) ·
`ebay_orders` · `users` · `retrieval_requests` · `backups` / `audit_log`.

---

## Run it locally

It's a single static file — no build step.

```bash
# option 1: just open it
open index.html

# option 2: serve it (recommended)
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Project structure

```
auto-parts-wms-demo/
├── index.html   # the entire app: HTML + CSS + JS + sample data
└── README.md
```

---

*Prototype prepared as a work sample for the Warehouse Management System project.
Timeline, pricing and support options are provided separately in the proposal.*
