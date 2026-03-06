**Audience:** Business Analysts, Developers, QA.
**Purpose:** Onboarding and understanding core system logic.

---
## Purpose of the System

The system supports multi-level B2B and B2C distribution of travel products with strict separation of commercial data between levels.

---
## Glossary

- **Product** — a travel service available for sale (for example, accommodation)
- **Booking** — a reservation created for a product
- **Direct upstream partner** — a participant in the sales chain who directly sells the product to the current user and from whom the current user purchases the product
- **Seller** — a participant who resells a product to a direct downstream partner and creates bookings
- **Base price** — the price set by the supplier for their own product
- **Selling price** — the price defined by a seller when reselling a product to their direct client
- **Final Selling Price** — the total price paid by the end customer (Tourist or Corporate). While it includes all upstream margins and commissions, it is displayed as a **single gross amount** without disclosing the individual markup components
- **Upstream price** — the price defined by the direct upstream partner
- **Contract price** — a price applied to a Сorporate Сlient according to corporate agreements and policies
- **Commission** — a portion of the selling price allocated to an TA or partner
- **Margin** — the difference between the upstream price and the selling price
- **Resale** — selling a product purchased from a direct upstream partner
- **Seller** — any participant in the system (TO or TA) who resells the product to the next participant (Downstream Partner) or end consumer. Each Seller in the chain is responsible for setting their own mark-up (Margin)

---

## Core System

| **System** | **Subsystem** | **Description**                        | **Key Functions**                                                                                               |
| ---------- | ------------- | -------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Buy**    | Inventory     | Internal product management            | Manual creation and management of hotels, transfers, excursions; descriptions, images, prices, and availability |
| **Buy**    | Hub           | External supplier integrations         | Automatic import of products, prices, and availability via API                                                  |
| **Sell**   | B2B Module    | Sales to agencies and corporate client | Partner management, B2B pricing, booking flow                                                                   |
| **Sell**   | B2C Module    | Direct sales to tourists               | Online booking via website or API, customer accounts                                                            |
| **Manage** | Enterprise    | Business and booking management        | Booking lifecycle management, policies, and reporting                                                           |
| **Manage** | Gateways      | External system integrations           | Payments, CRM, accounting, third-party systems                                                                  |

---

## System Actors and  Roles

| **Role**                                | **Definition**                                                          | **Key Functions**                                                                                                    |
| --------------------------------------- | ----------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Supplier**                            | The primary source of inventory (Hotels, Transfers, etc.)               | • Provides product data, availability, and base prices<br>• Confirms or rejects booking requests manually or via API |
| **Tour Operator (TO)**                  | A central intermediary responsible for product distribution and pricing | • Purchases inventory from Suppliers or Upstream TOs<br>• Manages margins and resells to Downstream TOs or Agencies  |
| **Tour Agency (TA)**                    | A retail partner and the final intermediary in the B2B chain            | • Purchases products from the Last TO in the chain<br>• Sells products to end consumers (Tourists or Corporates)     |
| **Tourist** <br>(End customer)          | An individual end consumer (B2C)                                        | • Searches, books, and pays for travel products via the platform<br>• Receives vouchers with final selling prices    |
| **Corporate Client** <br>(End customer) | A business entity purchasing services for employees (B2B)               | • Books products according to corporate policies<br>• Utilizes predefined contract prices and agreements             |

---

## Distribution Workflow (Step-by-Step)

**STEP 1: INITIALIZATION**
    - Supplier provides Product (Inventory + Base Price).
    - Product is assigned to TO Level 1 (TO L1).

**STEP 2: TO-TO SEQUENTIAL TRANSFER**
    - IF TO LN exists:
        - Current TO sells to Next TO.
        - Pricing: Upstream Price + Current TO Margin = New Selling Price.
        - Repeat STEP 2 for TO L2, L3, ..., LN.
    - ELSE (No further TO levels):
        - Proceed to STEP 3.

**STEP 3: TA HANDOVER**
    - The Last TO (TO LN) sells the product to the Tour Agency.
    - Note: This is the ONLY point in the chain where a TO can sell to an Tour Agency.

**STEP 4: FINAL SALE**
    - The Tour Agency acts as the final reseller.
    - IF Corporate Client is a Tourist:
        - Apply retail margin/commission.
        - Generate B2C Booking.
    - ELSE IF End Customer is a Corporate Client:
        - Apply Corporate Agreement/Policy prices.
        - Generate B2B Booking.


### Distribution Chain
```
graph LR
    S[Supplier] -- "Base Price" --> TO1[TO Level 1]
    
    subgraph Sellers_Chain [Recursive Seller Roles]
        TO1 -- "Price + Margin" --> TO2[TO Level 2]
        TO2 -- "Price + Margin" --> TON[TO Level N]
        TON -- "Price + Margin" --> TA[Tour Agency]
    end

    TA -- "Final Selling Price" --> Customer((End Customer))

    style Sellers_Chain fill:#f9f9f9,stroke:#333,stroke-dasharray: 5 5
```

### Error Handling

| **Scenario**                           | **System Action**                                         |
| -------------------------------------- | --------------------------------------------------------- |
| **TO L1 tries to sell directly to TA** | Allowed ONLY if TO L1 is the _only_ operator in the chain |
| **TA tries to sell to another TA**     | **REJECTED.** (Not supported by current business logic)   |
| **TO L2 tries to sell to TO L1**       | **REJECTED.** (Reverse flow is prohibited)`               |

---

## Pricing Principles

| **Role**                          | **Defines**                                                       | **See**                                                              | **Do NOT see**                                                                           |
| --------------------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Supplier**                      | Base price for own products                                       | Own base price and availability                                      | Prices, selling prices, margins, or commissions of any downstream partners               |
| **Tour Operator (TO)**            | Selling price for products they resell (to TA, or other TO)       | Direct upstream price (from supplier) and their own selling price    | Prices or margins defined by partners other than their direct upstream partner           |
| **Tour Operator Level N (TO LN)** | Selling price for products they resell (to agencies, or other TO) | Direct upstream price (from their upstream TO) and own selling price | Prices or margins defined by partners other than their direct upstream partner           |
| **Tour Agency (TA)**              | —                                                                 | Direct upstream price (from their selling TO) and own commission     | Prices, margins, or base prices of any partners other than their direct upstream partner |
| **Tourist**                       | —                                                                 | Final selling price                                                  | Any upstream prices, margins, or commissions                                             |
| **Corporate Client**              | —                                                                 | Contract price or final selling price according to corporate policy  | Any upstream prices, margins, or commissions                                             |

---

## Booking User Flow

1.  Product exists with price and availability
2. A booking is created by the seller (TO or TA) for their direct client, which may be another TO, an TA, an Corporate Client, or a Tourist
3. The direct upstream partner confirms or rejects the booking
4. Booking status is updated for all relevant roles
5. Voucher is generated
6. The customer receives a voucher

---

## Vouchers

- Vouchers are generated after booking confirmation
	They display:
	• seller information
	• final selling price
- They do not expose upstream prices, margins, or commissions

---

## Scope Notes

- Business rules and policies are created by TO
- The overview intentionally excludes implementation, API, or organizational details
