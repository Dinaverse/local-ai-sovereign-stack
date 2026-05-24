# Sovereign AI Stack Architecture

## Overview
This architecture enables local execution of Large Language Models (LLMs) (e.g., Qwen 27B) without cloud dependencies, ensuring complete data sovereignty.

## 1. Inference Engine (Backend): Ollama
Ollama serves as the central inference server.
- **Role:** Exposes a local REST API on port `11434`.
- **Acceleration:** Leverages CUDA to delegate tensor calculations to NVIDIA GPUs.
- **Models:** Dynamic management (loading/unloading) of model weights (Qwen, Llama3) in VRAM.

## 2. Orchestration & Isolation: Docker
UI and ancillary services are containerized for clean, repeatable management.
- **Isolation:** Each service (UI, Monitoring) runs in a dedicated container, without impacting the host.
- **Persistence:** Uses Docker volumes to ensure data (logs, databases) survives restarts.
- **Communication:** Containers communicate via an internal Docker network or directly with the host Ollama API via local network IP.

## 3. Integration Bridge (API Bridge)
The link between security tools (Python scripts/Kali) and the AI stack is ensured by a software bridge:
- **MCP Server:** An orchestrator (`mcp-security-server.js`) bridges models and external security tools (nmap, nuclei, etc.).
- **Python Bridge:** Python scripts use the `requests` library to query the Ollama API, translating scan results into human-readable insights.

## 4. Security & Sovereignty
- **Confidentiality:** Zero external calls. Inference is 100% local.
- **Auditability:** Inference and orchestration logs are centralized in the monitoring cluster (Prometheus/Grafana).
