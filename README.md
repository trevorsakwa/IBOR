# Investment Book of Records — Reconciliation Framework

A complete, regulation-mapped reconciliation framework for asset management firms, built to demonstrate how an Investment Book of Records (IBOR) should reconcile against custodian statements, market data vendors, and corporate action notifications under FCA rules.

## Background

The FCA requires asset managers to maintain orderly records of all services and transactions (SYSC 9.1), reconcile custody holdings against their custodian on a regular basis (CASS 6.6), and value fund assets fairly and independently (FUND 3.9). In practice, this means running daily checks across three data sources — the firm's own book of records, the custodian's statement, and independent market data vendors — and documenting every discrepancy, investigation, and resolution.

This project provides a working example of that framework in a single Excel workbook.

## Workbook Structure

The workbook contains seven sheets, each serving a distinct role in the reconciliation lifecycle.

### 1. IBOR — Book of Records

The master "golden source" for the fund's positions. Each row represents a holding identified by ISIN, with the following fields:

- **Security identification** — ISIN and name, the common key that links the firm's record to the custodian's record and to market data feeds.
- **Asset class** — Internal classification (UK Equity, Fixed Income, ETF, Cash) used for mandate compliance, risk management, and regulatory reporting.
- **Currency** — Denomination of each holding, relevant for multi-currency portfolios requiring FX conversion to a base currency.
- **Quantity (units)** — The firm's record of how many units it holds. This must match the custodian's quantity exactly; any difference is a break requiring investigation under CASS 6.6.
- **Average cost price and cost basis** — Internal to the firm. The custodian does not track acquisition cost. Used for unrealised P&L, performance attribution, and tax lot accounting.
- **Market price and price source** — The closing price from the primary market data vendor (Bloomberg), with the source recorded for audit trail purposes under FUND 3.9.
- **Market value** — Quantity multiplied by market price. Drives the NAV calculation, client reporting, and regulatory returns.
- **Unrealised P&L** — Market value minus cost basis. Internal performance measure.
- **Weight (%)** — Each position's proportion of total portfolio value. Used for concentration monitoring and rebalancing.
- **Custodian quantity, quantity variance, custodian market value, value variance** — The reconciliation layer. These columns compare the firm's records against the custodian's daily holdings feed and flag any discrepancies.

### 2. Custodian Reconciliation

Position-by-position comparison of the IBOR against the custodian statement (BNY Mellon in this example). For each holding:

- IBOR quantity vs custodian quantity, with the absolute difference calculated.
- IBOR market value vs custodian market value, with the value difference calculated.
- Automatic status flagging: **MATCHED** if quantity difference is zero and value difference is within ±£500 tolerance; **BREAK** otherwise.
- Break reason, action required, and resolution date fields for documenting the investigation.

The sheet includes two realistic break examples:

- A 500-unit discrepancy on Reckitt Benckiser caused by a pending bonus issue not yet processed by the custodian.
- A £2,500 cash variance caused by an unsettled T+2 trade.

A summary section shows total positions, matched count, break count, and match rate.

### 3. Market Data Reconciliation

Cross-check of closing prices from three independent sources:

- **Bloomberg** (primary vendor) vs **Refinitiv** (secondary vendor) — absolute and percentage differences.
- **Bloomberg** vs **custodian price** — absolute and percentage differences.
- Tolerance threshold of ±0.5%. Status reads **MATCHED** if both comparisons fall within tolerance; **REVIEW** if either exceeds it.
- **Price Used in IBOR** — records which price was actually booked into the valuation, creating an audit trail.
- **Override Reason** and **Approved By** — populated only when the booked price differs from the primary vendor, documenting the justification and sign-off.

A **Stale Price Check** section below the main grid compares today's price to the prior day's price, calculates the percentage change, and counts consecutive days unchanged. Securities with no price movement beyond the firm's threshold are flagged for review by the pricing committee.

### 4. Corporate Actions

Processing and reconciliation log for all corporate action events during the period. Covers six event types:

