# 🛡️ CYBERMON v11 — Sentinel Protocol

**A Real-Time Cybersecurity Monitoring and Threat Simulation Platform**

CYBERMON is an academic cybersecurity project designed to simulate security events, monitor system activity, detect suspicious behavior, and visualize potential threats through a real-time dashboard.

The platform combines event simulation, risk scoring, threat detection, and real-time monitoring to provide an interactive cybersecurity monitoring environment.

## 🚀 Getting Started

### Prerequisites

- Docker
- Docker Compose

### Installation and Launch

Clone the repository and navigate to the project directory:

```bash
git clone https://github.com/azizarfaoui123/Cybermon.git
cd cybermon
```

Build and start the services:

```bash
docker compose up --build
```

Open the application:

**http://localhost:3000**

Register a new account, then log in.

> **Note:** The first build may take several minutes.

To reset the database and remove persistent Docker volumes:

```bash
docker compose down -v
```

**Warning:** This command permanently removes the data stored in the associated Docker volumes.

---

## ✨ Main Features

- **Real-Time Dashboard:** Visualize security events and risk indicators.
- **Threat Detection:** Analyze simulated events and identify suspicious activity.
- **Risk Scoring:** Calculate risk scores using multiple detection factors.
- **Event Monitoring:** Browse and analyze recorded security events.
- **Alert Management:** Monitor detected threats and security alerts.
- **IP Blacklist:** Manage blocked or suspicious IP addresses.
- **User Management:** Manage registered users and access.
- **Event Simulation:** Generate security scenarios with configurable simulation speeds.
- **Real-Time Communication:** Receive live updates through WebSocket connections.

---

## ▶️ Starting the Simulation

1. Navigate to **Settings → Engine Core**.
2. Click **START SIMULATION**.
3. Select the simulation speed:
   - ×1
   - ×10
   - ×60

The simulation generates security events using an internal virtual clock.

## 🧠 Simulation Engine

### The Virtual 24-Hour Clock

CYBERMON uses an internal simulated clock that starts at `00:00:00` and progresses until `23:59:59`, independently of real-world time.

```text
00:00:00 ──────────────────────────► 23:59:59
   START SIMULATION                    AUTO-STOP
```

Event timestamps use the simulated clock and are stored using the date `2024-01-01`.

### Simulation Speed

| Speed | Real time per simulated second | Full simulated day |
|---|---:|---:|
| ×1 | 1 second | 24 hours |
| ×10 | 0.1 second | 2 hours 24 minutes |
| ×60 | Approximately 0.017 second | 24 minutes |

### Circadian Activity Model

The simulation uses different activity patterns depending on the virtual time of day.

| Simulated Time | Activity Pattern |
|---|---|
| 02:00–05:00 | Low normal traffic, higher simulated attack probability |
| 09:00–11:00 | Morning peak in normal activity |
| 13:00–15:00 | Afternoon peak in normal activity |
| 22:00–02:00 | Increased simulated attacker activity |

These patterns are simulation parameters, not measurements of real-world attack frequency.

### Risk Score Calculation

The risk score combines several factors:

```text
risk = base_score
     + freq_bonus     (repeated IP: +0 to +15)
     + time_bonus     (simulated night: +10)
     + geo_bonus      (selected countries: +8)
     + chain_bonus    (attack chain detected: +15 to +20)
     + history_bonus  (repeat offender: +0 to +10)

cap = 100
```

### Attack Chain Simulation

The simulation can generate multi-stage attack scenarios:

```text
Reconnaissance:
PORT_SCAN → BRUTE_FORCE → PRIVILEGE_ESCALATION

SQL Attack:
SQL_INJECTION → DATA_EXFILTRATION

DDoS:
5–12 simultaneous simulated DDoS sources
```

The documented 3% probability per simulation tick applies to the configured attack-chain generation mechanism.

### Risk Chart

The dashboard includes a risk visualization:

- **X-axis:** Simulated time (00:00–23:59).
- **Y-axis:** Average risk score per minute.
- **Red dashed line:** Critical risk threshold (80+).

---

## 🏗️ Project Architecture

The project is organized into two main components:

### Backend

Responsible for:
- API endpoints and application logic.
- Authentication.
- Database and Redis connectivity.
- Event ingestion and processing.
- Threat detection and simulation.
- WebSocket communication.

### Frontend

Responsible for:
- Dashboard visualization.
- Security event monitoring.
- Alert management.
- Authentication interfaces.
- User and settings management.
- Real-time data presentation.

---

## 🗄️ Database Access

Connect to the PostgreSQL container:

```bash
docker exec -it cybermon_postgres psql -U cybermon -d cybermon
```

### Example SQL Queries

View simulated events by time and category:

```sql
SELECT
    to_char(sim_timestamp, 'HH24:MI') AS sim_time,
    category,
    COUNT(*),
    AVG(risk_score)::int AS avg_risk
FROM events
GROUP BY 1, 2
ORDER BY 1;
```

View the most active countries:

```sql
SELECT country, COUNT(*) AS hits
FROM events
GROUP BY country
ORDER BY hits DESC
LIMIT 10;
```

---

## 🐳 Docker Commands

Start services in the background:

```bash
docker compose up -d
```

Follow backend logs:

```bash
docker compose logs -f backend
```

Stop services without deleting persistent data:

```bash
docker compose down
```

Stop services and remove associated volumes:

```bash
docker compose down -v
```

---

## 🛠️ Technologies

The project uses a backend/frontend architecture with containerized services.

The exact technology versions and dependencies are documented in the project configuration files.

---

## 🎓 Project Context

CYBERMON is an academic team project focused on cybersecurity monitoring, event simulation, threat detection, and real-time data visualization.

The platform is intended for educational and demonstration purposes. Its simulated attack scenarios do not represent actual attacks or replace a production-grade Security Information and Event Management (SIEM) solution.