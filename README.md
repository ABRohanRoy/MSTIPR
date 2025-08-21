# MSTIP Project

Flask -> Docker -> Jenkins -> Kubernetes pipeline

## Ports
- Flask app: 5000
- Local Docker test: 8080 -> 5000
- Jenkins: 8081
- Kubernetes NodePort: 30080 -> 5000

## Quick Start
docker build -t myuser/mstip-flask:latest .
docker run -p 8080:5000 myuser/mstip-flask:latest
