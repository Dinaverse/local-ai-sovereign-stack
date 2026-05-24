# Documentation docker-compose.yml

## Architecture du déploiement
Ce fichier orchestre les services de monitoring et d'IA de manière conteneurisée.

## Services inclus
- **Prometheus** : Collecte des métriques système.
- **Grafana** : Visualisation des données de monitoring.
- **Open-WebUI** : Interface pour interagir avec les modèles LLM.

## Configuration
- Utilisation de réseaux bridge pour l'isolation.
- Volumes persistants pour les données de configuration et de monitoring.
