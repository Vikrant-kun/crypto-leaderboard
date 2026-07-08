<div align="center">

# Crypto Leaderboard

A full-stack real-time leaderboard system built with Go, Redis, PostgreSQL, WebSockets, and Next.js.

[![Go](https://img.shields.io/badge/Go-1.22-00ADD8?style=flat-square&logo=go&logoColor=white)](https://go.dev/)
[![Next.js](https://img.shields.io/badge/Next.js-14-000000?style=flat-square&logo=next.js&logoColor=white)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?style=flat-square&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Redis](https://img.shields.io/badge/Redis-7-DC382D?style=flat-square&logo=redis&logoColor=white)](https://redis.io/)
[![WebSocket](https://img.shields.io/badge/Protocol-WebSocket-4A90D9?style=flat-square)](https://datatracker.ietf.org/doc/html/rfc6455)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)

</div>

---

## Overview

Crypto Leaderboard maintains a low-latency, real-time ranking system by combining Redis Sorted Sets for fast in-memory ranking with PostgreSQL for durable storage, broadcasting every change to connected clients over a persistent WebSocket connection instead of relying on client-side polling.

The project is split into two independent repositories:

| Repository | Description | Link |
|---|---|---|
| Frontend | Next.js dashboard with animated leaderboard, podium, avatars, and live activity feed | [crypto-leaderboard-frontend](https://github.com/Vikrant-kun/crypto-leaderboard-frontend) |
| Backend | Go server handling score simulation, ranking, persistence, and WebSocket broadcasting | [crypto-leaderboard-backend](https://github.com/Vikrant-kun/crypto-leaderboard-backend) |

---

## Demonstration

### Dashboard

<p align="center">
  <img width="720" alt="Dashboard overview" src="https://github.com/user-attachments/assets/78615cba-2e63-4225-8a8a-c4db482cf0f4" />
</p>

### Live Updates

<p align="center">
  <img width="600" alt="Live leaderboard updates" src="https://github.com/user-attachments/assets/756cb820-3bf8-4ec5-b23c-8387fe539d4f" />
</p>

### Activity Feed

<p align="center">
  <img width="320" alt="Live activity feed" src="https://github.com/user-attachments/assets/25cbccdb-7b62-4552-acc2-863ce0f9e2e5" />
</p>

---

## System Architecture

<p align="center">
  <img width="720" alt="System architecture diagram" src="https://github.com/user-attachments/assets/d2b6a2ae-cc2b-49da-8be0-186b5e7dee19" />
</p>

## Request Flow

<p align="center">
  <img width="720" alt="Request flow diagram" src="https://github.com/user-attachments/assets/8a15c415-a904-46cc-8a59-797a6afb225b" />
</p>

## Score Update Flow

<p align="center">
  <img width="420" alt="Score update flow diagram" src="https://github.com/user-attachments/assets/b89a9c11-4b3f-476d-b4b1-0c4a71ab4bc8" />
</p>

---

## Design Decisions

### Why Redis

The leaderboard is maintained using Redis Sorted Sets instead of SQL-based ordering.

- O(log N) score updates
- Efficient ranking and range queries
- In-memory performance with sub-millisecond latency
- Avoids re-sorting relational records on every score change

### Why PostgreSQL

Redis is optimized for speed, not durability. PostgreSQL is the persistent source of truth, storing:

- Player ID
- Username
- Score
- Last updated timestamp

This split lets Redis focus exclusively on ranking while PostgreSQL guarantees data durability.

### Why WebSockets

Polling introduces unnecessary requests and update delay. The frontend instead holds a persistent WebSocket connection, and the backend pushes two events on every score change:

- Updated leaderboard state
- Live activity event

Every connected client receives the update immediately, with no client-initiated requests.

---

## Features

**Frontend**
- Animated podium
- Real-time leaderboard
- Live activity feed
- Animated score counters
- Responsive layout
- Glassmorphism UI
- Dynamic avatars
- Connection status indicator

**Backend**
- Go HTTP server
- Gorilla WebSocket
- Redis Sorted Sets
- PostgreSQL persistence
- Concurrent score simulator
- Thread-safe client management
- Graceful shutdown
- REST API
- WebSocket streaming

---

## Technology Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js |
| UI | React |
| Language | TypeScript |
| Styling | Tailwind CSS |
| Animation | Framer Motion |
| Backend | Go |
| Database | PostgreSQL |
| Cache | Redis |
| Protocol | WebSocket |

---

## Performance

| Operation | Complexity |
|---|---|
| Redis score update | O(log N) |
| Rank lookup | O(log N) |
| Top-N leaderboard | O(log N + M) |
| PostgreSQL update | O(1) |
| WebSocket broadcast | O(C) |

Where **N** = total players, **M** = requested leaderboard size, **C** = connected clients.

---

## Project Structure

```
crypto-leaderboard/
├── README.md
├── assets/
├── frontend/          # crypto-leaderboard-frontend (separate repo)
└── backend/           # crypto-leaderboard-backend (separate repo)
```

Frontend and backend are maintained as separate repositories and linked above.

---

## Running Locally

### Backend

```bash
git clone https://github.com/Vikrant-kun/crypto-leaderboard-backend
cd crypto-leaderboard-backend
go mod tidy
go run main.go
```

Runs on `localhost:8080`

### Frontend

```bash
git clone https://github.com/Vikrant-kun/crypto-leaderboard-frontend
cd crypto-leaderboard-frontend
npm install
npm run dev
```

Runs on `localhost:3000`

---

## Note on the Score Simulator

This project includes a built-in score simulator that periodically updates random player scores. It exists to demonstrate the real-time architecture. In production, score updates would originate from actual game events, trading systems, or external services.

---

## Future Improvements

- Redis Pub/Sub
- Docker Compose
- Kubernetes deployment
- Authentication
- Historical score analytics
- Prometheus metrics
- Grafana dashboards
- Rate limiting
- Unit and integration testing
- Horizontal scaling
- Multiple leaderboard categories
- Tournament support

---

## Author

**Vikrant**
[GitHub — @Vikrant-kun](https://github.com/Vikrant-kun)

---

## License

Distributed under the MIT License.
