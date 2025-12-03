# Projekte-Fachinformatiker-Systemintegrati


„Automatisiertes Backup-System mit Restic + Raspberry Pi + NAS“


📄 README.md – Automatisiertes Backup-System (Restic + NAS + Raspberry Pi)
# Automatisiertes Backup-System mit Restic (Raspberry Pi 5 + NAS)

## 💡 Kurzbeschreibung
In diesem Projekt habe ich ein automatisiertes, verschlüsseltes Backup-System mit **Restic** aufgebaut.  
Der Raspberry Pi sichert regelmäßig definierte Ordner (z. B. Dokumente, Konfigurationen, wichtige Skripte) auf meinen **NAS-Server**. Die Backups laufen vollständig automatisiert per Cronjob und nutzen die schnelle, effiziente und verschlüsselte Backup-Engine von Restic.

Das Projekt zeigt mein Verständnis von Datensicherheit, Automatisierung und Linux-Systemadministration.

---

## 🎯 Ziele des Projekts
- Einrichten eines zuverlässigen Backup-Systems auf dem Raspberry Pi
- Nutzung eines NAS als sicheren Backup-Speicherort
- Automatisierung via Cronjobs  
- Verschlüsselte Backups mit Restic
- Prüfung der Backups & Dokumentation der Wiederherstellung
- Monitoring: Backup-Erfolg per Logfiles überprüfen

---

## 🧰 Verwendete Technologien & Tools
- **Hardware:** Raspberry Pi 5, NAS-Server
- **Software:**  
  - Restic (verschlüsseltes Backup)
  - Bash-Skripte
  - Cron (Automatisierung)
- **Speicherziel:**  
  - NAS per SMB oder NFS
- **Betriebssystem:** Raspberry Pi OS Lite

---

## 🛠️ Systemarchitektur

### Workflow:
1. Der Raspberry Pi mountet beim Backup das NAS-Verzeichnis.
2. Restic führt ein inkrementelles, verschlüsseltes Backup aus.
3. Der Backup-Prozess wird geloggt („Backup erfolgreich / fehlgeschlagen“).
4. Am Ende wird der NAS-Mount sauber getrennt.
5. Ein Cronjob führt den Prozess regelmäßig aus.

### Architekturdiagramm


┌──────────────┐ Backup ┌──────────────┐
│ Raspberry Pi │ ─────────────▶ │ NAS-Server │
└──────────────┘ └──────────────┘
▲ │
│ ▼
Autom. Jobs Verschlüsselt
(Cron) (Restic)


---

## 🚀 Umsetzung – Schritt für Schritt

### 1. Vorbereitung & Installation
System aktualisieren:
```bash
sudo apt update && sudo apt upgrade -y


Restic installieren:

sudo apt install restic -y

2. NAS mounten

NAS-Verzeichnis erstellen:

sudo mkdir -p /mnt/backup_nas


Zugriff testen (SMB-Beispiel):

sudo mount -t cifs -o username=<NAS_USER>,password=<NAS_PASSWORD> //<NAS-IP>/backup /mnt/backup_nas


Automatisierung via /etc/fstab (optional).

3. Restic Repository initialisieren
export RESTIC_REPOSITORY="/mnt/backup_nas/restic_repo"
export RESTIC_PASSWORD="DEIN_SICHERES_PASSWORT"

restic init

4. Backup-Skript erstellen

backup.sh:

#!/bin/bash

BACKUP_SOURCE="/home/pi"
NAS_MOUNT="/mnt/backup_nas"
RESTIC_REPOSITORY="$NAS_MOUNT/restic_repo"
RESTIC_PASSWORD="DEIN_SICHERES_PASSWORT"
LOGFILE="/var/log/restic-backup.log"

echo "=== Backup gestartet: $(date) ===" >> $LOGFILE

# NAS mounten
mount $NAS_MOUNT 2>> $LOGFILE

# Backup starten
restic backup $BACKUP_SOURCE >> $LOGFILE 2>&1

# Repository prüfen
restic check >> $LOGFILE 2>&1

# Alte Backups automatisch löschen (z. B. 14 Tage)
restic forget --keep-daily 14 --prune >> $LOGFILE 2>&1

# NAS unmounten
umount $NAS_MOUNT

echo "=== Backup abgeschlossen: $(date) ===" >> $LOGFILE
echo "" >> $LOGFILE


Skript ausführbar machen:

chmod +x backup.sh

5. Cronjob einrichten
crontab -e


Täglich um 03:00 Uhr Backup:

0 3 * * * /home/pi/backup.sh


Cron neu laden:

sudo systemctl reload cron

6. Backup testen

Manuell ausführen:

./backup.sh


Ergebnis prüfen:

cat /var/log/restic-backup.log


Beispiel Erfolgseintrag:

=== Backup gestartet: Mon Feb 5 03:00:01 2025 ===
snapshot xxxxxxxxxx saved
repository is consistent
=== Backup abgeschlossen: Mon Feb 5 03:05:12 2025 ===

7. Wiederherstellung testen (Restore)

Backup-Liste anzeigen:

restic snapshots


Snapshot wiederherstellen:

restic restore <SNAPSHOT_ID> --target /home/pi/restore-test


Erfolg prüfen:

ls /home/pi/restore-test

🧪 Testergebnisse

Datum: 05.02.2025

Feature	Ergebnis
Backup läuft automatisch	✔️
Daten werden verschlüsselt gespeichert	✔️
Wiederherstellung erfolgreich getestet	✔️
Log-System zeigt Fehler an	✔️
NAS-Mount stabil	✔️

Beispiel-Snapshot:

ID        Time                 Host        Tags
-------------------------------------------------------
c3c9d0ab  2025-02-05 03:00:06  raspberry   daily

📁 Projektstruktur
backup-system/
├─ README.md
├─ backup.sh
├─ systemd/ (optional)
├─ logs/
│  └─ restic-backup.log
└─ screenshots/

📸 Screenshots (Beispiele)

screenshots/restic-snapshot-list.png

screenshots/restic-restore.png

screenshots/logfile-example.png

🔐 Sicherheit

Backups vollständig AES-256 verschlüsselt

NAS nur im lokalen Netzwerk erreichbar

Restic-Passwort nicht im Repository gespeichert

Logfiles enthalten keine sensiblen Daten

Optional: Passwort über RESTIC_PASSWORD_FILE

⚙️ Automatisierung & Wartung
Logrotate für saubere Logs:
/var/log/restic-backup.log {
    weekly
    rotate 4
    compress
    missingok
    notifempty
}

Monitoring:

Cronjob-Failures per grep -i error

Optional: Prometheus-Exporter für Restic

📘 Lessons Learned

Backup-Konzepte: inkrementell, Verschlüsselung, Snapshotting

Wie wichtig Test-Restores sind

Umgang mit Netzwerk-Filesystemen & Mounts

Automatisierung mit Cronjobs

Strategien zur Aufbewahrung (Retention Policies)

🕒 Zeitaufwand

Installation & Vorbereitung: 1 Stunde

Skripte entwickeln & testen: 2 Stunden

Automatisierung & Debugging: 1,5 Stunden

Wiederherstellungstests: 1 Stunde

Dokumentation: 1 Stunde

Gesamt: ~6,5 Stunden

✔️ Status

Fertig – Version 1.0
Letztes Update: 05.02.2025