| Event Type | Example |
|---|---|
| Cash dividend | Shell PLC — GBP 0.287 per share |
| Cash dividend (foreign currency) | Rio Tinto PLC — USD 2.25 per share |
| Bonus issue | Reckitt Benckiser — 1 new share for every 120 held |
| Stock split | Apple Inc — 4:1 split |
| Coupon payment | UK Gilt 3.5% 2028 — semi-annual coupon |
| Rights issue | SAP SE — 1 right per 10 shares at EUR 150 |

For each event, the sheet records the announcement date, ex-date, record date, pay date, rate/terms, quantity held at ex-date, the firm's calculated entitlement, the custodian's credited entitlement, any variance, the amount received, and the processing status.

A **Corporate Action Processing Checklist** documents the eight-step lifecycle from notification receipt through to break investigation, with the responsible team and regulatory basis for each step.

### 5. Regulatory Reference

Complete list of 18 FCA regulations applicable to record keeping in asset management, each with:

- The specific rule reference (e.g. SYSC 9.1.1R, CASS 6.6, FUND 3.9)
- The FCA sourcebook it belongs to
- The type of record it governs
- The minimum retention period
- Its specific relevance to the reconciliation process

Key retention periods:

| Record Type | Retention | Source |
|---|---|---|
| General MiFID business records | 5 years | SYSC 9.1.2R |
| Suitability assessments | 5 years | COBS 9.5.2R |
| Pension transfer suitability | Indefinite | COBS 9.5.2R |
| Telephone/electronic communications | 5–7 years | SYSC 10A |
| Complaints | 3 years | DISP 1.9.1R |
| AML/CDD documents | 5 years post-relationship | MLR 2017 Reg. 40 |

### 6. Reconciliation Controls Framework

Twelve governance controls covering the full reconciliation cycle:

- **Daily controls** — custodian holdings recon, cash recon, price verification, stale price review, trade matching, FX verification, transaction reporting recon.
- **Per-event controls** — corporate action entitlement recon, income recon.
- **Periodic controls** — weekly break ageing review, quarterly CASS assurance review.

Each control has an owner, frequency, regulatory basis, and a description of the evidence that must be retained.

### 7. Legend and Instructions

Guide to the workbook's sheet structure, colour coding (green = matched, red = break, yellow = input cell, amber = review), and font conventions (blue = hardcoded input, green = cross-sheet link, red = error/critical break).

## Regulations Covered

| Regulation | Area |
|---|---|
| SYSC 9.1 | General record-keeping obligation |
| SYSC 10A | Telephone and electronic communications recording |
| COBS 9.5 / 9A.4 | Suitability record keeping |
| COBS 10.7 / 10A.7 | Appropriateness record keeping |
| COBS 11.3 | Client order handling |
| COBS 11.5A | Client orders and transactions |
| COBS 4.11 | Financial promotions |
| CASS 6 | Client asset custody and reconciliation |
| CASS 7 | Client money |
| FUND 3.9 | Fair and independent valuation |
| DISP 1.9 | Complaints |
| SUP 15A / 16 | Transaction reporting |
| PRIN 2A | Consumer Duty |
| SUP 10C (SM&CR) | Senior manager accountability |
| MLR 2017 Reg. 40 | Anti-money laundering record retention |
| UK MAR | Market abuse — suspicious transaction reports |

## Tools Used

- Python (openpyxl) for workbook generation
- LibreOffice for formula recalculation and verification
- All formulas are live — the workbook recalculates when inputs change

## How to Use

1. Download `Book_of_Records_Reconciliation.xlsx`.
2. Open in Excel or LibreOffice Calc.
3. The IBOR sheet is the starting point. All other sheets reference it.
4. Yellow-highlighted cells are input fields. Formulas are in black text.
5. Adjust the sample data to reflect your own fund's holdings, custodian, and pricing vendors.
6. Use the Regulatory Reference and Controls Framework sheets to map each reconciliation step to your firm's compliance obligations.

## Disclaimer

This project is an educational example. It does not constitute regulatory advice. Firms should consult their compliance teams and legal advisers to ensure their record-keeping arrangements meet FCA requirements specific to their authorisation, permissions, and business model.

## License

MIT
