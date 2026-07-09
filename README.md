# Promo Decision Tool — Marco's Pizza Store #8606

Interactive single-page HTML/JS tools for evaluating promotional strategies at **Marco's Pizza Store #8606 (Dundee, FL)**. Both tools run entirely in the browser — no server required.

---

## Files

### `promo_decision.html` — Weekly Promo Decision Flow

The primary tool for selecting and analyzing weekly promotions against a baseline week of transaction data (May 7–13, 2026).

**Baseline data:**
- 931 line items across 377 unique transactions
- Base revenue: $11,349 | Base net: $1,702 | Base margin: 15.0%

**Available promotions:**

| ID | Name | Description |
|----|------|-------------|
| P1 | XL NY Pizza + 2 Drinks | Bundle: XL New Yorker pizza + up to 2 fountain drinks at a combined discount |
| P2 | L Pizza + CheezyBread | Bundle: Large pizza + CheezyBread combo at $17.99 |
| P3 | Chocolate Delight Trio | Combo of 3 dessert items at a fixed price |
| P4 | Choose Your Side $4.99 | Any qualifying side item capped at $4.99 |
| P6 | 3 Dip Cups $1.49 | Three dip cups for $1.49 |

**Key logic:**

- **Promo assignment**: Each baseline transaction row is evaluated against all selected promos; the best (lowest customer price) is assigned.
- **Promo cost**: `max(0, menuPrice − promoPrice) × qty` — always uses menu price as baseline, never the customer's pre-existing discount price.
- **Bundle secondary qty cap**: For bundle promos (e.g., P1 drinks), the discount applies only to `min(qty, secondaryQty)` units, not the full cart quantity.
- **Deduplication**: Items in `dedupeGroup: "sides"` are only discounted by one promo to prevent double-discounting.
- **P2 upsell injection**: For transactions containing only S/M pizzas (no L pizza anchor), the tool injects a Pepperoni Magnifico (L) at $13.00 + CheezyBread at $4.99 as an upsell opportunity.
- **P4 upsell injection**: For 40% of transactions with no qualifying side item, a CheezyBread upsell at $4.99 is injected. Transactions without any other active promo are prioritized.

**Profit formula:**
- Existing rows: `menuPrice × qty × 0.15 − promoCost` (revenue already in BASE_REV)
- New/upsell rows: `promoPrice × 0.15 − promoCost` (incremental revenue on top of BASE_REV)
- Dashboard: `(BASE_REV + incrRev) × 0.15 − totalWeekDiscount`

**Dashboard metrics shown:**
- Incremental Revenue, Promo Cost (Week Discount), Projected Daily Net Profit, Projected Daily Margin

**CSV export**: Clicking "Export CSV" downloads a full transaction-level breakdown including:
- All baseline rows with their assigned promo (or no promo)
- Injected upsell rows labeled `Existing (Upsell)`
- A RECONCILIATION row bridging row-level sums to exact dashboard values
- A DASHBOARD TOTALS section at the bottom with exact metric values

---

### `promo_events_decision.html` — Event-Based Promo Decision Flow

A variant of the promo decision tool tailored for event-driven traffic patterns. Uses the same store (#8606) and promo framework but layers in event context (local events, holidays, seasonal patterns) to adjust projected transaction volumes and promo effectiveness.

---

## Usage

1. Open either HTML file directly in a browser (Chrome/Edge/Firefox recommended).
2. Select one or more promos using the toggle cards.
3. Review the dashboard metrics — incremental revenue, promo cost, and projected profit/margin update live.
4. Click **Export CSV** to download the full transaction-level detail for analysis.

No installation, build step, or internet connection required.

---

## Store Context

- **Store**: Marco's Pizza #8606
- **Location**: Dundee, FL
- **Baseline week**: May 7–13, 2026
- **Tool purpose**: Internal promo planning — evaluates which combination of promotions maximizes net profit while accounting for discount costs and upsell opportunities.
