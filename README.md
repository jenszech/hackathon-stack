# 🧩 Hackathon Stack – Lokale Installation & Serverbetrieb

Dieses Repository enthält alle notwendigen Informationen für die lokale Installation und den Betrieb des Hackathon Managers. Es umfasst Docker-Setup-Dateien, Konfigurationen und CI/CD-Pipelines. Traefik wird als optionale Alternative für Routing und TLS-Verschlüsselung beschrieben.

---

## 📦 Inhaltsverzeichnis

- [Architekturübersicht](#architekturübersicht)
- [Lokale Installation](#lokale-installation)
  - [Docker-Setup](#docker-setup)
  - [Starten der Umgebung](#starten-der-umgebung)
- [Optionale Alternative: Traefik](#optionale-alternative-traefik)
- [CI/CD Workflows](#cicd-workflows)
  - [Build Images](#build-images)
  - [Manual Deployment](#manual-deployment)
- [Konfiguration](#konfiguration)
  - [Frontend](#frontend-konfiguration)
  - [Backend](#backend-konfiguration)
- [Weiterführende Links](#weiterführende-links)

---

## 📦 Architekturübersicht

```text
            +----------------------------+
            |     Docker Container       |
            +----------------------------+
               |                |
   http://localhost/api       http://localhost/
     (Backend/API)             (Frontend)
       |                          |
+----------------+      +------------------------+
|  hackathon-api |      |  hackathon-frontend    |
|  Node.js (3005)|      |  NGINX (80) + SPA App  |
+----------------+      +------------------------+
```

---

## 🚀 Lokale Installation

### Docker-Setup

Die lokale Umgebung besteht aus folgenden Docker-Containern:

- **Backend**: Node.js-API, erreichbar unter `http://localhost:3005`.
- **Frontend**: NGINX-Server, erreichbar unter `http://localhost`.
- **Dozzle** (optional): Log Viewer, erreichbar unter `http://localhost:8081`.

Details zur Konfiguration und den Docker-Compose-Dateien findest du in [local_deployment.md](./docs/local_deployment.md).

### Starten der Umgebung

```bash
docker compose up --build
```

Stoppen der Umgebung:

```bash
docker compose down
```

---

## 🛠️ Optionale Alternative: Traefik

Traefik kann als Reverse Proxy für Routing und TLS-Verschlüsselung verwendet werden. Die Konfiguration ist in [production_deploy.md](./docs/production_deploy.md) beschrieben. Diese Option ist für produktive Umgebungen geeignet.

---

## ⚙️ CI/CD Workflows

### Build Images

Der Workflow `build.yml` erstellt Docker-Images für das Frontend und Backend und pusht sie zur GitHub Container Registry (`ghcr.io`). Details findest du in [workflows.md](./docs/workflows.md).

### Manual Deployment

Der Workflow `deploy.yml` ermöglicht das manuelle Deployment per SSH auf einem Zielserver. Details findest du ebenfalls in [workflows.md](./docs/workflows.md).

---

## 🛠️ Konfiguration erstellen und Volumes verwenden

### Konfiguration erstellen

Die Konfigurationsdateien für Backend und Frontend befinden sich im `volumes/config`-Verzeichnis. Kopiere die Beispiel-Dateien und passe sie an deine Umgebung an:

#### Backend-Konfiguration
```bash
cp ./backend/volumes/config/.env.example ./backend/volumes/config/.env.dev
```
Bearbeite die Datei `./backend/volumes/config/.env.dev`, um die Umgebungsvariablen für die lokale Entwicklung zu konfigurieren. Details zu den Variablen findest du in [config_backend.md](./docs/config_backend.md).

#### Frontend-Konfiguration
```bash
cp ./frontend/volumes/config/.env.example ./frontend/volumes/config/.env.dev
```
Bearbeite die Datei `./frontend/volumes/config/.env.dev`, um die Umgebungsvariablen für die lokale Entwicklung zu konfigurieren. Details zu den Variablen findest du in [config_frontend.md](./docs/config_frontend.md).

### Verwendung des Volumes-Verzeichnisses

Das `volumes`-Verzeichnis wird genutzt, um Konfigurationsdateien, Datenbanken und andere persistente Daten bereitzustellen. Es wird in den Docker-Containern als Bind-Mount eingebunden.

#### Backend
- **Konfigurationsdateien:** `./backend/volumes/config/`
- **Datenbank:** `./backend/volumes/database/`

#### Frontend
- **Konfigurationsdateien:** `./frontend/volumes/config/`
- **NGINX-Konfiguration:** `./frontend/volumes/nginx/`

Die Dateien im `volumes`-Verzeichnis werden automatisch von den Containern geladen und ermöglichen eine einfache Anpassung der Umgebung.

---

## ⚙️ Weiterführende Links

- [Docker Docs](https://docs.docker.com/)
- [Traefik Docs](https://doc.traefik.io/traefik/)
- [GitHub Container Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry)
- [Hackathon-Manager Repository](https://github.com/jenszech/hackathon-manager)