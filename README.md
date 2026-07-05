# 🐳 Local AI Sovereign Stack

> *A fully self-hosted, cloud-agnostic AI infrastructure stack combining Ollama, Docker, and Prometheus/Grafana for local LLM inference, autonomous workflows, and real-time observability.*

---

## 🎯 Overview

This repository defines the complete AI runtime environment of the sovereign lab. Every component runs locally on bare-metal hardware — no API keys, no cloud billing, no data leaving the perimeter.

**Stack:** Ollama (LLM runtime) + Docker Compose + Prometheus/Grafana + n8n automation + Portainer management

**Host:** Arch-GPU node or Dell-Gateway (flexible deployment)

---

## 🧩 Stack Components

| Component | Role | Technology | Port |
|-----------|------|-----------|------|
| **LLM Runtime** | Local model inference | Ollama | 11434 |
| **AI Model** | 27B parameter LLM | Qwen 3.5:27B | - |
| **GPU Backend** | Multi-GPU VRAM pooling | 4× NVIDIA P106-100 / CUDA | - |
| **Monitoring** | Real-time dashboards | Grafana | 3000 |
| **Metrics** | Telemetry collection | Prometheus | 9090 |
| **Workflows** | Autonomous automation | n8n | 5678 |
| **Container Mgmt** | Docker UI | Portainer | 9443 |
| **Orchestration** | Service management | Docker Compose | - |

---

## 🏗️ Architecture

```
[Bare-Metal Host (Arch Linux / Dell Node)]
         │
    [Docker Engine]
         │
    ┌────┴────────────────────────────┐
    │                                 │
[Ollama Container]            [Monitoring Stack]
  ├── GPU 0: P106-100             ├── Grafana :3000
  ├── GPU 1: P106-100             ├── Prometheus :9090
  ├── GPU 2: P106-100             └── Node Exporter
  └── GPU 3: P106-100
         │
    [n8n Automation]
         │
   [Autonomous Workflows]
```

---

## ⚡ Deployment

### Prerequisites
- Docker Engine 20.10+
- Docker Compose 2.0+
- 24 GB VRAM (for Qwen 3.5:27B) or adjust model size
- Linux host (Arch, Debian, Ubuntu)

### Quick Start

```bash
# Clone and launch the full stack
git clone https://github.com/Dinaverse/local-ai-sovereign-stack
cd local-ai-sovereign-stack

# Start all services
docker compose up -d

# Verify services
docker compose ps
```

### Container Health Checks

```bash
# View logs
docker compose logs -f ollama
docker compose logs -f grafana
docker compose logs -f prometheus

# Container stats
docker stats
```

---

## 📊 Monitoring & Access

| Service | URL | Purpose | Access |
|---------|-----|---------|--------|
| **Grafana** | http://localhost:3000 | GPU metrics, inference throughput, system health | Default creds: admin/admin |
| **Prometheus** | http://localhost:9090 | Raw metrics & alerting rules | Query interface |
| **Ollama API** | http://localhost:11434 | LLM inference endpoint | REST API |
| **Portainer** | https://localhost:9443 | Docker container management | Web UI |

### Example Ollama Queries

```bash
# List loaded models
curl http://localhost:11434/api/tags

# Run inference
curl -X POST http://localhost:11434/api/generate \
  -H "Content-Type: application/json" \
  -d '{"model": "qwen:27b", "prompt": "Hello"}'

# Check model status
curl http://localhost:11434/api/show -d '{"model": "qwen:27b"}'
```

---

## 🔄 Integration with Lab

| Component | Integration | Details |
|-----------|-------------|---------|
| **Arch-GPU Node** | Preferred host | Optimized CUDA setup, 24 GB VRAM available |
| **n8n Workflows** | Automated orchestration | Triggered by Prometheus alerts or schedules |
| **Sovereign AI Skills** | Custom prompts | Extends Ollama with domain-specific reasoning |
| **Morpheus Pipeline** | Threat detection | Consumes Prometheus metrics for anomaly detection |
| **Security Agents** | Log ingestion | Collect metrics from Prometheus for analysis |

---

## 📁 Directory Structure

```
local-ai-sovereign-stack/
├── README.md                          (this file)
├── docker-compose.yml                 Service definitions
├── .env                               Environment configuration
├── ollama/
│   ├── Dockerfile                     Ollama runtime image
│   └── config/
│       └── modelfile                  Model configuration
├── prometheus/
│   ├── prometheus.yml                 Scrape targets & alerting rules
│   └── alerts.yml                     Alert definitions
├── grafana/
│   ├── provisioning/
│   │   ├── datasources/               Prometheus data source
│   │   └── dashboards/                Pre-built dashboard definitions
│   └── dashboards/
│       ├── ai-metrics.json            LLM inference metrics
│       ├── gpu-performance.json       GPU utilization dashboard
│       └── system-health.json         System-wide health dashboard
└── n8n/
    └── workflows/
        ├── inference-triggers.json    Automated inference workflows
        └── alert-response.json        Alert-driven workflows
```

---

## 🔗 Related Repositories

| Repository | Purpose | Connection |
|------------|---------|-----------|
| **[sovereign-ai-infrastructure](https://github.com/Dinaverse/sovereign-ai-infrastructure)** | Architecture documentation | Central docs hub |
| **[arch-linux-multi-gpu-llm](https://github.com/Dinaverse/arch-linux-multi-gpu-llm)** | GPU cluster optimization | Hosts this stack |
| **[sovereign-ai-skills](https://github.com/Dinaverse/sovereign-ai-skills)** | AI skill extensions | Enhances Ollama prompts |
| **[sovereign-ai-security](https://github.com/Dinaverse/sovereign-ai-security)** | Morpheus integration | Consumes Ollama output |
| **[n8n-automation-hub](https://github.com/Dinaverse/n8n-automation-hub)** | Workflow definitions | Orchestrates services |
| **[cybersecurity-lab-automation](https://github.com/Dinaverse/cybersecurity-lab-automation)** | Security automation | Uses Prometheus metrics |

---

## 🚀 Advanced Configuration

### Custom Model Deployment

```bash
# Pull a different model
docker compose exec ollama ollama pull mistral:7b

# Set as default
docker compose exec ollama ollama set-default mistral:7b
```

### Prometheus Custom Metrics

Add custom scrape targets in `prometheus/prometheus.yml`:

```yaml
scrape_configs:
  - job_name: 'ollama'
    static_configs:
      - targets: ['localhost:11434']
  - job_name: 'custom-agent'
    static_configs:
      - targets: ['localhost:9100']
```

### Scale to Multiple Hosts

Modify `docker-compose.yml` to deploy on multiple nodes:

```yaml
services:
  ollama:
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: all
              capabilities: [gpu]
```

---

## 📖 Documentation

- **[System Design](docs/SYSTEM_DESIGN.md)** — Architecture deep-dive
- **[GPU Optimization](../arch-linux-multi-gpu-llm/README.md)** — CUDA tuning & layer offloading
- **[Orchestration Guide](../sovereign-ai-infrastructure/orchestration/README.md)** — Lab-wide operations

---

## ⚙️ Troubleshooting

### GPU Not Detected
```bash
# Check NVIDIA drivers
docker compose exec ollama nvidia-smi

# Verify CUDA availability
docker compose exec ollama ollama run qwen:27b "nvidia-smi"
```

### Out of Memory
```bash
# Reduce model size or enable layer offloading
# Edit docker-compose.yml environment variables
```

### Prometheus Not Scraping
```bash
# Check targets
curl http://localhost:9090/api/v1/targets
```

---

*Local by choice. Sovereign by design. Zero cloud, full control.*
