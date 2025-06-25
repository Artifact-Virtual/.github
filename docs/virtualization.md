# UltraCore Virtualization CTL Setup

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg) ![FastAPI](https://img.shields.io/badge/FastAPI-Framework-green.svg) ![React](https://img.shields.io/badge/React-JS-blue.svg) ![TailwindCSS](https://img.shields.io/badge/TailwindCSS-Framework-blue.svg) ![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-orange.svg) ![Grafana](https://img.shields.io/badge/Grafana-Dashboard-orange.svg) ![Kubernetes](https://img.shields.io/badge/Kubernetes-Helm-blue.svg)

## Overview
This project delivers a full-stack system for deploying, managing, and monitoring high-performance VMs with AI workloads. It includes:

- `ultracorectl`: CLI for VM orchestration.
- Backend: FastAPI API to control virtual environments, GPU passthrough, WASM VM runners.
- Frontend: React + Tailwind dashboard for live control and insights.
- Metrics: Prometheus + Grafana integration for detailed monitoring.
- Smart Scaling: Just-in-Time auto-scaling for AI-heavy virtual machines.
- Templates: Preloaded Ollama and WASM-optimized VM configurations.
- Secure Boot: UEFI-secure VM launch with signed kernels.
- Remote LLM Control: Trigger and manage LLM workloads from UI or `ultracorectl`.
- Encrypted Templates: All template files hosted with AES256 encryption and GPG validation.
- Prebuilt Images: Optional ready-to-deploy bundles for rapid setup.

---

## Directory Structure
```
ultracore/
├── ultracorectl/                # CLI tool
├── ui/
│   ├── backend/                # FastAPI server
│   └── frontend/               # React dashboard (Live Stats, Logs, Control)
├── monitor/                    # Prometheus & Grafana configs
├── templates/                  # Predefined AI/VM templates (Ollama, WASM, etc.)
├── charts/                     # Helm chart for Kubernetes deployment
└── diagrams/                   # Architecture visualizations
```

---

## ultracorectl (CLI)
Supports commands like:
```bash
ultracorectl create-vm --cpu 8 --ram 32G --gpu passthrough --ai ollama
ultracorectl start vm1
ultracorectl attach-container vm1 container123
ultracorectl stats vm1
ultracorectl scale vm1 --jit
```

- `create-vm`: Launch with GPU passthrough and AI workloads
- `attach-container`: Embed containers inside VMs (unfolded virtualization)
- `stats`: Real-time telemetry output via CLI
- `scale`: JIT-scaling control for resources

---

## Backend (FastAPI)
Supports API routes:
```python
POST /vm/create
POST /vm/start
POST /vm/attach-container
GET  /vm/stats
POST /vm/scale
```

Key Enhancements:
- **GPU Passthrough**: Leverages libvirt + vfio
- **WASM VM Runners**: Supports WASMEdge, Wasmtime with low-latency VM integration
- **Kubernetes Bridge**: Exposes VM networking for K8s pod injection
- **JIT Auto-scaling**: Dynamically expands compute based on load
- **Template Loader**: Ollama and WASM environments provisioned on command
- **Secure Boot**: Verified boot chains with OVMF + signed kernel support
- **Remote LLM Control**: Secure WebSocket API for LLM inference triggers
- **Encrypted Template Hosting**: Downloads and decrypts templates using per-session GPG keys

---

## Frontend (React + Tailwind)
Live dashboard with:
- VM controls (Start, Stop, Restart, Create)
- Real-time graphs (CPU, RAM, GPU, Disk I/O)
- Container to VM controls
- WASM/AI workload deployment from UI
- Grafana metrics iframe embed for live insights
- Template Launch Panel (Ollama, WASM, etc.)
- Secure Boot config and LLM activation switches
- Encrypted template download UI

Components:
- `DashboardCard` - Visual overview
- `VMControls` - Start/Stop/etc.
- `LiveStatsPanel` - Graphs via Chart.js
- `ContainerAttachForm`
- `TemplateLoader`
- `SecureBootToggle`
- `RemoteLLMTrigger`

---

## Embedded Grafana Dashboard
Prometheus scrapes all node, VM, and container metrics. Grafana dashboard config located at:
```
monitor/grafana/provisioning/dashboards/ultracore_dashboard.json
```
Load this in Grafana to get:
- VM CPU/GPU/memory usage
- WASM runtime performance
- Container-to-VM bridge stats
- Auto-scaler JIT triggers and history

---

## Preloaded Templates
Templates are stored in JSON under `templates/`:

- `ollama-ai.json`
```json
{
    "name": "ollama-ai",
    "cpu": 8,
    "ram": "32G",
    "gpu": true,
    "packages": ["ollama", "cuda", "torch"],
    "ai_loader": "ollama"
}
```

- `wasm-optimized.json`
```json
{
    "name": "wasm-optimized",
    "cpu": 4,
    "ram": "8G",
    "wasm_runner": "wasmtime",
    "network": "bridge"
}
```

---

## Kubernetes Helm Chart
To deploy entire stack in a cluster:
```bash
cd charts && helm install ultracore ./
```

Chart includes:
- Backend (FastAPI) + service
- Frontend (React) + ingress
- VM pods w/ GPU passthrough (via kubevirt)
- Prometheus + Grafana + scrape configs
- PVCs for VM persistence and templates

---

## Architecture Diagram
```
                                             +-----------------------+
                                             |  UltraCore UI (React) |
                                             +-----------------------+
                                                                 |
                                                                 ↓
                                        +--------------------------+
                                        |   FastAPI Backend API    |
                                        +--------------------------+
                                            /   |    |    |    \   
                                         ↓    ↓    ↓    ↓     ↓
                             [Libvirt][Docker][WASM][K8s][Prometheus]
                                         |                     |
                        +----------------+     +------------------+
                        |   QEMU VMs     |     |   Containers     |
                        | (GPU, JIT AI)  |     |  Inside VMs      |
                        +----------------+     +------------------+
                                         |
                                 [Grafana UI]
```

---

## Scripts
Run it all:
```bash
# Start backend
cd ui/backend && uvicorn main:app --reload

# Start frontend
cd ui/frontend && npm install && npm run dev

# Start monitoring
cd monitor && docker-compose up -d

# Use CLI
./ultracorectl create-vm --cpu 8 --ram 32G --gpu passthrough --ai ollama
```

---

## Build Prebaked Image
To generate a complete prebuilt VM image:
```bash
./ultracorectl build-image --template ollama-ai --output ultracore-ollama.img
```

Includes:
- UltraCore backend + frontend
- Prometheus/Grafana stack
- Ollama preinstalled and GPU ready
- WASM runner provisioned
- Secure Boot signed kernel
- GPG-secured template image with encrypted metadata
- Remote LLM socket interface (via `/api/llm/trigger`)
- Ready for immediate deployment on any KVM host