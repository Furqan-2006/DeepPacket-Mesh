# DeepPacket Mesh

Real-time network packet inspection and anomaly detection. An eBPF/C++ capture layer feeds a Redis message bus, which drives a Python analytics engine and a Node.js WebSocket gateway, all visualized through a React dashboard.

---

## Architecture

```
eBPF probe (C++)  →  Redis  →  Analytics worker (Python / Scikit-learn)
                          ↘  Flask API server  →  PostgreSQL
                          ↘  Node.js WS gateway  →  React dashboard
```

**Kill-switch path:** React → Flask (authenticated) → Redis → netfilter

---

## Stack

| Layer     | Technology            | Role                                          |
| --------- | --------------------- | --------------------------------------------- |
| Capture   | C++ / eBPF (`libbpf`) | Kernel-level packet capture and dissection    |
| Bus       | Redis                 | Streams, pub/sub, command channel             |
| Analytics | Python, Scikit-learn  | Anomaly and DDoS detection                    |
| API       | Flask, PostgreSQL     | REST config, auth, historical storage         |
| Gateway   | Node.js               | WebSocket broadcast to clients                |
| Dashboard | React                 | Live traffic, geo IP map, alerts, IP blocking |

---

## Features

- Kernel-level packet capture with zero-copy eBPF ring buffers
- Real-time traffic visualization — bandwidth, protocol breakdown, flow graphs
- Geographic IP origin mapping
- ML-based anomaly and DDoS hotspot detection
- Kill-switch IP blocking routed through authenticated Flask endpoint to netfilter
- Independently scalable layers via Redis message bus

---

## Getting Started

> Prerequisites: Linux kernel ≥ 5.8, Docker, Node.js 20+, Python 3.11+

```bash
# Clone
git clone https://github.com/your-username/deeppacket-mesh.git
cd deeppacket-mesh

# Start Redis and PostgreSQL
docker compose up -d redis postgres

# Build and run the C++ probe (requires root for eBPF)
cd probe && make && sudo ./deeppacket-probe

# Start the Python workers
cd ../analytics && pip install -r requirements.txt && python worker.py
cd ../api && python app.py

# Start the Node.js gateway
cd ../gateway && npm install && npm start

# Start the React dashboard
cd ../dashboard && npm install && npm run dev
```

---

## Project Status

Work in progress.

---

## License

MIT
