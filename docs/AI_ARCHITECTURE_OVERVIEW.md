# Architecture du Stack IA Souverain

## Vue d'ensemble
Cette architecture permet l'exécution de modèles de langage (LLM) complexes (type Qwen 27B) localement, sans aucune dépendance au cloud, garantissant une souveraineté totale sur les données.

## 1. Moteur d'Inférence (Backend) : Ollama
Ollama agit comme le serveur d'inférence central.
- **Rôle :** Expose une API REST locale sur le port `11434`.
- **Accélération :** Tire parti de la pile CUDA pour déléguer les calculs tensoriels aux GPU NVIDIA.
- **Modèles :** Gestion dynamique (chargement/déchargement) des poids des modèles (Qwen, Llama3) en VRAM.

## 2. Orchestration & Isolation : Docker
L'interface utilisateur et les services annexes sont conteneurisés pour une gestion propre et répétable.
- **Isolation :** Chaque service (UI, Monitoring) tourne dans un conteneur dédié, sans impacter l'hôte.
- **Persistance :** Utilisation de volumes Docker pour garantir que les données (logs, bases de données) survivent aux redémarrages.
- **Communication :** Les conteneurs communiquent via un réseau Docker interne ou directement avec l'API Ollama de l'hôte via l'IP réseau local.

## 3. Pont d'Intégration (API Bridge)
La liaison entre les outils de sécurité (scripts Python/Kali) et le stack IA est assurée par un pont logiciel :
- **Serveur MCP :** Un orchestrateur (`mcp-security-server.js`) fait le lien entre les modèles et les outils de sécurité externes (nmap, nuclei, etc.).
- **Python Bridge :** Des scripts Python utilisent la bibliothèque `requests` pour interroger l'API Ollama, transformant ainsi les résultats de scan en insights intelligibles pour l'utilisateur.

## 4. Sécurité et Souveraineté
- **Confidentialité :** Zéro appel externe. L'inférence est 100% locale.
- **Auditabilité :** Les logs d'inférence et d'orchestration sont centralisés dans le cluster de monitoring (Prometheus/Grafana).
EOF
,file_path: