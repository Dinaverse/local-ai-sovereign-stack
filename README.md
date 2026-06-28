# 🤖 Local AI Sovereign Stack

> *A fully self-hosted, cloud-agnostic AI infrastructure stack combining Ollama, Docker, and Prometheus/Grafana for local LLM inference, autonomous workflows, and real-time observability.*

---

## 🎯 Overview

This repository defines the complete AI runtime environment of the sovereign lab. Every component runs locally on bare-metal hardware no API keys, no cloud billing, no data leaving the perimeter. The stack is deployed via Docker Compose and designed for resilience and reproducibility.

---

## 🧩 Stack Components

| Component | Role | Technology |
|-----------|------|------------|
| **LLM Runtime** | Local model inference | Ollama |
| **AI Model** | 27B parameter LLM | Qwen 3.5:27B |
| **GPU Backend** | Multi-GPU VRAM pooling | 4x NVIDIA P106-100 / CUDA |
| **Monitoring** | Real-time dashboards | Grafana |
| **Metrics** | Telemetry collection | Prometheus |
| **Workflows** | Autonomous automation | n8n |
| **Orchestration** | Service management | Docker Compose |

---

## 🏗️ Architecture

```
[Bare-Metal Host Arch Linux]
         │
    [Docker Engine]
         │
    ┌────┴────────────────────────────┐
    │                                 │
[Ollama]                      [Monitoring Stack]
  ├── GPU 0: P106-100              ├── Grafana  :3000
  ├── GPU 1: P106-100              ├── Prometheus :9090
  ├── GPU 2: P106-100              └── Node Exporter
  └── GPU 3: P106-100
         │
   [n8n Automation]
         │
  [Autonomous Workflows]
```

---

## ⚡ Deployment

```bash
# Clone and launch the full stack
git clone https://github.com/Dinaverse/local-ai-sovereign-stack
cd local-ai-sovereign-stack
docker compose up -d

# Verify services
docker compose ps
```

---

## 📊 Monitoring

- **Grafana** → `http://localhost:3000` GPU metrics, inference throughput, system health
- **Prometheus** → `http://localhost:9090` raw metrics & alerting rules
- **Ollama API** → `http://localhost:11434` LLM inference endpoint

---

## 🔗 Related Repositories

| Repository | Role |
|------------|------|
| [`arch-linux-multi-gpu-llm`](https://github.com/Dinaverse/arch-linux-multi-gpu-llm) | GPU cluster configuration |
| [`sovereign-ai-infrastructure`](https://github.com/Dinaverse/sovereign-ai-infrastructure) | Full infrastructure architecture |
| [`sovereign-lab-orchestration`](https://github.com/Dinaverse/sovereign-lab-orchestration) | Orchestration methodology |
| [`n8n-automation-hub`](https://github.com/Dinaverse/n8n-automation-hub) | Workflow definitions |

---

## 📚 Documentation

- [System Design](architecture/SYSTEM_DESIGN.md)
- [AI Architecture Overview](docs/AI_ARCHITECTURE_OVERVIEW.md)
- [Optimization Report](docs/LOCAL_AI_CONFIG_OPTIMIZATION.md)

---

*Local by choice. Sovereign by design. Zero cloud, full control.*
