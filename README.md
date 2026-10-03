# HoneyBear (GreedyBear Clone) 🐻

A lightweight Threat Intelligence Platform (TIP) designed to ingest, process, and serve malicious IP blocklists collected from distributed honeypot sensors. Inspired by the open-source GreedyBear project.

## 🚀 Architecture Overview

*   **Sensor:** T-Pot / Cowrie honeypot sending JSON logs via Filebeat.
*   **Message Broker:** Redis for queueing incoming raw attack events.
*   **Core Backend:** FastAPI (Python) handles log processing, enrichment (GeoIP/ASN), and API endpoints.
*   **Database:** PostgreSQL (for metadata) + ClickHouse or Elasticsearch (for high-volume attack logs).

---

## 🛠️ Getting Started (Local Development)

### Prerequisites
*   Docker & Docker Compose
*   Python 3.10+
*   A MaxMind GeoLite2 API Key (for IP enrichment)

### 1. Clone & Setup Environment
```bash
git clone https://github.com
cd honeybear
cp .env.example .env
```

Edit your `.env` file and insert your configurations:
```env
DATABASE_URL=postgresql://user:password@localhost:5432/honeybear
MAXMIND_LICENSE_KEY=your_maxmind_key
API_SECRET_KEY=your_super_secret_jwt_key
```

### 2. Spin Up Infrastructure
Use Docker Compose to launch your database and queue layers:
```bash
docker-compose up -d postgres redis
```

### 3. Install Backend Dependencies & Run
```bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
pip install -r requirements.txt
```

Run the API and the background log worker:
```bash
# Terminal 1: Start the API
uvicorn app.main:app --reload --port 8000

# Terminal 2: Start the background log processor
python app/worker.py
```

---

