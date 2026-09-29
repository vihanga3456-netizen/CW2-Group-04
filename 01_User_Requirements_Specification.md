# 01 User Requirements Specification

## 1. Project Overview

### Business Context

Grill & Go is a small Western food stall in Orchard Road, Singapore. The business handles approximately 30,000 unique customer orders per month, or around 1,000 orders per day. Long physical queues during lunch and dinner periods cause customer drop-offs and kitchen delays.

### Business Requirements

| ID | Requirement |
|---|---|
| BR-01 | Customers shall access the menu through table-specific QR codes using a mobile browser. |
| BR-02 | Customers shall be able to customize menu selections, including side dishes and steak doneness where applicable. |
| BR-03 | Customers shall submit orders through the digital ordering system without downloading an application. |
| BR-04 | Customers shall pay immediately using PayNow QR or credit card before an order is transmitted to the kitchen. |
| BR-05 | Kitchen staff shall view incoming paid orders on a tablet-based Kitchen Display System (KDS). |
| BR-06 | Kitchen staff shall update order status through Pending, Preparing, and Ready for Pickup. |
| BR-07 | Store managers shall be able to toggle menu items as Out of Stock in real time. |
| BR-08 | The system shall support approximately 30,000 monthly transactions. |
| BR-09 | The system shall support peak bursts of up to 100 concurrent mobile browser sessions. |
| BR-10 | Revenue and peak-hour sales data shall be available to Power BI or Tableau. |

## 2. Actors

### Primary Human Actors

- **Customer** — scans a table QR code, views the menu, customizes items, places an order, and pays.
- **Kitchen Staff** — views paid incoming orders and updates preparation status.
- **Store Manager** — manages menu availability and operational controls.

### External System Actors

- **PayNow / Payment Gateway** — processes and confirms payment.
- **Power BI / Tableau** — consumes transactional data for revenue and peak-hour sales analysis.

## 3. Agile User Stories and BDD Acceptance Criteria

### US-01 — Access Menu by QR Code

**User Story**

As a Customer, I want to scan the table-specific QR code so that I can access the Grill & Go menu without downloading an app.

**Acceptance Criteria**

```gherkin
Given I am sitting at a Grill & Go table
When I scan the table-specific QR code
Then the mobile browser shall display the Grill & Go ordering menu.
```

### US-02 — Customize Menu Items

**User Story**

As a Customer, I want to customize available menu choices so that my order reflects my preferences.

**Acceptance Criteria**

```gherkin
Given I am viewing the ordering menu
When I select a menu item with available customization options
Then the system shall allow me to select the available options such as side dishes or steak doneness.
```

### US-03 — Pay for an Order

**User Story**

As a Customer, I want to pay for my order using PayNow or credit card so that my paid order can be sent to the kitchen.

**Acceptance Criteria**

```gherkin
Given I have created an order
When I select PayNow or credit card and payment succeeds
Then the system shall record the successful payment
And the system shall make the paid order available to the kitchen.
```

### US-04 — View Paid Orders on KDS

**User Story**

As Kitchen Staff, I want to view incoming paid orders on the KDS so that I can prepare them.

**Acceptance Criteria**

```gherkin
Given a customer payment has been successfully confirmed
When the order is released to the kitchen
Then the KDS shall display the order with Pending status.
```

### US-05 — Update Order Status

**User Story**

As Kitchen Staff, I want to update the order status so that order progress can be managed.

**Acceptance Criteria**

```gherkin
Given an order is displayed on the KDS
When kitchen staff begin preparing the order
Then the status shall change from Pending to Preparing.
```

```gherkin
Given an order is being prepared
When kitchen staff finish preparation
Then the status shall change to Ready for Pickup.
```

### US-06 — Mark Menu Item Out of Stock

**User Story**

As a Store Manager, I want to mark a menu item as Out of Stock so that customers cannot order an unavailable item.

**Acceptance Criteria**

```gherkin
Given I am an authorized Store Manager
When I mark a menu item as Out of Stock
Then the item shall be shown as unavailable for new customer orders.
```

### US-07 — Restore Menu Item Availability

**User Story**

As a Store Manager, I want to restore an Out of Stock item so that customers can order it again when it becomes available.

**Acceptance Criteria**

```gherkin
Given a menu item is marked Out of Stock
When an authorized Store Manager changes its availability to available
Then the item shall become selectable for new customer orders.
```

### US-08 — Analyze Sales

**User Story**

As a Store Manager, I want transactional sales data to be available to Power BI or Tableau so that revenue and peak-hour sales can be tracked.

**Acceptance Criteria**

```gherkin
Given transactional order data exists
When the BI tool connects to the transactional data source
Then revenue and peak-hour sales information shall be available for analysis.
```

## 4. UML Use Case Diagram

The required Use Case Diagram is provided in `diagrams/use-case.mmd`.

Primary use cases:

- Scan Table QR Code
- View Menu
- Customize Order
- Place Order
- Make Payment
- View Paid Orders
- Update Order Status
- Manage Menu Availability
- Mark Item Out of Stock
- Restore Item Availability
- View Sales Analytics

The diagram uses `<<include>>` relationships where a parent use case necessarily invokes a required sub-function.

## 5. Non-Functional Requirements

### Performance and Capacity

- **NFR-01:** The system shall support approximately 30,000 transactions per month.
- **NFR-02:** The system shall support peak bursts of up to 100 concurrent mobile browser sessions.
- **NFR-03:** The API should return normal order-management responses within 2 seconds under expected operating load.

### Compatibility

- **NFR-04:** Customer ordering shall operate through a mobile browser without requiring a native application.
- **NFR-05:** The KDS shall operate through a tablet-accessible interface.

### Security

- **NFR-06:** Client-to-server communication shall use HTTPS.
- **NFR-07:** Only authorized Store Manager users shall be permitted to modify menu availability.
- **NFR-08:** Payment confirmation shall be validated before an order is released to the kitchen.

### Data and Integration

- **NFR-09:** Transactional data shall be stored in a relational database.
- **NFR-10:** The transactional database shall support connection to Power BI or Tableau for revenue and peak-hour sales tracking.

## 6. Client Sign-Off

| Approval Role | Stakeholder Name | Organization / Position | Approval Status | Timestamp (SGT) | Digital Sign-Off (Git ID) |
|---|---|---|---|---|---|
| Client / Business Owner | Uncle Bob | Owner, Grill & Go (Orchard Road) | PENDING CLIENT APPROVAL | [Actual timestamp] | [Git ID] |
| Lead Systems Analyst | [Student A Name] | Systems Analyst / Author | PENDING REVIEW | [Actual timestamp] | [Git ID] |

> Replace the placeholders with the team's actual names, timestamps, Git IDs, and approval status after review. Do not claim approval before it has occurred.
