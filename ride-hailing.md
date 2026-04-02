# Technical Build Prompt — Scalable Ride-Hailing System (Modern Stack Upgrade)

You are a coding agent tasked with building a **production-grade, scalable ride-hailing system** with **modern (latest stable) technologies**, strong **project structure**, and **comprehensive testing strategy**.

This is not a prototype. Build a **clean, extensible, observable system**.

---

# 1. Technology Stack (MANDATORY)

## Backend

* Runtime: **Deno 2.x**
* Language: **TypeScript 6.x**
* HTTP: native or lightweight (Oak acceptable)
* Realtime: WebSockets

## Database

* **PostgreSQL 18** (via Docker)
* Port: **31101**

## Frontend

* **React 19**
* **TailwindCSS**
* **Radix UI**
* **Zustand**
* Build tool: Vite
* Port: **31103**

## Backend Port

* Deno API: **31102**

---

# 2. Repository Structure (STRICT)

```
root/
│
├── apps/
│   ├── server/              # Deno backend
│   ├── web/                 # React app
│
├── packages/
│   ├── geo-index/           # Grid-based indexing module
│   ├── matching-engine/     # Driver allocation logic
│   ├── types/               # Shared types
│
├── scripts/                 # Automation scripts
│   ├── dev.ts
│   ├── seed.ts
│   ├── migrate.ts
│   ├── load-test.ts
│
├── docs/
│   ├── architecture.md
│   ├── api.md
│   ├── scaling.md
│   ├── decisions.md
│   ├── ai/
│   │   ├── memory.md        # agent memory model
│   │   ├── state.md         # system state transitions
│   │   ├── prompts.md       # reusable prompts
│
├── tests/
│   ├── unit/
│   ├── integration/
│   ├── e2e/
│
├── docker-compose.yml
├── README.md
```

---

# 3. Infrastructure

## Docker (Postgres 18)

```yaml
version: "3.9"

services:
  db:
    image: postgres:18
    container_name: ride_db
    restart: always
    environment:
      POSTGRES_USER: ride
      POSTGRES_PASSWORD: ride
      POSTGRES_DB: ride
    ports:
      - "31101:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

---

# 4. Core System Requirements

## Functional

* Rider requests ride
* Nearby drivers discovered using **grid-based geo index**
* Matching within **<2 seconds**
* Driver accept/reject
* Real-time ride updates
* Cancellation handling

## Non-Functional

* Horizontal scalability
* Low latency
* Concurrency-safe assignment
* Fault tolerance
* Observability-ready

---

# 5. Geo Index (Grid-Based)

## Requirements

* O(1) updates
* Fast nearby search
* Memory-first design (pluggable to Redis)

## Interface

```ts
interface GeoIndex {
  upsertDriver(driverId: string, lat: number, lon: number): void;
  removeDriver(driverId: string): void;
  findNearby(lat: number, lon: number, limit: number): string[];
}
```

## Implementation

* Convert lat/lon → grid cell (~500m)
* Store:

  * `cell_id → Set<driverId>`
  * `driverId → cell_id`
* Expand search radius dynamically

---

# 6. Matching Engine (CRITICAL)

## Algorithm

1. Query nearby drivers (expand radius)
2. Rank:

   * Distance
   * Rating
   * Idle time
3. Dispatch:

   * Send to top 3–5 drivers (parallel)
4. Wait (timeout ~5 sec)
5. First accept wins

---

## Concurrency Control (MANDATORY)

* Ensure **single assignment per driver**

### Strategy

* Atomic state transition

```ts
if (driver.state === "IDLE") {
  driver.state = "ASSIGNED";
}
```

### Must be:

* Thread-safe
* Extendable to Redis:

  * `SETNX driver_lock`

---

# 7. Backend (Deno 2)

## Modules

* `routes/`
* `services/`
* `geo/`
* `matching/`
* `realtime/`

## APIs

### Rider

```
POST /rides/request
GET  /rides/:id
POST /rides/:id/cancel
```

### Driver

```
POST /drivers/location
POST /drivers/state
POST /drivers/:id/accept
POST /drivers/:id/reject
```

---

## WebSocket

* `/ws`
* Channels:

  * rider:{rideId}
  * driver:{driverId}

Events:

* driver_assigned
* ride_status_update
* driver_location_update

---

# 8. Frontend (React 19)

## Features

### Rider

* Request ride
* Live status
* Driver tracking

### Driver

* Toggle availability
* Accept/reject rides

---

## State (Zustand)

```ts
rideState = {
  rideId,
  status,
  driver,
  location
}
```

---

# 9. Scripts Directory (MANDATORY)

All operational tooling must live in `/scripts`.

## Required scripts

* `dev.ts` → start full system
* `seed.ts` → seed drivers/users
* `migrate.ts` → DB migrations
* `load-test.ts` → simulate drivers + riders
* `cleanup.ts` → reset system

---

# 10. Documentation (docs/)

## Required

* `architecture.md` → system design
* `api.md` → endpoints
* `scaling.md` → bottlenecks + mitigation
* `decisions.md` → trade-offs

## AI-Specific (docs/ai/)

* `memory.md`

  * What state persists?
  * What is ephemeral?

* `state.md`

  * Driver state machine
  * Ride lifecycle

* `prompts.md`

  * Reusable agent instructions

---

# 11. Testing Strategy (MANDATORY)

## Unit Tests

* Geo index
* Matching logic

## Integration Tests

* API + DB
* Ride lifecycle

## E2E Tests

* Use **Playwright**
* Simulate:

  * Rider requesting ride
  * Driver accepting
  * Full flow completion

---

## Structure

```
tests/
├── unit/
├── integration/
├── e2e/
```

---

# 12. Observability (Basic)

* Structured logs
* Request IDs
* Error tracking hooks

---

# 13. Scaling Considerations

## Must be reflected in code

### 1. Hotspots

* Limit drivers per query
* Prioritize closest

### 2. Sharding

* By geohash prefix or region

### 3. Stale drivers

* TTL eviction (e.g., 10s inactivity)

### 4. Stateless backend

* Ready for horizontal scaling

---

# 14. Deliverables

You must produce:

1. Fully working Docker + services
2. Backend (Deno 2 + TS6)
3. Geo index module
4. Matching engine with concurrency safety
5. WebSocket system
6. React 19 frontend
7. Scripts for operations
8. Complete documentation
9. Full test suite (unit + integration + e2e)

---

# 15. Constraints

* No external geo APIs
* No skipping concurrency
* No monolithic unstructured code

---

# 16. Evaluation Criteria

* Correctness of matching
* Concurrency safety
* Latency-aware design
* Code modularity
* Test coverage quality
* Documentation clarity

---

# Final Instruction

Build a **clean, scalable core system**, not feature-heavy noise.

Focus on:

* correctness
* clarity
* extensibility

Avoid:

* premature abstraction
* unnecessary frameworks

---

This system should be **interview-grade AND production-reasonable**.
