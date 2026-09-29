# 02 Technical Design Specification

## 1. Technical Design Overview

The proposed Grill & Go system is a web-based digital ordering and fulfillment system. Customers access the mobile ordering interface through table-specific QR codes. Orders require successful payment before being transmitted to the kitchen. Kitchen staff use a tablet-based KDS, while Store Managers control menu availability. Transactional data is stored in a relational database and can be consumed by Power BI or Tableau.

## 2. C4 Architecture Model

### C1 — System Context

The System Context Diagram identifies:

- Customer
- Kitchen Staff
- Store Manager
- Grill & Go Digital Ordering System
- PayNow / Payment Gateway
- Power BI / Tableau

See `diagrams/c4-context.mmd`.

### C2 — Container Diagram

The main containers are:

1. **Mobile Web Front-End** — customer-facing mobile browser interface.
2. **API Backend** — business logic, order processing, payment coordination, menu availability and KDS services.
3. **Relational Database** — stores tables, orders, order items, menu items and payments.
4. **KDS Tablet Interface** — kitchen-facing interface for paid orders and status updates.

External integrations:

- PayNow / payment gateway
- Power BI / Tableau

See `diagrams/c4-container.mmd`.

## 3. UML Sequence Diagram — Order Placement and PayNow Payment

The sequence flow covers:

1. Customer selects menu items.
2. Mobile Front-End sends the order request to the API Backend.
3. API Backend validates the order and creates the order record.
4. Customer is directed to the payment process.
5. Payment is processed through the PayNow / payment gateway.
6. Successful payment is confirmed to the API Backend.
7. API Backend records the payment.
8. API Backend releases the paid order to the KDS.
9. Kitchen Staff can view the order as Pending.

The order must not be transmitted to the kitchen before successful payment confirmation.

See `diagrams/sequence-diagram.mmd`.

## 4. Database Schema / ERD

### Tables

```text
tables
------
table_id PK
table_number
qr_code
status
```

### Orders

```text
orders
------
order_id PK
table_id FK -> tables.table_id
order_status
total_amount
created_at
```

### Order Items

```text
order_items
-----------
order_item_id PK
order_id FK -> orders.order_id
menu_item_id FK -> menu_items.menu_item_id
quantity
unit_price
customization
```

### Menu Items

```text
menu_items
----------
menu_item_id PK
name
description
price
availability
```

### Payments

```text
payments
--------
payment_id PK
order_id FK -> orders.order_id
payment_method
payment_status
transaction_reference
paid_at
```

### Cardinalities

- One table can have many orders.
- One order can contain many order items.
- One menu item can appear in many order items.
- An order can have payment records associated with it.

See `diagrams/erd.mmd`.

## 5. API Interface Specifications

### 5.1 POST /api/v1/orders

**Purpose:** Create a customer order and initiate the required payment process.

**Request**

```json
{
  "table_id": 12,
  "items": [
    {
      "menu_item_id": 5,
      "quantity": 2,
      "customization": {
        "steak_doneness": "Medium"
      }
    }
  ],
  "payment_method": "PAYNOW"
}
```

**Successful Response**

```json
{
  "order_id": 10025,
  "status": "PENDING_PAYMENT",
  "total_amount": 24.00,
  "payment_required": true
}
```

**Validation considerations**

- `table_id` must identify a valid table.
- Each `menu_item_id` must identify an available menu item.
- Quantity must be valid for the order.
- Required customization values must be valid.
- Payment method must be supported.
- The order must not be released to the KDS until payment is confirmed.

### 5.2 PATCH /api/v1/menu/items/{item_id}

**Purpose:** Allow an authorized Store Manager to change menu item availability.

**Request**

```json
{
  "availability": "OUT_OF_STOCK"
}
```

**Successful Response**

```json
{
  "menu_item_id": 5,
  "availability": "OUT_OF_STOCK"
}
```

**Validation considerations**

- `item_id` must identify an existing menu item.
- The caller must be authorized as a Store Manager.
- Availability must be a supported state.
- The change should take effect for new customer ordering.

## 6. Requirements Traceability Matrix

| Raw Requirement | User Story | Use Case | API / Component | Database |
|---|---|---|---|---|
| BR-01 Mobile browser QR ordering | US-01 | Scan Table QR Code / View Menu | Mobile Web Front-End | tables, menu_items |
| BR-02 Customization | US-02 | Customize Order | POST /api/v1/orders | order_items, menu_items |
| BR-03 Digital order submission | US-03 | Place Order | POST /api/v1/orders | orders, order_items |
| BR-04 Pay before kitchen | US-03 | Make Payment | Payment integration / POST /api/v1/orders | payments |
| BR-05 KDS | US-04 | View Paid Orders | KDS Tablet Interface / API Backend | orders |
| BR-06 Order statuses | US-05 | Update Order Status | API Backend / KDS | orders |
| BR-07 Out of Stock | US-06, US-07 | Manage Menu Availability | PATCH /api/v1/menu/items/{item_id} | menu_items |
| BR-08 30,000 monthly transactions | US-03, US-04 | Place/View Orders | API Backend + Database | All transactional tables |
| BR-09 100 concurrent sessions | US-01, US-03 | View Menu / Place Order | Mobile Front-End + API Backend | orders |
| BR-10 BI analytics | US-08 | View Sales Analytics | Power BI / Tableau | Transactional database |

## 7. Technical Non-Functional Requirements

### Scalability

- The architecture shall support approximately 30,000 transactions per month.
- The architecture shall support bursts of up to 100 concurrent mobile browser sessions.
- The API and relational database shall be designed so that additional capacity can be added if transaction volume increases.

### Security

- HTTPS shall protect communication between browsers, tablets and the backend.
- Manager-only menu controls shall be protected by authorization.
- Payment confirmation shall be validated before kitchen release.
- Payment-sensitive data should remain with the payment provider where appropriate rather than being unnecessarily stored in the application database.

### Reliability

- Orders shall maintain a clear payment and fulfillment state.
- A failed payment shall not cause an unpaid order to be released to the kitchen.
- The KDS shall receive only orders that satisfy the payment-release condition.

### Analytics

- The relational transactional database shall be accessible to Power BI or Tableau as required by the business.
- Revenue and peak-hour sales information shall be represented through the transactional data.

## 8. Technical Sign-Off

| Approval Role | Engineer Name | Project Role | Approval Status | Timestamp (SGT) | Digital Sign-Off (Git ID) |
|---|---|---|---|---|---|
| Lead Systems Analyst | [Student A Name] | Systems Analyst / Author | PENDING REVIEW | [Actual timestamp] | [Git ID] |
| Lead Software Engineer | [Student B Name] | Software Architect / Lead Developer | PENDING REVIEW | [Actual timestamp] | [Git ID] |
| QA & Data Engineer | [Student C Name] | Integration & BI Specialist | PENDING REVIEW | [Actual timestamp] | [Git ID] |

> Replace placeholders with actual team details and approval information. Do not mark a document APPROVED until the relevant person has actually reviewed it.
