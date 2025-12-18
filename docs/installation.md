📄 STEP: Create installation.md

From your project root:

nano docs/installation.md


Now paste everything below ⬇️

# Linux Monitoring Stack — Installation Guide

## Overview
This guide documents the installation and configuration of a Linux monitoring stack using:

- Prometheus
- Node Exporter
- Alertmanager
- Grafana

The setup reflects real-world Linux system administration practices and is compatible with:
- Ubuntu 22.04+
- WSL (systemd enabled)

---

## Prerequisites

- Linux system with systemd
- sudo privileges
- Internet access
- Ports available:
  - 3000 (Grafana)
  - 9090 (Prometheus)
  - 9093 (Alertmanager)
  - 9100 (Node Exporter)

---

## Directory Structure

Project files are stored under:



~/projects/linux-monitoring-stack


Binaries are installed to:



/usr/local/bin


---

## Component Versions

| Component       | Version |
|----------------|---------|
| Prometheus     | 2.48.0  |
| Node Exporter  | 1.7.0   |
| Alertmanager   | 0.27.0  |
| Grafana        | Latest  |

---

## Installation Steps

### 1. Install Node Exporter

```bash
sudo useradd --no-create-home --shell /bin/false node_exporter


Download and extract Node Exporter:

wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
tar xvf node_exporter-1.7.0.linux-amd64.tar.gz
sudo mv node_exporter-1.7.0.linux-amd64/node_exporter /usr/local/bin/


Create systemd service:

sudo nano /etc/systemd/system/node_exporter.service

[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target


Enable and start:

sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter


Verify:

curl http://localhost:9100/metrics

2. Install Prometheus

Create user:

sudo useradd --no-create-home --shell /bin/false prometheus


Download and extract:

wget https://github.com/prometheus/prometheus/releases/download/v2.48.0/prometheus-2.48.0.linux-amd64.tar.gz
tar xvf prometheus-2.48.0.linux-amd64.tar.gz
sudo mv prometheus-2.48.0.linux-amd64/prometheus /usr/local/bin/


Configuration file location:

~/projects/linux-monitoring-stack/prometheus/prometheus.yml


Create service:

sudo nano /etc/systemd/system/prometheus.service

[Unit]
Description=Prometheus
After=network.target

[Service]
User=prometheus
ExecStart=/usr/local/bin/prometheus \
  --config.file=/home/roy/projects/linux-monitoring-stack/prometheus/prometheus.yml

[Install]
WantedBy=multi-user.target


Enable and start:

sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus


Verify:
👉 http://localhost:9090/targets

3. Install Alertmanager

Create user:

sudo useradd --no-create-home --shell /bin/false alertmanager


Download and extract:

wget https://github.com/prometheus/alertmanager/releases/download/v0.27.0/alertmanager-0.27.0.linux-amd64.tar.gz
tar xvf alertmanager-0.27.0.linux-amd64.tar.gz
sudo mv alertmanager-0.27.0.linux-amd64/alertmanager /usr/local/bin/


Config file location:

~/projects/linux-monitoring-stack/alertmanager/alertmanager.yml


Create service:

sudo nano /etc/systemd/system/alertmanager.service

[Unit]
Description=Alertmanager
After=network.target

[Service]
User=alertmanager
ExecStart=/usr/local/bin/alertmanager \
  --config.file=/home/roy/projects/linux-monitoring-stack/alertmanager/alertmanager.yml

[Install]
WantedBy=multi-user.target


Enable and start:

sudo systemctl daemon-reload
sudo systemctl enable alertmanager
sudo systemctl start alertmanager


Verify:
👉 http://localhost:9093

4. Install Grafana
sudo apt update
sudo apt install -y grafana
sudo systemctl enable grafana-server
sudo systemctl start grafana-server


Access:
👉 http://localhost:3000

(Default login: admin / admin)

Port Reference
Component	Port
Prometheus	9090
Node Exporter	9100
Alertmanager	9093
Grafana	3000
Verification Checklist

 Node Exporter metrics load

 Prometheus targets show UP

 Grafana dashboards render

 Alertmanager UI accessible

Notes

All binaries are stored in /usr/local/bin

Configuration files are version-controlled in this repository

systemd service files in the repo are reference copies
