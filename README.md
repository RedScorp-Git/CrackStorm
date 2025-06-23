# CrackStorm - Distributed Hashcat Cluster Manager

![CrackStorm Logo](https://img.shields.io/badge/CrackStorm-v1.0-990000?style=for-the-badge)

**CrackStorm** ist ein professionelles Tool für das verteilte Passwort-Cracking mit Hashcat über mehrere Rechner im lokalen Netzwerk. Entwickelt für Cybersecurity-Studium und Penetrationstests.

## 🚀 Features

- **Distributed Computing**: Verteilt Hashcat-Jobs automatisch über mehrere Clients
- **Real-time Monitoring**: Live-Status aller verbundenen Clients
- **GPU & CPU Support**: Automatische Erkennung von NVIDIA/AMD GPUs
- **Load Balancing**: Intelligente Arbeitsverteilung basierend auf Skip/Limit
- **Interactive Shell**: Benutzerfreundliche Kommandozeile für Job-Management
- **Robust Networking**: Automatische Wiederverbindung und Heartbeat-Monitoring
- **Professional UI**: Farbcodierte Ausgaben mit CrackStorm-Branding

## 📋 Voraussetzungen

### Host-System (Server)
- Python 3.6+
- Required packages: `psutil`, `socket`, `threading`

### Client-Systeme
- Python 3.6+
- **Hashcat** installiert und im PATH verfügbar
- Required packages: `psutil`, `socket`, `subprocess`

## 🔧 Installation

### 1. Repository klonen
```bash
git clone https://github.com/scorp/crackstorm.git
cd crackstorm
```

### 2. Dependencies installieren
```bash
pip install psutil
```

### 3. Hashcat installieren (nur auf Clients)

#### Ubuntu/Debian:
```bash
sudo apt update
sudo apt install hashcat
```

#### Windows:
1. Download von [hashcat.net](https://hashcat.net/hashcat/)
2. Zu PATH hinzufügen

#### macOS:
```bash
brew install hashcat
```

## 🚀 Nutzung

### Host starten (Server)
```bash
python3 host.py
```

Mit spezifischem Port:
```bash
python3 host.py -p 8888
```

### Client verbinden
```bash
python3 client.py <HOST_IP>
```

Mit spezifischem Port:
```bash
python3 client.py <HOST_IP> -p 8888
```

### Hashcat-Test
```bash
python3 client.py --test-hashcat <HOST_IP>
```

## 📖 Kommandos (Interactive Mode)

| Kommando | Beschreibung |
|----------|-------------|
| `status` | Zeigt Cluster-Status und verbundene Clients |
| `crack <hashfile> <wordlist>` | Startet distributed Hashcat-Job |
| `results` | Zeigt alle gefundenen Passwörter |
| `help` | Zeigt Hilfe-Menü |
| `quit` / `exit` | Beendet CrackStorm |

## 🔨 Beispiel-Workflow

### 1. Host starten
```bash
# Terminal 1 (Host-Rechner)
python3 host.py
```

### 2. Clients verbinden
```bash
# Terminal 2 (Client 1)
python3 client.py 192.168.1.100

# Terminal 3 (Client 2)  
python3 client.py 192.168.1.100

# Terminal 4 (Client 3)
python3 client.py 192.168.1.100
```

### 3. Job starten
```bash
CrackStorm> status
CrackStorm> crack hashes.txt rockyou.txt
CrackStorm> results
```

## 📁 Beispiel-Dateien

### Hash-Datei (hashes.txt)
```
5d41402abc4b2a76b9719d911017c592
098f6bcd4621d373cade4e832627b4f6
```

### Test-Wordlist (test_passwords.txt)
```
password
123456
admin
hello
test
```

## 🔐 Unterstützte Hash-Modi

Aktuell unterstützt CrackStorm:
- MD5 (Mode 0) - Standard
- Weitere Modi können einfach konfiguriert werden

**Erweiterung für andere Hash-Modi:**
```python
# In execute_hashcat_job() ändern:
cmd = [
    'hashcat',
    '-m', '1000',  # NTLM
    # oder
    '-m', '1400',  # SHA256
    # etc.
    ...
]
```

## 🛡️ Sicherheitshinweise

⚠️ **WICHTIG: Nur für autorisierte Penetrationstests verwenden!**

- Verwenden Sie CrackStorm nur in kontrollierten Umgebungen
- Stellen Sie sicher, dass Sie die Berechtigung haben, Passwörter zu cracken
- Verwenden Sie starke Netzwerksicherheit in Produktionsumgebungen
- Monitoring des Netzwerkverkehrs empfohlen

## 🐛 Troubleshooting

### Client kann sich nicht verbinden
```bash
# Firewall prüfen
sudo ufw allow 9999

# Netzwerk testen
ping <host_ip>
telnet <host_ip> 9999
```

### Hashcat nicht gefunden
```bash
# Installation prüfen
which hashcat
hashcat --version

# PATH prüfen
echo $PATH
```

### Performance-Optimierung
```bash
# GPU-Status prüfen
nvidia-smi  # NVIDIA
rocm-smi    # AMD

# CPU-Auslastung monitoren
htop
```

## 📊 System-Monitoring

CrackStorm zeigt automatisch:
- Anzahl verbundener Clients
- CPU/GPU-Informationen
- Aktive Jobs
- Netzwerkstatus
- Gefundene Passwörter

## 🔧 Erweiterte Konfiguration

### Custom Hashcat-Parameter
```python
# In client.py - execute_hashcat_job()
cmd = [
    'hashcat',
    '-m', '0',
    '-a', str(attack_mode),
    '--force',           # GPU-Warnings ignorieren
    '--opencl-device-types', '1,2',  # CPU + GPU
    '--workload-profile', '3',       # High Performance
    hash_file,
    wordlist,
    '--skip', str(skip),
    '--limit', str(limit)
]
```

### Netzwerk-Timeouts anpassen
```python
# In host.py
client_socket.settimeout(30)  # Erhöhen für langsamere Netzwerke

# In client.py  
self.socket.settimeout(60)    # Heartbeat-Intervall
```

## 📈 Performance-Tipps

1. **GPU-Optimierung**: Stellen Sie sicher, dass alle Clients GPU-Unterstützung haben
2. **Netzwerk**: Verwenden Sie Gigabit-Ethernet für beste Performance
3. **Wordlists**: Große Wordlists auf lokale SSDs kopieren
4. **RAM**: Mindestens 8GB RAM pro Client empfohlen
5. **Cooling**: Überwachen Sie die Temperaturen bei längeren Jobs

## 🤝 Contributing

1. Fork das Repository
2. Feature-Branch erstellen (`git checkout -b feature/AmazingFeature`)
3. Änderungen committen (`git commit -m 'Add some AmazingFeature'`)
4. Branch pushen (`git push origin feature/AmazingFeature`)
5. Pull Request erstellen

## 📝 Lizenz

Dieses Projekt steht unter der MIT License - siehe [LICENSE](LICENSE) für Details.

## ⚠️ Disclaimer

CrackStorm ist ausschließlich für Bildungszwecke und autorisierte Sicherheitstests entwickelt worden. Die Autoren übernehmen keine Verantwortung für den Missbrauch dieses Tools. Verwenden Sie es nur mit ausdrücklicher Berechtigung auf Systemen, die Sie besitzen oder für die Sie autorisiert sind.

## 🎓 Entwickelt für Cybersecurity-Studium

Dieses Tool wurde speziell für Cybersecurity-Studiengänge entwickelt, um:
- Distributed Computing-Konzepte zu demonstrieren
- Praktische Erfahrungen mit Penetrationstesting-Tools zu sammeln
- Netzwerk-Programmierung zu erlernen
- Professionelle Cybersecurity-Workflows zu verstehen

---

**CrackStorm v1.0** - Distributed Hashcat Cluster Manager  
🔴 Powered by Cybersecurity Education
