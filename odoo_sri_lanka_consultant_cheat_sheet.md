# Odoo 19 Functional Setup Cheat Sheet for Sri Lanka

_Last updated: May 5, 2026 (UTC)_

This version is tailored for **Odoo 19** and written as a consultant runbook you can execute with client teams.

## 1) Global company setup (do this first)

### 1.1 Create company record
Path: **Settings → Companies → Create**

Set:
- Legal Name
- Country: **Sri Lanka**
- Timezone: **Asia/Colombo**
- Currency: **LKR** (keep USD/EUR active for import/export)
- Company email, phone, address, VAT/TIN fields

### 1.2 Activate required apps
- Accounting
- Inventory
- Purchase
- Sales
- Manufacturing
- Quality
- Documents (recommended for trade compliance docs)
- Approvals (recommended for PO/MO governance)

### 1.3 Fiscal and accounting baseline
Path: **Accounting → Configuration**

1. Attempt to install Sri Lanka localization if available in your environment.
2. If unavailable, configure manually:
   - Chart of Accounts
   - Sales/Purchase taxes
   - Fiscal positions (Domestic vs Export)
   - Tax grids and report mappings
3. Configure:
   - Fiscal year
   - Lock dates
   - Multi-currency
   - Exchange gain/loss accounts
   - Bank journals (LKR and foreign currency)

---

## 2) Industry A: Import + Distribution (advanced inventory with expiry)

## 2.1 Feature activation
Path: **Inventory → Configuration → Settings**

Enable:
- Storage Locations
- Multi-Step Routes
- Lots & Serial Numbers
- Expiration Dates
- Barcode (if warehouse scanners are used)

## 2.2 Product master template (mandatory fields)
For every expiry-controlled product:
- Product Type = Storable
- Tracking = By Lots
- Purchase UoM and Sales UoM defined
- Expiry parameters:
  - Expiration Time
  - Best Before Time
  - Removal Time
  - Alert Time

## 2.3 Warehouse design
Path: **Inventory → Configuration → Locations**

Create minimum locations:
- WH/IN
- WH/QA
- WH/STOCK
- WH/COLD (if required)
- WH/EXPIRED
- WH/RETURN

Set **FEFO** removal strategy on saleable locations handling perishable stock.

## 2.4 Import inbound process
1. Create PO in supplier currency.
2. Receive goods and assign lot numbers.
3. Record manufacturing/expiry dates per lot.
4. Move to QA/Quarantine if quality control is required.
5. Release approved stock to saleable location.

## 2.5 Distribution outbound process
1. Confirm Sales Order.
2. Validate delivery picking with lot control.
3. Ensure FEFO allocation is applied.
4. Block expired lots (via location and/or quality rule).
5. Print lot traceability on delivery docs if customer requires.

## 2.6 Control reports (weekly/monthly)
- Lots nearing expiry (30/60/90 days)
- Expired lots on hand
- Traceability report by lot
- Slow/near-obsolete stock by warehouse

---

## 3) Industry B: Manufacturing setup

## 3.1 Foundation
Path: **Manufacturing → Configuration**

Set up:
- Work Centers
- Operations
- Bills of Materials (BoM)
- Routings (if multi-step production)
- Quality control points

## 3.2 Product + BoM policy
- Finished Goods: route = Manufacture
- Raw Materials: reordering rules or MTO by policy
- BoM governance:
  - Revision control owner
  - Effective dates for changes
  - Component substitution approval rule

## 3.3 Execution model
- Use Manufacturing Orders (MO) with lot tracking for RM and FG.
- Capture scrap and by-products where relevant.
- Use work orders for operation-level time capture.
- Add in-process and final quality checks.

## 3.4 Costing and valuation
Define one costing policy by product family:
- FIFO or AVCO (most common)
- Standard (only if strong cost governance exists)

Also configure:
- Landed costs for imported materials
- MO variance review (planned vs actual)

---

## 4) Industry C: Export operations

## 4.1 Commercial configuration
- Export pricelists in USD/EUR/etc.
- Incoterms on Sales Orders/Invoices
- Payment terms by region/customer class

## 4.2 Tax and fiscal positions
Create fiscal positions for export customers:
- Map local sales tax to zero-rated/export tax logic as legally applicable.
- Ensure tax behavior is tested from SO → Invoice → Journal Entry.

## 4.3 Multi-currency flow
- Customer invoices in foreign currency
- Customer receipts in foreign currency bank journal
- Auto-post exchange differences to configured accounts

## 4.4 Export document control
Track per shipment:
- Commercial invoice
- Packing list
- BL/AWB
- COO
- Inspection/other statutory docs

Use Documents app folders/tags for auditability.

---

## 5) Go-live implementation sequence (consultant plan)
1. Discovery workshops (all 3 industries)
2. Master data templates sign-off
3. Sandbox configuration
4. Cycle tests:
   - Import-to-stock with expiry
   - Plan-to-produce
   - Order-to-export-cash
5. UAT and issue log closure
6. Cutover checklist + opening balances + opening stock by lot
7. Go-live + 2 to 4 weeks hypercare

---

## 6) Sri Lanka-specific compliance controls
- Validate VAT/tax setup with local accountant before posting to production.
- Confirm statutory report format expectations before month-end.
- Lock accounting periods after review.
- Keep documented approval matrix for purchases, credit notes, and inventory adjustments.

---

## 7) Day-1 consultant checklist
- [ ] Company profile and timezone are correct.
- [ ] LKR base currency and FCY currencies are active.
- [ ] Localization approach is confirmed (package/manual).
- [ ] Domestic vs Export fiscal positions tested.
- [ ] Lots + Expiry + FEFO tested end-to-end.
- [ ] BoM + MO + QC flow tested end-to-end.
- [ ] Export invoicing and FX realization tested.
- [ ] Key users trained and SOPs signed off.
