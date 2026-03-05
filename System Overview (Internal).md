**Audience:** Business Analysts, Developers, QA.
**Purpose:** Onboarding and understanding core system logic.

---
## Purpose of the System

The system supports multi-level B2B and B2C distribution of travel products with strict separation of commercial data between levels.

---

## Core System

| **System** | **Subsystem** | **Description**                         | **Key Functions**                                                                                               |
| ---------- | ------------- | --------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Buy**    | Inventory     | Internal product management             | Manual creation and management of hotels, transfers, excursions; descriptions, images, prices, and availability |
| **Buy**    | Hub           | External supplier connections           | Automatic import of products, prices, and availability via API                                                  |
| **Sell**   | B2B Module    | Sales to agencies and corporate clients | Partner management, B2B pricing, booking flow                                                                   |
| **Sell**   | B2C Module    | Direct sales to tourists                | Online booking via website or API, customer accounts                                                            |
| **Manage** | Enterprise    | Business and booking management         | Booking lifecycle management, policies, and reporting                                                           |
| **Manage** | Gateways      | External system integrations            | Payments, CRM, accounting, third-party systems                                                                  |

---

## Distribution Model

The system supports a multi-level sales chain:
**Supplier → Tour Operator (Level 1) → Tour Operator (Level 2) → Agency → Tourist or Corporate Client**.

Explanation:
- Each participant buys products from a direct upstream partner and may resell them to their own clients
- The chain may include multiple tour operator levels
- Agencies are usually the last reseller before the end customer

---

## Key Concepts

- **Product** — a travel service available for sale (for example, accommodation)
- **Booking** — a reservation created for a product
- **Direct upstream partner** — a participant in the sales chain who directly sells the product to the current user and from whom the current user purchases the product
- **Seller** — a participant who resells a product to a direct downstream partner and creates bookings.

---

## Roles in the Sales Chain

### Supplier
Suppliers provide products, availability, and base prices.
They are the original source of inventory and confirm or reject bookings for their products.

### Tour Operator
A tour operator is the central participant in the system.  
Tour operators purchase products from suppliers or other tour operators and resell them to agencies, other tour operators, or directly to end customers.

### Tour Agency
A tour agency acts as a sales partner of a tour operator.  
Agencies sell products to end customers using the prices and conditions provided by the tour operator.

### Tourist
A tourist is the end consumer of travel products.  
Tourists search for, book, and purchase products through the platform.

### Corporate Client
A corporate client is a company that purchases travel services for its employees.  
Corporate clients book products according to predefined corporate agreements and policies.

---

## Pricing Principles

| **Role**                       | **Price they define**                                                                | **Price they see**                                                              | **Price they do NOT see**                                                                |
| ------------------------------ | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Supplier**                   | Base price for own products                                                          | Own base price and availability                                                 | Prices, selling prices, margins, or commissions of any downstream partners               |
| **Tour Operator**              | Selling price for products they resell (to agencies, other tour operators, tourists) | Direct upstream price (from supplier) and their own selling price               | Prices or margins defined by partners other than their direct upstream partner           |
| **Tour Operator (next level)** | Selling price for products they resell (to agencies, other tour operators, tourists) | Direct upstream price (from their upstream tour operator) and own selling price | Prices or margins defined by partners other than their direct upstream partner           |
| **Agency**                     | —                                                                                    | Direct upstream price (from their selling tour operator) and own commission     | Prices, margins, or base prices of any partners other than their direct upstream partner |
| **Tourist**                    | —                                                                                    | Final selling price                                                             | Any upstream prices, margins, or commissions                                             |
| **Corporate Client**           | —                                                                                    | Contract price or final selling price according to corporate policy             | Any upstream prices, margins, or commissions                                             |

### Glossary
- **Base price** — the price set by the supplier for their own product
- **Selling price** — the price defined by a seller when reselling a product to their direct client
- **Final selling price** — the price defined by the last seller in the chain and shown to the tourist or corporate client, without margins or commissions
- **Upstream price** — the price defined by the direct upstream partner
- **Contract price** — a price applied to corporate clients according to corporate agreements and policies
- **Commission** — a portion of the selling price allocated to an agency or partner
- **Margin** — the difference between the upstream price and the selling price
- **Sale** — selling a product to an end customer
- **Resale** — selling a product purchased from a direct upstream partner

### Multi-Level Chain Support
- Unlimited number of tour operator levels is supported
- Each level:
    - Buys from the direct upstream parner
    - Sells to one or more downstream partners
- Pricing visibility is enforced at every level

---

## Booking Flow
1.  Product exists with price and availability
2. A booking is created by the seller (tour operator or agency) for their direct client, which may be another operator, an agency, or an end customer
3. The direct upstream partner confirms or rejects the booking
4. Booking status is updated for all relevant roles
5. Voucher is generated
6. The customer receives a voucher
7. The voucher shows only the seller information and the final selling price, without revealing upstream prices or margins

---

## Vouchers

- Vouchers are generated after booking confirmation
	They display:
	• seller information
	• final selling price
- They do not expose upstream prices, margins, or commissions

---

## Scope Notes

- Business rules and policies are created by Tour Operators
- The overview intentionally excludes implementation, API, or organizational details
