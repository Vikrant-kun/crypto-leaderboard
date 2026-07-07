<img width="842" height="1554" alt="feed" src="https://github.com/user-attachments/assets/2bb71069-2d7e-40df-ba74-15df19dd418c" /># Crypto Leaderboard

A full-stack real-time leaderboard system built with **Go**, **Redis**, **PostgreSQL**, **WebSockets**, and **Next.js**.

This project demonstrates how modern real-time systems can maintain low-latency leaderboards by combining Redis Sorted Sets with persistent PostgreSQL storage while broadcasting updates to connected clients over WebSockets.

Rather than continuously polling the server, clients maintain a persistent WebSocket connection and receive leaderboard updates instantly whenever player scores change.

---

## Overview

The project consists of two independent repositories:

| Repository | Description |
|------------|-------------|
| **Frontend** | Next.js dashboard with animated leaderboard, podium, avatars and live activity feed |
| **Backend** | Go server responsible for score simulation, ranking, persistence and WebSocket broadcasting |

Frontend Repository

```
https://github.com/Vikrant-kun/crypto-leaderboard-frontend
```

Backend Repository

```
https://github.com/Vikrant-kun/crypto-leaderboard-backend
```

---

# Demonstration

## Dashboard

<img width="1440" height="819" alt="hero" src="https://github.com/user-attachments/assets/78615cba-2e63-4225-8a8a-c4db482cf0f4" />



---

## Live Updates

<img width="831" height="816" alt="leaderboard" src="https://github.com/user-attachments/assets/756cb820-3bf8-4ec5-b23c-8387fe539d4f" />


---

## Activity Feed

<img width="842" height="1554" alt="feed" src="https://github.com/user-attachments/assets/25cbccdb-7b62-4552-acc2-863ce0f9e2e5" />


---

# System Architecture

<img width="1027" height="342" alt="architecture" src="https://github.com/user-attachments/assets/d2b6a2ae-cc2b-49da-8be0-186b5e7dee19" />


---

# Request Flow

<img width="1031" height="332" alt="request-flow" src="https://github.com/user-attachments/assets/8a15c415-a904-46cc-8a59-797a6afb225b" />


---

# Score Update Flow

```
Score Generated

        │

        ▼

Update PostgreSQL

        │

        ▼

Update Redis Sorted Set

        │

        ▼

Retrieve latest rankings

        │

        ▼

Broadcast leaderboard

        │

        ▼

Broadcast activity event

        │

        ▼

Frontend updates instantly
```

---

# Why Redis?

The leaderboard is maintained using Redis Sorted Sets instead of SQL ordering.

Redis provides:

- O(log N) score updates
- Efficient ranking queries
- Extremely low latency
- In-memory performance

This avoids repeatedly sorting relational database records every time a score changes.

---

# Why PostgreSQL?

Redis is optimized for speed, not durability.

PostgreSQL serves as the persistent source of truth by storing:

- Player ID
- Username
- Score
- Update timestamp

This separation allows Redis to focus exclusively on ranking while PostgreSQL guarantees persistence.

---

# Why WebSockets?

Traditional polling introduces unnecessary requests and delays.

Instead, the frontend maintains a persistent WebSocket connection.

Whenever a score changes, the backend immediately broadcasts:

- Updated leaderboard
- Live activity event

Every connected client receives the update in real time.

---

# Features

## Frontend

- Animated podium
- Real-time leaderboard
- Live activity feed
- Animated score counters
- Responsive layout
- Glassmorphism UI
- Dynamic avatars
- Connection status indicator

---

## Backend

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

# Technology Stack

| Layer | Technology |
|---------|------------|
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

# Project Structure

```
crypto-leaderboard/

README.md

assets/

frontend/

backend/
```

Frontend and backend are maintained as separate repositories.

---

# Performance

| Operation | Complexity |
|------------|------------|
| Redis score update | O(log N) |
| Rank lookup | O(log N) |
| Top N leaderboard | O(log N + M) |
| PostgreSQL update | O(1) |
| WebSocket broadcast | O(C) |

Where:

- **N** = total players
- **M** = leaderboard size requested
- **C** = connected clients

---

# Running Locally

## Backend

```bash
git clone https://github.com/Vikrant-kun/crypto-leaderboard-backend
cd crypto-leaderboard-backend
go mod tidy
go run main.go
```

Backend runs on

```
localhost:8080
```

---

## Frontend

```bash
git clone https://github.com/Vikrant-kun/crypto-leaderboard-frontend
cd crypto-leaderboard-frontend
npm install
npm run dev
```

Frontend runs on

```
localhost:3000
```

---

# Future Improvements

- Redis Pub/Sub
- Docker Compose
- Kubernetes deployment
- Authentication
- Historical score analytics
- Prometheus metrics
- Grafana dashboards
- Rate limiting
- Unit testing
- Integration testing
- Horizontal scaling
- Multiple leaderboard categories
- Tournament support

---

# Note

This project includes a built-in score simulator that periodically updates random player scores.

The simulator exists to demonstrate the real-time architecture. In a production environment, score updates would originate from actual game events, trading systems, or external services.

---

# Author

**Vikrant**

GitHub

```
https://github.com/Vikrant-kun
```

---

# License

MIT License
