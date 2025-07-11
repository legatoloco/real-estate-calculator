# Real Estate Investment Calculation Model Specification

## Required Numerical Inputs

| Input Name                                 | Type/Unit         | Notes                                      |
|---------------------------------------------|-------------------|---------------------------------------------|
| Acquisition date                           | Date              |                                           |
| Construction start date (OS)                | Date              |                                           |
| Construction end date (DAT)                 | Date              |                                           |
| Permit dates                               | Date(s)           | If applicable                             |
| Portage period start/end                    | Date(s)           | If land portage applies                   |
| Monthly commercialization rates             | % per month       |                                           |
| Commercialization threshold dates           | Date(s)           | When 50%, 75% are reached                 |
| Technical cost price (TTC)                  | Currency          |                                           |
| Land price (HT)                            | Currency          |                                           |
| Internal sales fees                        | Currency          |                                           |
| External sales fees                        | Currency          |                                           |
| Notary fees                                | Currency          |                                           |
| Rental marketing fees                      | Currency          |                                           |
| Rental guarantee                           | Currency          |                                           |
| Financial charges (interest, stock, other) | Currency          |                                           |
| Group management fees                      | Currency          | If 100% owned                             |
| Corporate tax rate (IS)                    | %                 |                                           |
| Debt interest rate (monthly)               | %                 |                                           |
| Monthly inflows                            | Currency/month    | Cashflow plan                             |
| Monthly outflows                           | Currency/month    | Cashflow plan                             |
| Equity contributions by partner            | Currency          | Per partner                               |
| Partner share of equity                    | %                 | Per partner                               |
| Partner share of profit                    | %                 | If different from equity share            |
| Partner remuneration rate                  | % or IRR target   | If applicable                             |
| Third-party advance amount                 | Currency          |                                           |
| Third-party advance period                 | Date(s)           |                                           |
| Third-party advance interest rate          | %                 |                                           |
| Normative monthly rate for equity cap      | %                 |                                           |
| Portage rate                               | %                 |                                           |
| Custom bank equity cap rate                | %                 | If overridden                             |
| Manual overrides                           | Various           | For any rates or thresholds               |
| Additional budget items/fees               | Currency          | As needed                                 |
| Custom scenario parameters                 | Various           | For edge cases                            |

## 1. Inputs Required
- **Operation Type:** Residential, Tertiary, Land Portage, etc.
- **Operation Milestones:** Acquisition date, construction start (OS), construction end (DAT), permit dates, etc.
- **Commercialization Rates:** % sold at each period
- **Financial Data:**
  - Technical cost price (TTC), with adjustments (see below)
  - Land price (HT)
  - Budget items (internal/external sales fees, notary fees, rental marketing fees, financial charges, etc.)
  - Debt interest rate (monthly)
  - Corporate tax rate (IS)
  - Cashflow plan (monthly inflows/outflows)
  - Equity contributions (by partner, with possible different shares and remuneration)
  - Third-party advances (amount, period, interest rate)
- **Bank requirements:** Custom equity cap rates if applicable

## 2. Calculation Model

### 2.1. Equity Cap (Plafond des Fonds Propres)
- **General Principle:**
  - The equity cap is the cumulative equity released into the operation's bank account, peaking at land acquisition and dropping to zero by the 3rd month after construction ends (DAT).
- **Formula:**
  - `Equity Cap = Normative Monthly Rate × Adjusted Technical Cost Price (TTC)`
  - `Adjusted Technical Cost Price = Technical Cost Price (TTC) - [internal/external sales fees, notary fees, rental marketing, rental guarantee, financial charges, group management fees (if 100% owned)]`
- **Special Cases:**
  - **Land Portage:**
    - During portage: `Equity Cap = Portage Rate × Land Price (HT)`
    - After permit: revert to normal rate and base.
  - **Bank-specific cap:** Allow override of the normative rate.

### 2.2. Normative Monthly Rate (Taux Mensuel)
- **Depends on:**
  - Operation type (residential, tertiary, etc.)
  - Commercialization progress (thresholds at 50%, 75%)
  - Construction milestones (OS, DAT)
  - Land portage status
