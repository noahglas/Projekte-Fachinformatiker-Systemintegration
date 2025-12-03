# Projekte-Fachinformatiker-Systemintegration
Dieses Repository enthält eine Sammlun IT-Projekten, die meine Fähigkeiten in den Bereichen **Netzwerk**, **Monitoring**, **Automatisierung**, **Serverdienste**, **Sicherheit** und **Systemadministration** zeigen.  

# VPN + Pi-Hole Setup (Raspberry Pi 5)

## 🔍 Kurzbeschreibung
Dieses Projekt implementiert ein kombiniertes System aus **WireGuard VPN** und **Pi-Hole DNS-Filtering**, um einen sicheren Remote-Zugriff ins Heimnetz zu ermöglichen und gleichzeitig Werbung und Tracking auf allen verbundenen Geräten zu blockieren. Die Lösung läuft auf einem Raspberry Pi 5 und wurde auf Sicherheit, Stabilität und einfache Wartbarkeit ausgelegt.

---

## 🎯 Ziele des Projekts
- Aufbau eines sicheren WireGuard-VPN-Servers für Remote-Zugriff auf das Heimnetz
- Integration von Pi-Hole als zentralem DNS-Filter für alle VPN- und LAN-Clients
- Verständnis für Netzwerksicherheit, DNS-Architektur und Portweiterleitung vertiefen
- Dokumentierte Einrichtung und Testverfahren für ein professionelles Portfolio
- Sichere und wartbare Konfiguration mit klarer Struktur

---

## 🧰 Verwendete Technologien & Tools
- **Hardware:** Raspberry Pi 5 (4–8 GB)  
- **Betriebssystem:** Raspberry Pi OS Lite (Debian-basiert)  
- **Dienste:**  
  - *WireGuard* – moderner, sicherer VPN-Dienst  
  - *Pi-Hole* – DNS-Filter gegen Werbung & Tracking  
- **Containerisierung:** Docker & Docker Compose  
- **Netzwerk:** DNS, DHCP, Portforwarding, Firewall (iptables)  
- **Skripte:** Bash für Management & Diagnose  

---

## 🛠️ Architektur & Aufbau

### Systemübersicht
- Der Raspberry Pi fungiert sowohl als **VPN-Gateway** als auch als **DNS-Resolver**.  
- Clients verbinden sich über WireGuard und erhalten automatisch Pi-Hole als DNS-Server.  
- Das Heimnetz bleibt über das VPN erreichbar (NAS, Monitoring, Web-UI etc.).  
- Der Router leitet den WireGuard-Port (UDP 51820) an den Pi weiter.

### Diagramm

