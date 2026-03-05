**Audience:** Tour Operators, Tour Agencies, Corporate Clients
**Purpose:** Explain how the system works from a business and user perspective

---

## Introduction

GP Travel Enterprise is a platform for tour operators and travel agencies that supports resale of travel products between business partners and direct sales to end customers.

The system allows users to purchase products from their **direct upstream seller**, resell them to their own clients, and manage prices, commissions, and bookings with clear role-based visibility.

---

## Core System

| **System** | **Subsystem** | **Description**                         | **Key Functions**                                                                                                |
| ---------- | ------------- | --------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Buy**    | Inventory     | Internal product management             | Manual creation and management of hotels, transfers, excursions; descriptions, images, prices, and availability  |
| **Buy**    | Hub           | External supplier connections           | Automatic import of products, prices, and availability via API                                                   |
| **Sell**   | B2B Module    | Sales to agencies and corporate clients | Partner management, B2B pricing, booking flow                                                                    |
| **Sell**   | B2C Module    | Direct sales to tourists                | Online booking via website or API, customer accounts                                                             |
| **Manage** | Enterprise    | Business and booking management         | Booking lifecycle management, policies, and reporting                                                            |
| **Manage** | Gateways      | External system integrations            | Payments, CRM, accounting, third-party systems                                                                   |

---

## Roles in the Sales Chain

### Supplier
A supplier is a company that provides travel products, such as accommodation or services.  
Suppliers make their products available to tour operators under contractual conditions and confirm or reject bookings for their own products.

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

| **Role**                       | **Price they define**                                                                                     | **Price they see**                                                              | **Price they do NOT see**                                                               |
| ------------------------------ | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Supplier**                   | Base price for own products                                                                               | Own base price and availability                                                 | Prices, selling prices, margins, or commissions of any downstream participants          |
| **Tour Operator**              | Selling price for products they resell to direct clients (agencies, other tour operators, tourists)       | Direct upstream price (from supplier) and own selling price                     | Prices and margins of participants beyond their direct upstream partner                 |
| **Tour Operator (next level)** | Selling price for products they resell to their direct clients (agencies, other tour operators, tourists) | Direct upstream price (from their upstream tour operator) and own selling price | Prices and margins of participants beyond their direct upstream partner                 |
| **Agency**                     | —                                                                                                         | Direct upstream price (from their selling tour operator) and own commission     | Prices, margins, or base prices of any participant beyond their direct upstream partner |
| **Tourist**                    | —                                                                                                         | Final selling price                                                             | Any upstream prices, margins, or commissions                                            |
| **Corporate Client**           | —                                                                                                         | Contract price or final selling price according to corporate policy             | Any upstream prices, margins, or commissions                                            |

### Glossary:
- **Direct upstream partner** — the participant in the sales chain who directly sells the product to the current user and from whom the current user purchases the product
- **Base price** — the price set by the supplier for their own product
- **Selling price** — the price defined by a seller when reselling a product to their direct client
- **Upstream price** — the price defined by the direct seller of the product
- **Final selling price** — the price defined by the last seller in the chain and shown to the tourist or corporate client, without margins or commissions
- **Contract price** — a price applied to corporate clients according to corporate agreements and policies
- **Commission** — a portion of the selling price allocated to an agency or partner
- **Margin** — the difference between the upstream price and the selling price

---

## Booking and Vouchers

- A booking is created by the selling party (tour operator or agency) for their direct client, which may be another operator, an agency, or an end customer
- The upstream partner who provided the product confirms or rejects the booking
- After confirmation, the customer receives a voucher
- The voucher shows only the selling party’s information and the final selling price, without revealing upstream prices or margins
