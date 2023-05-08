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

Die IP-Adresse finden Sie mit `ip a` heraus. Hier können Sie unter dem Netzwerkadapter
die IP-Adresse sehen.

### 2. SSH-Verbindung herstellen

Die SSH Session starten Sie mit `ssh user@machine`. Sie werden nach einer Bestätigung
und einem Passwort des Benutzers gefragt.

---

Für Ihre dritte Aufgabe hinterlegen sie ihren public SSH-Key auf dem SSH-Server

### 3. SSH-Keys generieren und auf dem Server hinterlegen

Für die Authentifizierung via SSH-Keys müssen diese Schlüssel vorhanden sein.
Mit dem Befehl `ssh-keygen` können Sie SSH-Keys generieren lassen.

Nach dem generieren können Sie mit `ssh-copy-id` den SSH public key hinterlegen.

---

Probieren Sie noch einmal, sich per SSH auf dem Server einzuloggen. Sie werden bemerken,
dass Sie *nicht* nach einem Kennwort gefragt werden.

---

Für die vierte Aufgabe laden Sie Dateien hoch/runter von der Ubuntu VM via SCP,
einem Programm für "remote copying" via SSH.

### 1. Datei herunterladen

Suchen Sie sich eine Datei im Ordner "Dokumente" auf der VM aus, die sie herunterladen
wollen.

FÜhren Sie dann den Befehl `scp username@machine:/home/username/Dokumente/file.txt ~/Desktop` aus. Die
Datei taucht dann auf dem Desktop auf.

### 2. Datei hochladen

Für den Upload können wir das gleiche Programm nutzen. Dazu führen Sie folgenden Befehl aus:
`scp file.txt username@machine:/home/username/Dokumente`

---

Wenn Sie die bisherigen Aufgaben zu leicht/nicht tiefgehend genug finden und wenn Sie zur Zeit
einen Linux-PC nutzen, **leiten Sie ein xorg-Fenster über SSH an ihre Host-Mashcine**. 

Um dies bereitzustellen können Sie sich hier zum Thema [xorg-forwarding](https://www.baeldung.com/linux/forward-x-over-ssh)
einlesen. 

