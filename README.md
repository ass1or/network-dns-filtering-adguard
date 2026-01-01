#  Home Lab: Network Traffic Filtering & Privacy Protection

## Project Overview
This project implements a network-wide ad-blocking and tracking protection solution using **AdGuard Home** deployed via **Docker** on a **Linux Server**.

The goal was to create a centralized DNS sinkhole that filters malicious traffic and telemetry for all devices in the local network (IoT, Smartphones, Workstations), enforcing privacy policies at the infrastructure level.

##  Infrastructure & Architecture

* **Hardware:** Lenovo ThinkCentre (Home Server)
* **OS:** Ubuntu Server LTS
* **Network Core:** Mikrotik Router (RouterOS)
* **Deployment:** Docker & Docker Compose (Infrastructure as Code)

### Network Topology
The server acts as the primary DNS resolver for the local network. The Mikrotik router distributes the server's IP via DHCP and enforces usage via **DST-NAT rules**, preventing devices from bypassing the filter (DNS Leaks protection).

##  Tech Stack

* ![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white) **Docker & Docker Compose** - For containerized, reproducible deployment.
* ![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black) **Ubuntu Server** - Headless host operating system.
* ![AdGuard](https://img.shields.io/badge/AdGuard-68BC71?style=for-the-badge&logo=adguard&logoColor=white) **AdGuard Home** - DNS Sinkhole and network monitoring tool.
* ![Mikrotik](https://img.shields.io/badge/MikroTik-C23C27?style=for-the-badge&logo=mikrotik&logoColor=white) **RouterOS** - Advanced networking (VLANs, NAT, DHCP).

##  Configuration

### Docker Compose
The service is defined using `docker-compose.yml` for easy orchestration and updates.

```yaml
version: "3"
services:
  adguardhome:
    image: adguard/adguardhome
    container_name: adguardhome
    restart: unless-stopped
    volumes:
      - ./workdir:/opt/adguardhome/work
      - ./confdir:/opt/adguardhome/conf
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80/tcp"


Network Hardening (Mikrotik)
To ensure all traffic is filtered, I implemented a Destination NAT rule on the Mikrotik router to intercept any DNS requests (port 53) trying to bypass the local server (e.g., hardcoded Google DNS 8.8.8.8 on IoT devices) and redirect them forcibly to AdGuard.

 Results & Monitoring
(Place for your screenshot from AdGuard Dashboard)

Dashboard showing blocked tracking requests and network activity analysis.

 Key Takeaways
Deployed a containerized service on a headless Linux server.

Configured static networking and DHCP leases.

Analyzed network traffic logs to identify unauthorized telemetry from mobile and IoT devices.

Implemented "Man-in-the-Middle" DNS redirection on the router level.
