# Ansible VPS Bootstrap & Deployment Verification

![Ansible](https://img.shields.io/badge/Automation-Ansible-red)
![Linux](https://img.shields.io/badge/Platform-Linux-blue)
![IaC](https://img.shields.io/badge/Infrastructure-IaC-green)


## Projektbeschreibung

Dieses Repository enthält Ansible-Playbooks und Rollen zur reproduzierbaren Bereitstellung einer Linux-Infrastruktur sowie zur automatisierten Verifikation produktionsnaher Services.

Das Projekt ist bewusst als infrastrukturelle Ergänzung zur [Device Inventory API](https://github.com/pavel-sharipov/device-inventory-api) aufgebaut und folgt derselben Portfolio-Logik: Während die API Backend-Entwicklung, Security, Monitoring und Deployment im Anwendungskontext demonstriert, zeigt dieses Repository die Infrastruktur- und Automatisierungsseite eines produktionsnahen Betriebs.

## Repository Highlights

- Infrastruktur-Automatisierung mit Ansible
- Deployment einer containerisierten Anwendung (Device Inventory API + PostgreSQL)
- Automatisierte Verifikation zentraler Betriebsdienste
- Bastion-/Proxy-basierter SSH-Zugriff
- Trennung zwischen Bootstrap, Deployment und Verifikation

## Ziel des Projekts

- Wiederholbares Server-Setup statt manueller Einzelkonfiguration
- Docker-basierte Bereitstellung einer containerisierten Anwendung
- Automatisierte Healthchecks nach Deployment
- Vorbereitung für spätere Erweiterungen im Bereich Hardening, CI/CD und Monitoring

## Technische Motivation

In realen Umgebungen entstehen viele Probleme nicht im Anwendungscode, sondern bei inkonsistenten Serverzuständen, fehlenden Betriebsprüfungen oder nicht dokumentierten Deployments.

Dieses Projekt demonstriert den praktischen Einsatz von Infrastructure as Code mit Ansible in einer reproduzierbaren Linux-Umgebung.

## Verwendeter Technologie-Stack

**Infrastructure & Automation**

- Ansible
- YAML
- Linux (Debian/Ubuntu)
- SSH
- UFW
- Fail2ban

**Container & Deployment**

- Docker
- Docker Compose
- PostgreSQL
- Spring Boot (Device Inventory API als Zielanwendung)

## Betriebsprüfung nach Deployment

- Nginx
- Prometheus
- Grafana

## Architektur


## Architekturüberblick

Das Projekt ist rollenbasiert aufgebaut:

- `common` → Baseline-Konfiguration (Pakete, Firewall, Zeitzone)
- `docker` → Docker Engine und Compose Installation
- `deploy_device_inventory` → Deployment der Device Inventory API inkl. PostgreSQL
- `deploy_app` → Applikations- und Container-Healthchecks
- `monitoring` → Monitoring-Endpunkt-Prüfungen
- `nginx` → Reverse-Proxy- und Konfigurationsprüfung
- `security` → Fail2ban-Verifikation

## Implementierte Funktionen

### Baseline / Host Setup

- APT Cache Update
- Installation grundlegender Pakete
- UFW aktivieren
- SSH-Zugriff absichern
- Zeitzone setzen

### Container Deployment

- Kopieren von `docker-compose.yml`
- Bereitstellung einer konfigurierbaren `.env`-Datei
- Docker Image Pull
- Start des Stacks via Compose
- Healthcheck gegen `/actuator/health`

### Service- und Infrastrukturprüfung

- Prüfung von Prometheus (`/-/healthy`)
- Prüfung von Grafana (`/api/health`)
- Nginx Service-Status + `nginx -t`
- HTTPS-Healthcheck über Reverse Proxy

## Sicherheitsaspekte

- Firewall-Basis mit UFW
- Fail2ban-Prüfung integriert
- Healthchecks als frühe Fehlererkennung
- Vorbereitung für Secret-Trennung (`.env.example` statt produktiver Werte)

## Automatisierung / Deployment / Infrastruktur

Das Projekt trennt bewusst Deployment und Verifikation.

Aktuell enthalten:

- reproduzierbare Ausführung via Playbooks
- Smoke Checks mit `uri`
- SSH-Zugriff über Bastion-/Proxy-Struktur

## Bezug zur Device Inventory API

Beide Projekte bilden zusammen ein vollständigeres Portfolio:

- **Device Inventory API** → Backend Engineering
- **Ansible VPS Bootstrap** → Infrastruktur & Deployment

Dadurch wird ein End-to-End-Verständnis gezeigt: Anwendung + Betrieb.

## Projektstruktur

```text
.
├── ansible.cfg
├── inventory/
│   └── hosts.yml
├── site.yml
├── homelab_bootstrap.yml
├── production_verify.yml
└── roles/
    ├── common/
    │   └── tasks/main.yml
    ├── docker/
    │   └── tasks/main.yml
    ├── deploy_device_inventory/
    │   ├── files/
    │   │   ├── docker-compose.yml
    │   │   └── .env.example
    │   └── tasks/main.yml
    ├── deploy_app/
    │   └── tasks/main.yml
    ├── monitoring/
    │   └── tasks/main.yml
    ├── nginx/
    │   └── tasks/main.yml
    └── security/
        └── tasks/main.yml
```

## Lokaler Start / Playbook-Ausführung

**Voraussetzungen**

- Python 3
- Ansible
- SSH-Zugriff auf Zielhost
- vorbereitete Inventory-Datei

## Lokale Ausführung

```bash
ansible-playbook -i inventory/hosts.yml homelab_bootstrap.yml
```

```bash
ansible-playbook -i inventory/hosts.yml production_verify.yml
```

## Projektstatus

- Rollenbasierte Projektstruktur
- Docker-basierter Deployment-Ablauf
- Healthchecks nach Deployment
- Service-Verifikation für Monitoring und Reverse Proxy

## Geplante Erweiterungen

- Molecule Tests
- Variablenstruktur über group\_vars
- Ansible Vault für Secrets
- Compose v2 / community.docker Module

## Screenshots / Dokumentation

Empfohlene Ergänzungen für die Repository-Dokumentation:

- erfolgreicher Ansible-Run (Play recap)
- Healthcheck Response der Anwendung
- Architekturdiagramm als PNG oder extern erstellt

