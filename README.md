# 🎯 HitMap

**HitMap** is a Spring Boot-based backend application designed to manage a lottery-style betting system with clear access patterns, a clean architecture, and NoSQL principles using **AWS DynamoDB**. The system supports **admin game management**, **client bet placement**, **round-based draws**, and **winner reporting**.

The goal of this project is to provide a scalable, cloud-ready backend showcasing advanced engineering practices and cloud-native architecture. The application is intended to be deployed on **AWS** infrastructure and exposed through **Cloudflare** for enhanced security and performance.

---

## 📌 Functional Overview

### 🔐 Authentication & Authorization
- JWT-based login and role-based access control.
- Roles: `ADMIN`, `CLIENT`.
- Clients must register before placing bets.
- Admins manage games, rounds, and reporting.

### 🎮 Game & Round Management
- Admins can:
  - Create games with a configurable closing time.
  - Define multiple rounds per game.
- The system automatically:
  - Closes games when time expires.
  - Assigns random numbers to rounds.

### 🎰 Betting Engine
- Clients:
  - Place bets on active rounds.
  - Submit number selections.
- Constraints:
  - Bets must be submitted at least 24 hours before round closes.
  - No online payment integration (in-person payment only).
- Admins can mark bets as `PAID` or `UNPAID`.

### 📈 Reports & Winner Calculation
- After each round closes:
  - System calculates number of hits per bet.
  - Admins can generate a top-3 winner report.
  - Optional export to CSV (future enhancement).

---

## 🧱 Tech Stack

### 🛠 Backend
- **Java 21** + **Spring Boot 3**
- **Spring Security** (JWT)
- **Spring Web** (RESTful APIs)
- **Spring Scheduler** (for game/round automation)
- **Spring Data (custom)** for AWS DynamoDB access
- **Testcontainers** + **JUnit 5** for testing

### ☁️ Cloud & Infra
- **AWS DynamoDB** – NoSQL, cost-efficient access-pattern-first storage
- **AWS IAM / SSM** – Secure secrets and environment config
- **Cloudflare** – API gateway, DDoS protection, custom domain routing
- **Docker / LocalStack** – Local development environment
- **GitHub Actions** – CI/CD pipeline with build, test, deploy

---

## 🧭 DynamoDB Schema Design

| **PK**                  | **SK**                            | **Item**           |
|------------------------|-----------------------------------|--------------------|
| `CLIENT#<id>`          | `BET#<game_id>#<round_id>#<id>`   | Client Bet         |
| `ROUND#<id>`           | `BET#<id>`                         | Round Bet          |
| `GAME#<id>`            | `ROUND#<id>`                      | Game Round         |
| `GAME#<id>`            | `BET#<round_id>#<bet_id>`          | Game Bet Summary   |
| `CLIENT#<id>`          | `METADATA`                        | Client Profile     |
| `ADMIN#<id>`           | `METADATA`                        | Admin Profile      |
| `GAME#<id>`            | `METADATA`                        | Game Metadata      |

📝 **Note**: Bets are duplicated across entities to optimize for specific query access patterns — following NoSQL best practices like *avoiding joins* and *access-pattern-first design*.

---

## 🚀 Deployment Targets

### ✅ Cloudflare
- API endpoints exposed through Cloudflare tunnels or proxies
- DNS and SSL termination managed via Cloudflare

### ✅ AWS Services
- **DynamoDB** – Primary data store
- **IAM Roles** – Scoped access for secure infrastructure interactions
- **SSM Parameter Store** – Secret management
- **(Future)** Lambda/SQS/EventBridge for async bet processing

---

## 📦 Project Modules (Planned Layout)

```
lucky-admin/
├── api/                    # Spring Boot REST API
├── core/                   # Domain models and business rules
├── dynamodb/               # DynamoDB repository + config
├── scheduler/              # Automated jobs (game closure, results)
├── tests/                  # Integration + E2E test setup
├── docker/                 # Docker & LocalStack config
├── docs/                   # Diagrams, API specs
└── README.md
```

---

## 🧪 Test Strategy

- **Unit Tests** – JUnit 5 + Mockito
- **Integration Tests** – Spring Boot Test + Testcontainers + DynamoDB Local
- **API Tests** – RestAssured
- CI runs on **GitHub Actions**.

---

## 📘 Roadmap

- [ ] Game creation and scheduling
- [ ] Round random number generation
- [ ] Bet placement with validation
- [ ] Manual payment tracking
- [ ] Admin report generation (CSV export)
- [ ] Frontend Admin Dashboard (React)
- [ ] Asynchronous winner calculation (EventBridge / SQS)
- [ ] Caching layer for frequent reports (Redis)

---

## 📄 License

This project is open-source under the MIT License. See `LICENSE` for details.

---

## 🤝 Contributing

Contributions, ideas, and feedback are welcome! Create issues or submit PRs to enhance the platform.
