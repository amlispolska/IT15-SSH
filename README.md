# Hands-On-Szenario SSH-Dienst

## Vorbereitung

1. [VirtualBox](https://www.virtualbox.org/) installieren
2. VMDK Abbild importieren

## Hands-On Guide
---
Die Credentials für die Ubuntu VM lauten wie folgt: 

|Username|Passwort|
|--------|--------|
|user    |pass    |

---

Mit folgenden Befehlen können Sie SSH einrichten: 

### 1. SSH-Server installieren

SSH verfolgt eine Client-Server Architektur - im Klartext: 
Kein SSH-Server - Kein SSH!

Also führen Sie folgenden Befehl für die Installation aus: 
```bash
sudo apt install openssh-server
```
*Erklärung*: `sudo` steht für "superuser do" und ist vergleichbar mit "Als
Administrator ausführen". `apt` ist der Paket Manager - sowas wie winget oder der
App Store. `openssh-server` ist das Programm zur Installation. 

### 2. SSH Daemon prüfen und ggf. anschalten

Jetzt muss der Dienst noch laufen - das muss allerdings überprüft und ggf. 
sichergestellt werden. 

Zunächst die Überprüfung: 
```bash
sudo systemctl status ssh
```
*Erklärung*: `systemctl` ist ein Programm zur Interaktion mit Systemd - der
Parameter `status` mit einem Dienstnamen zeigt den Status des Dienstes an: ist der
Dienst gerade am Laufen und ist der Dienst aktiviert? 

Sollte unter "Active: " nicht `active (running)` stehen, führen Sie bitte folgende 
Befehle aus: 
```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

### 3. SSH-Port öffnen

Damit SSH-Verbindungen möglich sind muss der SSH-Port geöffnet werden. Ein 
geschlossener 
