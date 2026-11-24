# Boutique Order-to-Billing (POC)

A single-file web app for a tailoring boutique. It covers the whole flow from taking a client's measurements to sending the invoice: measurements, style preferences, pieces and materials, worker assignment, delivery dates based on worker bandwidth, production tracking, bottleneck alerts and invoicing.

No install, no server, no build step. Open `index.html` in Chrome or Edge.

## Run it

```bash
git clone https://github.com/Pawarvb10/boutique-order-to-billing.git
cd boutique-order-to-billing
# then double-click index.html, or:
python3 -m http.server 8000   # and open http://localhost:8000
```

Demo data (customers, orders, workers, designs) loads on first launch. **Setup → Reset to demo data** brings it back at any time.

## Modules

| Module | What it does |
|---|---|
| **Orders** | Measurements per dress type, a required client-liking / style note, pieces and sub-components with unit price, and a work plan (stage, piece, worker, pricing). Cost and delivery date update as you type. |
| **Scheduling** | Places every open task in its worker's queue using hours per day, weekly offs and holidays. Tasks within a stage run in parallel and stages run in sequence. Orders are queued by priority and then promised date. |
| **Costing** | Pricing per piece, per hour × complexity factor (for embroidery and hand work), fixed, or external vendor charge. Shows materials + labour + external = input cost, then markup, override, GST and margin. |
| **Production board** | Kanban by stage, Start/Done buttons, planned-vs-actual timeline, printable job cards. |
| **Workers & load** | Queue hours and days per worker, next free date, 12-day capacity heatmap. |
| **Alerts** | Flags late or at-risk orders, overrunning tasks, overdue vendor work, unassigned tasks, hand-off delays, overloaded workers and stage bottlenecks. Sends alerts to the admin via WhatsApp or email. |
| **Configurator** | Dress types (measurement fields, default pieces, default work plan) and a design catalogue: stencils, sample photos, 3D renders / `.glb` model, a drag-to-rotate 360° view, and add-on materials and work that get added to an order's plan. |
| **Communication** | Editable SMS / WhatsApp presets with fields like `{customer_first}`, `{order_no}`, `{balance}`, `{upi_id}`. Offers to send automatically on order saved / ready / delivered. Keeps a sent log. |
| **Invoices** | GST invoice (CGST/SGST or IGST) with logo, GSTIN, UPI and bank details, optional per-piece breakdown, amount in words, payments and balance. Print or save as PDF. |
| **Shop profile** | Logo, address, mobile, website, GSTIN, PAN, UPI ID, bank details, tax settings. |

## Data & limits

- Data is stored in the browser: `localStorage` for records, IndexedDB for images and 3D files. Each browser has its own copy.
- Back up with **Setup → Export data + images** and restore with **Import**.
- WhatsApp / SMS open on the device with the message filled in; a person presses send. Sending without that step would need an SMS or WhatsApp Business API provider.
- The 3D model viewer (`<model-viewer>`) loads from a CDN, so it needs an internet connection.
- Single user, no login. Confirm the GST rate and SAC/HSN code with your CA before using invoices for real.

## Next steps toward production

- Backend + database (e.g. Node/Python + Postgres) so staff share one set of orders
- Login and roles: admin, front desk, workers
- SMS / WhatsApp Business API for messages sent without manual tapping
- Worker mobile view to start/finish tasks from the shop floor
