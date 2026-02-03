# 🚀 Server Monitoring Stack: Prometheus, Grafana, & Docker

![Docker](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

## 📖 Project Overview
As a DevOps engineer, I needed a way to visualize what was happening "under the hood" of my Linux servers. I didn't just want to know *if* they were running; I wanted to track CPU spikes, memory leaks, and network traffic in real-time.

I built this industry-standard **Observability Stack** using:
* **Prometheus:** To collect and store metrics (The Database).
* **Grafana:** To visualize data with interactive dashboards (The Dashboard).
* **Node Exporter:** To expose system metrics from the host (The Sensor).
* **Docker Compose:** To orchestrate the entire stack in a single file.

---

## 🏗️ Architecture
The system follows a **Microservices** architecture where three containers communicate over a dedicated Docker network:

1.  **Node Exporter (Port 9100):** Acts as a "pulse monitor," reading kernel-level metrics (CPU, RAM, Disk) from the Ubuntu host.
2.  **Prometheus (Port 9090):** Configured to "scrape" (pull) data from Node Exporter every 15 seconds.
3.  **Grafana (Port 3000):** Connects to Prometheus to query data and render it into human-readable graphs.

---

## 🛠️ Implementation Details

### Phase 1: Environment & Prerequisites
* **OS:** Windows 10/11 with **WSL 2 (Ubuntu LTS)**.
* **Engine:** Docker Desktop for Windows (with WSL 2 Integration enabled).
* **Challenge:** Ensuring the Windows Docker engine could manage containers running natively inside the Linux terminal.

### Phase 2: Configuration as Code
I defined the entire infrastructure in code to ensure reproducibility.

**1. The Prometheus Config (`prometheus.yml`)**
Defined scrape jobs to target both the Prometheus container itself and the Node Exporter.

**2. The Orchestrator (`docker-compose.yml`)**
* **Networking:** Created a custom bridge network (`monitoring`) so containers can resolve each other by hostname (e.g., `http://prometheus:9090`).
* **Persistence:** Attached **Docker Volumes** to Prometheus and Grafana. This ensures that even if containers crash or restart, historical data and dashboard configurations are saved.

---

## 🚀 How to Run

### 1. Clone the Repository
```bash
git clone <your-repo-url>
cd monitoring-stack

```

### 2. Directory Structure

Ensure your files are organized like this:

```text
~/monitoring-stack/
├── docker-compose.yml
└── prometheus/
    └── prometheus.yml

```

### 3. Launch the Stack

Run the following command to download images and start containers in the background:

```bash
docker compose up -d

```

### 4. Access the Dashboards

| Service | URL | Default Creds |
| --- | --- | --- |
| **Grafana** | `http://localhost:3000` | `admin` / `admin` |
| **Prometheus** | `http://localhost:9090` | N/A |
| **Metrics** | `http://localhost:9100/metrics` | N/A |

---

## ⚙️ Configuration Setup (First Time Only)

1. **Login to Grafana:** Open `localhost:3000`.
2. **Add Data Source:**
* Go to **Connections > Data Sources > Prometheus**.
* **URL:** `http://prometheus:9090` (Note: Use the container name, not localhost).
* Click **Save & Test**.


3. **Import Dashboard:**
* Go to **Dashboards > Import**.
* Enter ID: `1860` (Node Exporter Full).
* Select your Prometheus data source and click **Import**.



---

## 🧠 Key Learnings

* **Port Mapping:** Learned the critical difference between Host Ports (exposed to browser) and Container Ports (internal listeners).
* **Service Discovery:** Leveraged Docker's internal DNS to allow containers to talk to each other by service name instead of fragile IP addresses.
* **Volume Management:** Understood that containers are ephemeral; without volumes, data is lost on restart.

---

## 🛑 Commands Cheat Sheet

| Action | Command |
| --- | --- |
| **Start Stack** | `docker compose up -d` |
| **Check Status** | `docker ps` |
| **Stop Stack** | `docker compose down` |
| **Stop & Delete Data** | `docker compose down -v` |

```

```
