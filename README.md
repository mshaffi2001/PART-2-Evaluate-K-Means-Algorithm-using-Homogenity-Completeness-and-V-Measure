# PART-2-Evaluate-K-Means-Algorithm-using-Homogenity-Completeness-and-V-Measure
Assignment Data Science Concept

# TGTS Tracker — MongoDB Implementation

A shipment tracking database built with MongoDB, Node.js, and Express.
Supports customer profiles, dashboard config, alerts with TTL, and UI preferences.

---

## Prerequisites

- Node.js v18+
- MongoDB (local) or MongoDB Atlas
- mongosh

---

## Setup

### 1. Clone / unzip the project
```bash
cd tgts-tracker
```

### 2. Install dependencies
```bash
npm install
```

### 3. Configure environment
Create a `.env` file in the root folder:
```
MONGO_URI=mongodb://localhost:27017/tgts_tracker
PORT=3000
```

---

## Database Setup

Run these scripts in order from your terminal:

### Step 1 — Create collections, validation, and indexes
```bash
mongosh tgts_tracker db/01_create_collections.js
```

### Step 2 — Insert sample data
```bash
mongosh tgts_tracker db/02_seed_data.js
```

### Step 3 — Run aggregation pipeline
```bash
mongosh tgts_tracker db/03_aggregation.js
```

---

## Start the API
```bash
node api/server.js
```

Server runs at: `http://localhost:3000`

---

## Project Structure
```
tgts-tracker/
├── db/
│   ├── 01_create_collections.js   # Schema, validation, indexes
│   ├── 02_seed_data.js            # Sample data for 3 accounts
│   └── 03_aggregation.js          # Dashboard labels + alert count
├── api/
│   ├── server.js                  # Express entry point
│   ├── db.js                      # MongoDB connection
│   └── routes/
│       └── profile.js             # All CRUD + merged profile routes
├── screenshots/                   # Evidence screenshots
├── report/                        # PDF report
├── .env                           # Environment variables
├── package.json
└── README.md
```

---

## Sharding Strategy

Shard key: `{ _id: "hashed" }` on all collections.
Rationale: Hash sharding prevents hotspots from sequential account numbers
and distributes load evenly across shards for 10,000 concurrent users.
```javascript
sh.enableSharding("tgts_tracker")
sh.shardCollection("tgts_tracker.customers",  { _id: "hashed" })
sh.shardCollection("tgts_tracker.dashboards", { _id: "hashed" })
sh.shardCollection("tgts_tracker.alerts",     { _id: "hashed" })
sh.shardCollection("tgts_tracker.ui_configs", { _id: "hashed" })
```

