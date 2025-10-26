# Unblock-Discord-tiktok

# 🚀 Liberty - Advanced WARP Proxy Client

<div align="center">

![Liberty Banner](https://img.shields.io/badge/Liberty-Proxy_Client-blue?style=for-the-badge&logo=cloudflare)

**A lightweight, intelligent proxy solution using Cloudflare WARP technology**

[![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)](https://github.com/1buGaith/liberty-proxy)
[![C++](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)](https://github.com/1buGaith/liberty-proxy)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Release](https://img.shields.io/badge/Release-v1.0-orange?style=for-the-badge)](https://github.com/1buGaith/liberty-proxy/releases)

[Features](#-features) • [Installation](#-installation) • [Usage](#-usage) • [FAQ](#-faq) • [Support](#-support)

</div>

---

## 📋 Overview

Liberty is a sophisticated Windows proxy client that leverages Cloudflare WARP's infrastructure to provide secure, reliable internet connectivity. Built with performance and stability in mind, it features automatic failover, health monitoring, and seamless system integration.

### 🎯 Key Highlights

- **Zero Configuration** - Works out of the box with automatic WARP key generation
- **Smart Reconnection** - Intelligent health checks and automatic recovery
- **System Integration** - Runs silently in the system tray with minimal resource usage
- **Double WARP** - WARP-in-WARP configuration for enhanced privacy
- **Proxy Guard** - Prevents orphaned proxy settings after system crashes

---

## ✨ Features

### 🔐 Security & Privacy
- **Encrypted Keys Storage** - WARP credentials secured using Windows DPAPI
- **No Data Logging** - Zero telemetry or user tracking
- **Automatic Key Rotation** - Fresh WARP keys on first run

### 🛡️ Reliability
- **Health Monitoring** - Continuous connection health checks (20s intervals)
- **Auto-Recovery** - Automatic reconnection with exponential backoff
- **Proxy Guard Service** - Cleans up proxy settings on system shutdown
- **Crash Protection** - Detects and fixes orphaned proxy configurations

### ⚙️ System Integration
- **System Tray Interface** - Unobtrusive, easy-to-access controls
- **Auto-Start Support** - Launch on Windows startup
- **Service Mode** - Windows service for background operation
- **UAC-Aware** - Proper elevation handling for administrative tasks

### 🌐 Network Features
- **Dual Stack Support** - Full IPv4 and IPv6 compatibility
- **DNS over HTTPS** - Enhanced privacy with Cloudflare DNS (1.1.1.1)
- **Port Conflict Detection** - Automatic detection of port availability
- **Smart Routing** - Rule-based routing for Discord, TikTok, and more

---

## 💻 System Requirements

| Component | Requirement |
|-----------|-------------|
| **OS** | Windows 7 SP1 / Windows Server 2008 R2 or later |
| **Architecture** | x64 (64-bit) |
| **RAM** | 100 MB minimum |
| **Disk Space** | 50 MB |
| **Network** | Active internet connection |
| **Privileges** | Administrator (first-time setup only) |

---

## 🚀 Installation

### Method 1: Quick Install (Recommended)

1. **Download** the latest `liberty.exe` from [Releases](https://github.com/1buGaith/liberty-proxy/releases)
2. **Run** the executable - it will automatically:
   - Copy itself to `%LOCALAPPDATA%\Liberty\`
   - Install the Proxy Guard service
   - Generate WARP security keys
   - Set up auto-start (optional)
3. **Done!** Look for the Liberty icon in your system tray

### Method 2: Manual Setup

```bash
# Download the binary
curl -LO https://github.com/1buGaith/liberty/releases/latest/download/unblock-jo.exe
```

### First Run

On first launch, Liberty will:
1. Request administrator privileges (one-time only)
2. Install the background service for proxy cleanup
3. Generate unique WARP credentials
4. Configure auto-start settings
5. Initialize the system tray interface

---

## 📖 Usage

### Basic Operations

**Starting the Proxy**
- Right-click the tray icon → **Start**
- Or enable **Auto-connect on startup** for automatic activation

**Stopping the Proxy**
- Right-click the tray icon → **Stop**

**Status Indicators**
- 🔵 **Disconnected** - Proxy is not active
- 🟡 **Connecting...** - Establishing connection
- 🟢 **Connected** - Proxy active and healthy
- 🔴 **Failed** - Connection error (auto-retry in progress)

### Configuration

#### Auto-Connect
Enable automatic connection on Windows startup:
```
Right-click tray icon → ☑ Auto-connect on startup
```

#### Proxy Settings
Liberty automatically configures Windows system proxy:
- **HTTP/HTTPS Proxy**: `127.0.0.1:7890`
- **SOCKS5 Proxy**: `127.0.0.1:7891`
- **Mixed Port**: `127.0.0.1:7892`

#### Advanced Configuration
The application uses mihomo (Clash Meta) core with pre-configured settings:
- DNS: Cloudflare (1.1.1.1)
- Mode: Rule-based routing
- MTU: 1280 (WARP) / 1200 (WARP-in-WARP)

---

## 🔧 Technical Details

### Architecture

```
┌─────────────────────────────────────────┐
│         Liberty Application             │
│  ┌───────────────────────────────────┐  │
│  │     System Tray Interface         │  │
│  └───────────┬───────────────────────┘  │
│              │                           │
│  ┌───────────▼───────────────────────┐  │
│  │   MihomoProxy Manager             │  │
│  │  • Connection lifecycle           │  │
│  │  • Health monitoring              │  │
│  │  • Auto-recovery                  │  │
│  └───────────┬───────────────────────┘  │
│              │                           │
│  ┌───────────▼───────────────────────┐  │
│  │   mihomo Core (Clash Meta)        │  │
│  │  • WireGuard tunneling            │  │
│  │  • Rule-based routing             │  │
│  │  • DNS resolution                 │  │
│  └───────────┬───────────────────────┘  │
└──────────────┼───────────────────────────┘
               │
    ┌──────────▼──────────┐
    │  Cloudflare WARP    │
    │   (162.159.192.1)   │
    └─────────────────────┘
```

### WARP Configuration

Liberty uses a **double WARP** setup for enhanced security:

1. **Primary WARP Tunnel**
   - Server: `162.159.192.1:500`
   - Protocol: WireGuard with AmneziaWG enhancements
   - MTU: 1280 bytes

2. **Secondary WARP Tunnel** (via primary)
   - Server: `162.159.192.1:2408`
   - Protocol: WireGuard
   - MTU: 1200 bytes
   - Configuration: WARP-in-WARP chaining

### Security Features

- **DPAPI Encryption**: WARP keys stored using Windows Data Protection API
- **SHA-256 Verification**: Downloaded binaries verified against known hashes
- **No Plain Text Storage**: All sensitive data encrypted at rest
- **Secure Key Generation**: Random client IDs with proper entropy

---

## 🛠️ Building from Source

### Prerequisites

- **Visual Studio 2019 or later** with C++ desktop development workload
- **Windows SDK 10.0** or later
- **Git** for version control

### Build Steps

```bash
# Clone the repository
git clone https://github.com/1buGaith/liberty.git
cd liberty

# Open in Visual Studio
start liberty.sln

# Build configuration
# - Platform: x64
# - Configuration: Release

# Output: Release/unblock-jo.exe
```

### Dependencies

All dependencies are Windows built-in:
- `ws2_32.lib` - Winsock 2
- `wininet.lib` - Internet APIs
- `advapi32.lib` - Advanced APIs (Registry, Services)
- `shell32.lib` - Shell functions
- `crypt32.lib` - Cryptography APIs

---

## ❓ FAQ

<details>
<summary><b>Why does it need administrator privileges?</b></summary>

Administrator access is required **only once** during initial setup to:
- Install the Windows service for proxy cleanup
- Create the application directory in AppData
- Set up auto-start registry keys

After setup, the application runs with normal user privileges.
</details>

<details>
<summary><b>Is my data secure?</b></summary>

Yes. Liberty:
- Encrypts all WARP credentials using Windows DPAPI
- Does not collect or transmit any personal data
- Uses Cloudflare's secure WARP infrastructure
- Verifies downloaded files with SHA-256 checksums
</details>

<details>
<summary><b>What if the connection fails?</b></summary>

Liberty includes intelligent auto-recovery:
- Health checks every 20 seconds
- Automatic reconnection with exponential backoff
- Up to 3 retry attempts before reporting failure
- Manual reconnection available via tray icon
</details>

<details>
<summary><b>Can I use this with other VPNs?</b></summary>

It's not recommended to run multiple VPN/proxy solutions simultaneously as they may conflict. Liberty should be used as your primary proxy solution.
</details>

<details>
<summary><b>How do I uninstall?</b></summary>

Right-click the tray icon → **Uninstall** → Confirm
This will:
- Stop all proxy services
- Remove the Windows service
- Delete application files
- Clean up registry entries
- Reset system proxy settings
</details>

---

## 🐛 Troubleshooting

### Connection Issues

**Problem**: Cannot connect after installation
```
Solution:
1. Check if ports 7890 and 9090 are available
2. Disable antivirus temporarily
3. Restart the application as administrator
4. Check Windows Firewall settings
```

**Problem**: Frequent disconnections
```
Solution:
1. Check your internet connection stability
2. Verify WARP keys: Delete %LOCALAPPDATA%\Liberty\data.dat
3. Restart the application to regenerate keys
```

### Service Issues

**Problem**: Proxy Guard service won't install
```
Solution:
1. Run cmd.exe as administrator
2. Execute: sc delete LibertyService
3. Restart the application
4. Accept UAC prompt when requested
```

---

## 📊 Performance

| Metric | Value |
|--------|-------|
| Memory Usage | ~15-20 MB (idle) |
| CPU Usage | <1% (normal operation) |
| Startup Time | 2-3 seconds |
| Connection Time | 1-2 seconds |
| Binary Size | ~350 KB |

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

### Development Guidelines

- Follow the existing code style
- Add comments for complex logic
- Test on multiple Windows versions
- Update documentation as needed

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Cloudflare WARP** - For the excellent VPN infrastructure
- **mihomo (Clash Meta)** - For the powerful proxy core
- **Community** - For testing and feedback

---

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/1buGaith/liberty/issues)
- **Discussions**: [GitHub Discussions](https://github.com/1buGaith/liberty/discussions)
- **Instagram**: [@2lryy](https://www.instagram.com/2lryy/)

---

<div align="center">

**Made with ❤️ for unrestricted internet access**

[![Star this repo](https://img.shields.io/github/stars/1buGaith/liberty?style=social)](https://github.com/1buGaith/liberty-proxy)
[![Follow on GitHub](https://img.shields.io/github/followers/1buGaith?style=social)](https://github.com/yourusername)

</div>