- **Example Rules:**
  - **Tertiary (or residential sold in block):**
    - After VEFA signed, until DAT+2 months: 5%
    - After DAT+3 months: 0%
    - If BEFA signed before VEFA: 50% → 30% → 5% → 0% (see table for periods)
  - **Residential:**
    - <50% sold: 10%
    - 50–75% sold: 6.7%
    - ≥75% sold: 3.4%
    - After DAT+3 months: 0%
  - **Land Portage:**
    - 20% of land price (HT) during portage period

### 2.3. Financing Split (Equity vs. Debt)
- All expenses at start are covered by equity.
- Once equity cap is reached, additional expenses are financed by debt.
- Typically, this switch happens at land acquisition.

### 2.4. Repayment Logic
- **Debt must be fully repaid before equity is reimbursed.**
- **Equity is reimbursed only if:**
  - Debt is fully repaid
  - Sufficient cash is available to cover all future needs
- **Marge (profit) is distributed only if:**
  - Debt and equity are fully repaid
  - Sufficient cash is available for all future needs
  - Corporate tax (IS) is paid in the same month as profit distribution

### 2.5. Partner and Third-Party Financing
- Partners may contribute equity in different proportions, possibly with remuneration (fixed interest or IRR target).
- Third parties may advance funds at a fixed monthly rate.

### 2.6. ROI/IRR Calculation
- Use cashflow (actual and/or forecast) to compute IRR before and after financing.
- Ensure consistency between balance sheet and cashflow plan (difference < 100k).

## 3. Outputs
- **Monthly cashflow table:** inflows, outflows, debt, equity, cash position
- **Equity cap evolution over time**
- **Debt drawdown and repayment schedule**
- **Timing of positive/negative cashflow**
- **ROI/IRR before and after financing**
- **Alerts for when to take a loan, when cashflow turns positive/negative**
- **Partner/third-party remuneration details**

## 4. Business Rules & Edge Cases
- Allow manual override of rates and thresholds.
- Support for multiple operation types and mixed scenarios.
- Handle special cases (e.g., land acquired without permit, custom bank requirements). 

# Real Estate Investment Calculation Model Specification

## Required Numerical Inputs

| Input Name                                 | Type/Unit         | Notes                                      |
|---------------------------------------------|-------------------|---------------------------------------------|
| Acquisition date                           | Date              |                                           |
| Construction start date (OS)                | Date              |                                           |
| Construction end date (DAT)                 | Date              |                                           |
| Permit dates                               | Date(s)           | If applicable                             |
| Portage period start/end                    | Date(s)           | If land portage applies                   |
| Monthly commercialization rates             | % per month       |                                           |
| Commercialization threshold dates           | Date(s)           | When 50%, 75% are reached                 |
| Technical cost price (TTC)                  | Currency          |                                           |
| Land price (HT)                            | Currency          |                                           |
| Internal sales fees                        | Currency          |                                           |
| External sales fees                        | Currency          |                                           |
| Notary fees                                | Currency          |                                           |
| Rental marketing fees                      | Currency          |                                           |
| Rental guarantee                           | Currency          |                                           |
| Financial charges (interest, stock, other) | Currency          |                                           |
| Group management fees                      | Currency          | If 100% owned                             |
| Corporate tax rate (IS)                    | %                 |                                           |
| Debt interest rate (monthly)               | %                 |                                           |
| Monthly inflows                            | Currency/month    | Cashflow plan                             |
| Monthly outflows                           | Currency/month    | Cashflow plan                             |
| Equity contributions by partner            | Currency          | Per partner                               |
| Partner share of equity                    | %                 | Per partner                               |
| Partner share of profit                    | %                 | If different from equity share            |
| Partner remuneration rate                  | % or IRR target   | If applicable                             |
| Third-party advance amount                 | Currency          |                                           |
| Third-party advance period                 | Date(s)           |                                           |
| Third-party advance interest rate          | %                 |                                           |
| Normative monthly rate for equity cap      | %                 |                                           |
| Portage rate                               | %                 |                                           |
| Custom bank equity cap rate                | %                 | If overridden                             |
| Manual overrides                           | Various           | For any rates or thresholds               |
| Additional budget items/fees               | Currency          | As needed                                 |
| Custom scenario parameters                 | Various           | For edge cases                            |

