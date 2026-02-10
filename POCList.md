# Day 1 – Retail Order Processing (REST + JDBC + ERP API)

## Scenario

An e-commerce system sends orders to webMethods.
Orders must be validated, stored, and sent to ERP.

## Requirements

1. Create REST API:

```
POST /api/orders
```

2. Validate order data.
3. Insert order into database.
4. Call mock ERP API.
5. Return ERP order number.

## Components Used

- REST Resource
- Flow service
- JDBC Adapter
- HTTP client

---

# Day 2 – Bank Payment File Processing (File Poll + Flat File + JDBC)

## Scenario

Bank sends payment files every hour.

## Input File

```
paymentId,account,amount
P1,ACC100,200
P2,ACC200,-50
```

## Requirements

1. Poll SFTP/local folder.
2. Parse flat file.
3. Validate records:
   - amount > 0

4. Insert valid records into DB.
5. Generate:

- success file
- error file

## Components Used

- File Polling
- Flat File Schema
- JDBC Adapter

---

# Day 3 – Daily Customer Sync (Scheduler + JDBC + REST)

## Scenario

CRM system needs daily customer data from database.

## Requirements

1. Create scheduler:
   - Runs every 5 minutes.

2. Fetch customers from DB.
3. Send data to external CRM API.
4. Log results.

## Components Used

- Scheduler
- JDBC Adapter
- REST client

---

# Day 4 – Inventory Update via Universal Messaging (UM)

## Scenario

Warehouse system publishes inventory updates to UM topic.

## Requirements

1. Subscribe to:

```
/topic/inventory
```

2. When message arrives:
   - Parse inventory data.
   - Update inventory table in DB.

## Components Used

- UM Trigger
- JDBC Adapter

---

# Day 5 – Order Notification via UM (Publish)

## Scenario

When an order is created, notify downstream systems.

## Requirements

1. Receive order via REST API.
2. Validate and store in DB.
3. Publish order message to:

```
/topic/orders
```

## Components Used

- REST
- JDBC
- UM Publish

---

# Day 6 – Kafka-Based Shipment Events

## Scenario

Logistics system sends shipment events to Kafka.

## Requirements

1. Create Kafka consumer.
2. Subscribe to:

```
shipment-events
```

3. For each message:
   - Parse shipment data.
   - Insert into DB.

## Components Used

- Kafka Consumer
- JDBC Adapter

---

# Day 7 – Kafka Order Producer

## Scenario

Order system must send events to Kafka for analytics.

## Requirements

1. Create REST API:

```
POST /api/order
```

2. Validate order.
3. Publish to Kafka topic:

```
order-events
```

## Components Used

- REST
- Kafka Producer

---

# Day 8 – Insurance Claim File Processing

## Scenario

Insurance partner sends daily claim file.

## Input File

```
claimId,policyId,amount
C1,P100,500
C2,P200,-100
```

## Requirements

1. Poll claim file.
2. Parse flat file.
3. Validate:
   - amount > 0

4. Insert valid claims into DB.
5. Generate error file.

## Components Used

- File Polling
- Flat File
- JDBC

---

# Day 9 – Product Catalog Sync (Scheduler + File + API)

## Scenario

Supplier provides daily product catalog file.

## Requirements

1. Scheduler triggers job.
2. Read product file.
3. Parse records.
4. Call product API for each record.
5. Log results.

## Components Used

- Scheduler
- File Reader
- Flat File
- REST client

---

# Day 10 – End-to-End Enterprise Flow (File + DB + Kafka)

## Scenario

A retailer sends daily order file.
System must process and send events to Kafka.

## Requirements

1. Scheduler triggers process.
2. Poll order file.
3. Parse flat file.
4. Validate records.
5. Insert valid orders into DB.
6. Publish each order to Kafka:

```
orders-topic
```

7. Generate:

- success file
- error file

## Components Used

- Scheduler
- File Polling
- Flat File
- JDBC
- Kafka Producer

---

# What Developers Learn from These 10 POCs

| Area            | Covered POCs             |
| --------------- | ------------------------ |
| REST APIs       | Day 1, 5, 7              |
| JDBC            | Day 1, 2, 3, 4, 6, 8, 10 |
| Flat File       | Day 2, 8, 9, 10          |
| File Polling    | Day 2, 8, 10             |
| Scheduler       | Day 3, 9, 10             |
| UM Publish      | Day 5                    |
| UM Subscribe    | Day 4                    |
| Kafka Producer  | Day 7, 10                |
| Kafka Consumer  | Day 6                    |
| End-to-End Flow | Day 10                   |

---
