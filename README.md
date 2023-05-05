# Hands-On-Szenario SSH-Dienst

## Vorbereitung

1. [VirtualBox](https://www.virtualbox.org/) installieren
2. VMDK Abbild importieren

## Die Hands-On Aufgaben
1. Einrichten eines SSH-Servers unter Ubuntu
2. Verbinden und Authentifizieren via Kennwort
	- Hinterlegen des eigenen Public Keys auf dem SSH-Server
3. Verbinden und Authentifizieren via SSH-Keys
4. Upload und Download von Dateien via SCP (Secure Copy)
5. Ein graphisches Programm über SSH remote nutzen (Weiterleiten eines 
   X11-Fensters)


## Hands-On Guide
---
Die Credentials für die Ubuntu VM lauten wie folgt: 

|Username|Passwort|
|--------|--------|
|user    |pass    |

---

 Ihre erste Aufgabe besteht darin, einen SSH-Dienst zu konfigurieren

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
geschlossener SSH-Port ist oft Grund dafür, dass SSH-Verbindungen nicht 
herstellbar sind. 

Mit folgendem Befehl kann der SSH-Port unter Ubuntu freigegeben werden: 

```bash
sudo ufw allow ssh
```

### 4. Konfiguration

Die Basiskonfiguration ist bestens geeignet für dieses Hands-On. Sollten Sie 
dennoch die Konfiguration anpassen wollen - diese finden Sie unter 
`/etc/ssh/sshd_config`. Sie müssen diese mit "Root"-Rechten (Admin-Rechte in der 
Microsoft-Welt) öffnen und nutzen dafür am besten den Terminal-Texteditor `nano`. 
Sollten Sie nicht neu in der Bash/Unix-Welt sein und den Texteditor Vi/Vim/Neovim
bereits beherrschen, können Sie natürlich `vim` benutzen. 

```bash
sudo nano /etc/ssh/sshd_config
```
oder 
```bash
sudo vim /etc/ssh/sshd_config
```

Wenn Sie all Ihre Konfiguration fertiggestellt haben, speichern Sie die Datei und 
starten mit folgendem Systemd-Befehl den SSH-Dienst neu: 

```bash
sudo service ssh restart
``` 

---

Ihre Zweite Aufgabe besteht darin, eine SSH-Sitzung mit der Ubuntu-VM 
herzustellen. Authentifzieren Sie sich über das Kennwort. 

### 1. IP-Adresse der VM herausfinden

