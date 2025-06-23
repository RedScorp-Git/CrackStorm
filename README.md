# CrackStorm - Distributed Hashcat Cluster Manager

![CrackStorm Logo](https://img.shields.io/badge/CrackStorm-v1.0-990000?style=for-the-badge)

**CrackStorm** is a professional tool for distributed password cracking with Hashcat across multiple computers in the local network. Developed for cybersecurity studies and penetration testing.

## 🚀 Features

- **Distributed Computing**: Automatically distributes Hashcat jobs across multiple clients
- **Real-time Monitoring**: Live status of all connected clients
- **GPU & CPU Support**: Automatic detection of NVIDIA/AMD GPUs
- **Load Balancing**: Intelligent work distribution based on Skip/Limit
- **Interactive Shell**: User-friendly command line for job management
- **Robust Networking**: Automatic reconnection and heartbeat monitoring
- **Professional UI**: Color-coded outputs with CrackStorm branding

## 📋 Requirements

### Host System (Server)
- Python 3.6+
- Required packages: psutil, socket, threading

### Client Systems
- Python 3.6+
- **Hashcat** installed and available in PATH
- Required packages: psutil, socket, subprocess

## 🔧 Installation

### 1. Clone repository

```bash
git clone https://github.com/scorp/crackstorm.git
cd crackstorm
```

### 2. Install dependencies

```bash
pip install psutil
```

### 3. Install Hashcat (clients only)

#### Ubuntu/Debian:

```bash
sudo apt update
sudo apt install hashcat
```

#### Windows:
1. Download from [hashcat.net](https://hashcat.net/hashcat/)
2. Add to PATH

#### macOS:

```bash
brew install hashcat
```

## 🚀 Usage

### Start host (server)

```bash
python3 host.py
```

With specific port:

```bash
python3 host.py -p 8888
```

### Connect client

```bash
python3 client.py <HOST_IP>
```

With specific port:

```bash
python3 client.py <HOST_IP> -p 8888
```

### Hashcat test

```bash
python3 client.py --test-hashcat <HOST_IP>
```

## 📖 Commands (Interactive Mode)

| Command | Description |
|----------|-------------|
| status | Shows cluster status and connected clients |
| crack <hashfile> <wordlist> | Starts distributed Hashcat job |
| results | Shows all found passwords |
| help | Shows help menu |
| quit / exit | Exits CrackStorm |

## 🔨 Example Workflow

### 1. Start host

```bash
# Terminal 1 (Host machine)
python3 host.py
```

### 2. Connect clients

```bash
# Terminal 2 (Client 1)
python3 client.py 192.168.1.100

# Terminal 3 (Client 2)  
python3 client.py 192.168.1.100

# Terminal 4 (Client 3)
python3 client.py 192.168.1.100
```

### 3. Start job

```bash
CrackStorm> status
CrackStorm> crack hashes.txt rockyou.txt
CrackStorm> results
```

## 📁 Example Files

### Hash file (hashes.txt)

```
5d41402abc4b2a76b9719d911017c592
098f6bcd4621d373cade4e832627b4f6
```

### Test wordlist (test_passwords.txt)

```
password
123456
admin
hello
test
```

## 🔐 Supported Hash Modes

Currently CrackStorm supports:
- MD5 (Mode 0) - Default
- Additional modes can be easily configured

**Extension for other hash modes:**

```python
# Change in execute_hashcat_job():
cmd = [
    'hashcat',
    '-m', '1000',  # NTLM
    # or
    '-m', '1400',  # SHA256
    # etc.
    ...
]
```

## 🛡️ Security Notes

⚠️ **IMPORTANT: Use only for authorized penetration tests!**

- Use CrackStorm only in controlled environments
- Ensure you have permission to crack passwords
- Use strong network security in production environments
- Network traffic monitoring recommended

## 🐛 Troubleshooting

### Client cannot connect

```bash
# Check firewall
sudo ufw allow 9999

# Test network
ping <host_ip>
telnet <host_ip> 9999
```

### Hashcat not found

```bash
# Check installation
which hashcat
hashcat --version

# Check PATH
echo $PATH
```

### Performance optimization

```bash
# Check GPU status
nvidia-smi  # NVIDIA
rocm-smi    # AMD

# Monitor CPU usage
htop
```

## 📊 System Monitoring

CrackStorm automatically shows:
- Number of connected clients
- CPU/GPU information
- Active jobs
- Network status
- Found passwords

## 🔧 Advanced Configuration

### Custom Hashcat parameters

```python
# In client.py - execute_hashcat_job()
cmd = [
    'hashcat',
    '-m', '0',
    '-a', str(attack_mode),
    '--force',           # Ignore GPU warnings
    '--opencl-device-types', '1,2',  # CPU + GPU
    '--workload-profile', '3',       # High Performance
    hash_file,
    wordlist,
    '--skip', str(skip),
    '--limit', str(limit)
]
```

### Adjust network timeouts

```python
# In host.py
client_socket.settimeout(30)  # Increase for slower networks

# In client.py  
self.socket.settimeout(60)    # Heartbeat interval
```

## 📈 Performance Tips

1. **GPU Optimization**: Ensure all clients have GPU support
2. **Network**: Use Gigabit Ethernet for best performance
3. **Wordlists**: Copy large wordlists to local SSDs
4. **RAM**: Minimum 8GB RAM per client recommended
5. **Cooling**: Monitor temperatures during longer jobs

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (git checkout -b feature/AmazingFeature)
3. Commit changes (git commit -m 'Add some AmazingFeature')
4. Push branch (git push origin feature/AmazingFeature)
5. Create Pull Request

## 📝 License

This project is under the MIT License - see [LICENSE](LICENSE) for details.

## ⚠️ Disclaimer

CrackStorm is developed exclusively for educational purposes and authorized security testing. The authors assume no responsibility for misuse of this tool. Use it only with explicit permission on systems you own or are authorized to test.

## 🎓 Developed for Cybersecurity Studies

This tool was specifically developed for cybersecurity degree programs to:
- Demonstrate distributed computing concepts
- Gain practical experience with penetration testing tools
- Learn network programming
- Understand professional cybersecurity workflows

---

**CrackStorm v1.0** - Distributed Hashcat Cluster Manager  
Powered and Developed by RedScorp OS Team
