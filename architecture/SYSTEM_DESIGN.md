# Sovereign AI Infrastructure - System Design

## Overview
This laboratory is built as a decentralized, modular, and cloud-agnostic platform. It provides high-compute capabilities for AI model inference (LLM), automated security reconnaissance, and resilient network services, maintaining full data sovereignty.

## Architectural Layers

### 1. Orchestration Layer (Control Plane)
*   **Host:** Kali Station (Local)
*   **Role:** Acts as the primary orchestrator for the entire lab. It handles CI/CD workflows, source code management (via GitHub CLI), and administrative control over other nodes via SSH.

### 2. Compute Layer (AI/ML Engine)
*   **Host:** Arch Cluster (4x NVIDIA P106-100)
*   **Role:** Distributed inference and model training.
*   **Implementation:** Leverages Ollama for LLM runtime, utilizing 24GB of VRAM for distributed inference of large models (e.g., Qwen 27B). The system is optimized with kernel-level tuning for high throughput.

### 3. Observability Layer (Monitoring)
*   **Host:** Dell Precision (Gateway/Orchestrator)
*   **Role:** Centralized monitoring of the infrastructure.
*   **Implementation:** Docker-Compose stack running Prometheus for metric collection and Grafana for visual dashboards, monitoring node health across the lab.

### 4. Network/Security Layer (Edge)
*   **Host:** Raspberry Pi
*   **Role:** Defensive infrastructure and network resilience.
*   **Implementation:** Runs Pi-hole for DNS security and serves as a sensor for Intrusion Detection Systems (IDS).

### 5. Storage Layer (Data Management)
*   **Host:** Canwork189 (AMD Worker)
*   **Role:** General purpose compute and centralized storage.
*   **Implementation:** Dedicated `/local` partition provides high-capacity storage for datasets, logs, and backups across the infrastructure.

## Component Interaction
- **SSH Backbone:** All communication is secured via a master key infrastructure, configured for persistence and stability.
- **Agent Framework:** Autonomous agents (`Security-Ops`, `Net-Analyzer`, `R&D`) communicate across the network to provide security oversight and automated recon, reporting back to central logging.
- **Dockerization:** Container-based deployment ensures environment consistency across different Linux distributions (Arch, Ubuntu, Debian).

## Security & Sovereignty
- **Cloud-Agnostic:** No reliance on public cloud APIs for core infrastructure.
- **Isolation:** Critical infrastructure nodes are isolated from external traffic, managed through internal networking.
- **Auditability:** All agent activities are logged locally and mirrored to central monitoring.
EOF
,file_path: