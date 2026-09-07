<div align="center">

```text
███████╗██████╗  ██████╗ ███████╗    ██╗ ██████╗ 
██╔════╝██╔══██╗██╔════╝ ██╔════╝    ██║██╔═══██╗
█████╗  ██║  ██║██║  ███╗█████╗      ██║██║   ██║
██╔══╝  ██║  ██║██║   ██║██╔══╝      ██║██║▄▄ ██║
███████╗██████╔╝╚██████╔╝███████╗    ██║╚██████╔╝
╚══════╝╚═════╝  ╚═════╝ ╚══════╝    ╚═╝ ╚══▀▀═╝ 
```

<h1 align="center">EDGE IQ — Smart Retail</h1>
<h3 align="center"><em>Sense. Understand. Decide. Act. Measure.</em></h3>

---

[![Hardware](https://img.shields.io/badge/Hardware-ESP32%20%7C%20Arduino-00979D?style=for-the-badge&logo=arduino&logoColor=white)]()
[![Language](https://img.shields.io/badge/Language-Python%20%7C%20C%2B%2B-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)]()
[![AI](https://img.shields.io/badge/AI-Computer%20Vision-FF6F00?style=for-the-badge)]()
[![Status](https://img.shields.io/badge/Status-Prototype%20Ready-2ea44f?style=for-the-badge)]()
[![Architecture](https://img.shields.io/badge/Architecture-Edge--First-purple?style=for-the-badge)]()

> **Edge-AI-Powered Intelligent Retail Management Architecture**  
> *Built by Team INVictus · Vidyavardhaka College of Engineering*

<br>

<img src="assets/edge-iq-hero.svg" width="900" alt="EDGE IQ Architecture">

</div>

---

## What This Is

**EDGE IQ** is an edge-first intelligent retail management architecture. It connects physical-store data sources with local intelligence to make real-time operational decisions exactly where they happen. 

The main problem with modern retail is **fragmentation**. CCTV, POS, RFID, and sensors act independently, leading to limited operational responses. When a shelf goes empty or a queue builds up, staff react *too late*.

EDGE IQ reads **all streams simultaneously** using a tight local AI loop.  
**No cloud dependency. No massive latency. No excuses.**

```text
CCTV   (Vision) ────┬──► Local AI ────► Edge ERP
Sensors (IoT)   ────┘                 (Actions & Alerts)
```

---

## Comprehensive System Architecture

EDGE IQ is divided into 5 tightly integrated layers, communicating locally to guarantee low-latency, offline-first reliability.

### 1. IoT & Sensor Layer (The Senses)
This layer acts as the eyes and nerves of the store, constantly gathering real-time telemetry from physical events.
- **CCTV Networks:** Captures live visual feeds from checkout counters, aisles, and entry points.
- **ESP32 & Arduino Nodes:** Distributed microcontrollers monitoring temperature, humidity (for perishable goods), and physical shelf weight.
- **RFID Scanners:** Instantly recognizes pallet movements and high-value item tracking without line-of-sight.
- **Barcode & Weight Sensors:** Validates exactly what is placed inside smart trolleys or removed from shelves.

### 2. Edge AI Server Layer (The Brain)
Instead of streaming heavy video feeds to the cloud (which wastes bandwidth and violates privacy), the Edge AI server processes video **locally**.
- **Computer Vision Pipelines:** Uses optimized models (like YOLOv8/v10) to run **People Tracking**, **Product Detection**, and **Queue Estimation**.
- **Event Extraction:** The AI throws away the video frames immediately after processing, generating lightweight JSON events: e.g., `{"event": "queue_long", "lane": 3, "count": 8, "timestamp": 1694002345}`.
- **Privacy-First Processing:** No facial recognition. Customers are tracked as anonymous vectors moving through zones.

### 3. Edge ERP Layer (The Logic & Command Center)
The ERP listens to the AI and IoT events via an internal **MQTT Broker** and decides what needs to be done.
- **Inventory Engine:** Cross-references the POS database with CCTV shelf-monitoring. If the POS says "5 items left" but CCTV sees an empty shelf, the ERP flags an "Inventory Mismatch" alert.
- **FEFO Management Engine:** (First Expired, First Out). Tracks perishables and auto-generates discount promotions on digital displays for items nearing expiration.
- **Staff Orchestration System:** Pings wearable devices or staff dashboards. *"Restock Aisle 4"* or *"Open Checkout Lane 2"*.

### 4. Customer Interaction Layer (The Experience)
Reducing friction for the shopper by digitizing their physical journey.
- **Smart Trolley:** A shopping cart fitted with an ESP32, an LCD display, and a barcode scanner. Shoppers scan as they drop items in, see their running total, and bypass checkout lines entirely via digital payment integration.
- **QR Web Portal:** For shoppers without a smart trolley, scanning a store QR code opens a lightweight web app acting as a self-checkout terminal on their own smartphone.

### 5. Optional Cloud Layer (The Archive)
While the store operates 100% locally, the cloud provides asynchronous multi-store oversight.
- **Batch Synchronization:** Pushes aggregate sales data and footfall analytics to centralized AWS/GCP buckets during off-peak hours.
- **Global Dashboards:** Allows regional managers to compare branch performance and train better AI models on massive, anonymized, globally-aggregated datasets.

---

## The Data Flow Loop

### CAPTURE → ANALYZE → UNDERSTAND → DECIDE → ACT → MEASURE

1. **Sense:** A camera detects a spill in Aisle 3.
2. **Understand:** The Edge AI Server categorizes the visual anomaly as `hazard_spill`.
3. **Decide:** The Edge ERP queries the staff database to find the nearest available employee based on their current assignment.
4. **Act:** An alert is fired via WebSocket to the store manager's dashboard and the specific employee's device.
5. **Measure:** The system tracks how long it took for the anomaly to be cleared from the CCTV feed, storing the KPI locally in PostgreSQL to measure operational efficiency.

---

## The Technology Stack

| Component | Technologies & Protocols | Purpose |
|:---|:---|:---|
| **Hardware Nodes** | Arduino UNO, ESP32 | Lightweight sensor and trolley logic. |
| **Edge AI Framework** | Python, OpenCV, YOLO | Real-time object and queue detection. |
| **Message Broker** | Eclipse Mosquitto (MQTT) | Pub/Sub messaging for instant IoT events. |
| **Backend & ERP API** | Node.js / FastAPI / REST | Core business logic and staff alerting. |
| **Real-time Comms** | WebSockets (Socket.io) | Pushing live UI updates to dashboards. |
| **Database** | PostgreSQL (Local) | Robust relational storage for inventory & logs. |

---

## Key Intelligent Features

### Smart Inventory & FEFO
Monitors shelves in real-time. Detects low-stock and out-of-stock instantly via combined CCTV and RFID data. Manages **FEFO (First Expired, First Out)** to automatically suggest markdowns on perishable items, drastically reducing food wastage.

### Proactive Queue Intelligence
Transforms checkout-area observations into operational events. Estimates queue length, wait times, and predicts congestion before it escalates, alerting staff immediately to open new lanes.

### Offline-First Resilience
If the store's internet connection fails, the store stays smart.  
`Edge → Process → Decide → Local Action` continues normally. The local PostgreSQL instance buffers all transaction and event logs. It safely syncs to the cloud only when connectivity is restored.

---

## System Architecture Map

```text
[ CUSTOMER INTERACTION ]
  Smart Trolley (ESP32)  <══(WiFi)══>  Mobile QR Web App
           │                                 │
           └────────────┐       ┌────────────┘
                        ▼       ▼
[ IOT & SENSOR LAYER ]
  CCTV Cameras ─── Weight Sensors ─── RFID Gates
         │              │                 │
         └────────(RTSP / MQTT)───────────┘
                        ▼
[ EDGE AI SERVER ]
  Vision Pipeline ──► Anomaly Detection ──► Privacy Filter
                        │ (JSON Events)
                        ▼
[ EDGE ERP CORE ]
  Inventory Logic ◄──► PostgreSQL DB ◄──► Staff Routing
                        │ (Alerts/REST)
                        ▼
[ RETAIL OPERATIONS ]
  POS Terminals ─── Digital Signage ─── Staff Dashboards
                        │
                  (Async Sync)
                        ▼
[ OPTIONAL CLOUD LAYER ]
  Central Analytics ──► Multi-Store Data ──► Model Retraining
```

---

## Upload & Run

```bash
# 1. Clone the repository
git clone https://github.com/INVictus/EDGE-IQ.git

# 2. Setup Web & API Environment
cd EDGE-IQ/backend
npm install
cd ../frontend
npm install

# 3. Setup AI Environment
cd ../edge-ai
pip install -r requirements.txt

# 4. Configure Hardware & Broker
# Flash ESP32 firmware via Arduino IDE
# Start Mosquitto MQTT Broker: `mosquitto -c /etc/mosquitto/mosquitto.conf`

# 5. Launch Edge Node & AI Server
python edge_ai_server.py
```

> **Boot order:** Start the MQTT broker and PostgreSQL DB, followed by the Edge AI server, *before* powering on the ESP32 sensor nodes.

---

## Expected Advantages

| Advantage | Why It Matters |
|---|---|
| **Reduced Cloud Dependency** | AI inference happens on local hardware (GPUs/TPUs). No expensive cloud computing bills for processing video. |
| **Zero Decision Latency** | No round-trip to the cloud is required to alert staff about an active shoplifter or a massive queue. |
| **Strict Data Privacy** | Raw video feeds never leave the store's local network. Only anonymized numbers (e.g., "5 people waiting") hit the database. |
| **Offline Resilience** | Core store operations, billing, and AI alerts survive complete internet outages. |
| **Modular Scalability** | Add new cameras or sensors without rewriting the ERP logic thanks to the decoupled MQTT event architecture. |

---

<div align="center">

```text
See the store as data. Act in real-time.
```

**Built for the edge.**  
If you believe in decentralized intelligence, leave a star.

</div>
