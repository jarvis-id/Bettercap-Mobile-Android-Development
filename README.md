# Bettercap-Mobile-Android-Development

# 🛡️ Bettercap Mobile — Android Edition

<p align="center">
  <img src="https://img.shields.io/badge/Version-mobile.v2.4.0-00d084?style=for-the-badge&logo=android&logoColor=white"/>
  <img src="https://img.shields.io/badge/Build-SUCCESSFUL-00d084?style=for-the-badge&logo=gradle&logoColor=white"/>
  <img src="https://img.shields.io/badge/Platform-Android-3DDC84?style=for-the-badge&logo=android&logoColor=white"/>
  <img src="https://img.shields.io/badge/Language-Kotlin%20%2F%20Java-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white"/>
  <img src="https://img.shields.io/badge/Theme-Cyber%20Tactical%20Dark-0f172a?style=for-the-badge&logo=gnuprivacyguard&logoColor=00d084"/>
  <img src="https://img.shields.io/badge/Tests-PASSED-00d084?style=for-the-badge&logo=checkmarx&logoColor=white"/>
</p>

<p align="center">
  <strong>A full-featured Android port of the Bettercap network attack & monitoring framework</strong><br/>
  <em>Built for Professional Network Security Assessments & Wireless Penetration Testing on Mobile Devices</em>
</p>

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Architecture & Tech Stack](#-architecture--tech-stack)
- [Visual Identity & UI Theme](#-visual-identity--ui-theme)
- [Core Module Development](#-core-module-development)
- [Interactive CLI Terminal](#-interactive-cli-terminal)
- [Caplet Engine](#-caplet-engine)
- [Navigation & UI Components](#-navigation--ui-components)
- [Output & Generated Reports](#-output--generated-reports)
- [Build & Test Results](#-build--test-results)
- [Legal & Ethics](#-legal--ethics)

---

## 🔍 Project Overview

**Bettercap Mobile** is a native Android application that brings the full power of the [Bettercap](https://www.bettercap.org) framework to mobile devices. The project reimplements Bettercap's core attack and recon modules as native Android services, enabling security professionals to conduct field assessments without relying on a laptop.

### Development Goals

- ✅ **Native Android Port** — Reimplementing Bettercap modules in Kotlin/Java as Android Services
- ✅ **Root & Rootless Support** — Dual execution mode: `su` (kernel-level injection) and Userland Audit
- ✅ **Mobile-First UX** — Purpose-built touch interface with a Cyber Tactical Dark aesthetic
- ✅ **Autonomous Operation** — Caplet scripting engine ported for on-device automation
- ✅ **Real-Time Feedback** — Live packet inspection and attack status streamed to an embedded terminal

> ⚠️ **DISCLAIMER**: This application is intended **solely for authorized security testing**, internal audits, and cybersecurity education. Unauthorized use against networks you do not own or have explicit written permission to test is **illegal**.

---

## 🏗️ Architecture & Tech Stack

```
┌─────────────────────────────────────────────────────────┐
│                  Bettercap Mobile Android                │
│                     mobile.v2.4.0                        │
├─────────────────────────────────────────────────────────┤
│  UI Layer          │  Jetpack Compose / View System      │
│  Navigation        │  Bottom Nav + FAB (4-tab layout)    │
│  Theme Engine      │  Custom MaterialTheme (Dark/Emerald)│
├─────────────────────────────────────────────────────────┤
│  Module Layer      │  Android Services (Foreground)      │
│  ├─ net.recon      │  ARP Table Reader + Active Prober   │
│  ├─ syn.scan       │  TCP Socket Scanner                 │
│  ├─ ble.recon      │  BluetoothLeScanner API             │
│  ├─ arp.spoof      │  Raw Socket / su Packet Injection   │
│  ├─ dns.spoof      │  DNS Response Forger                │
│  └─ net.sniff      │  Packet Capture (pcap / VPN API)    │
├─────────────────────────────────────────────────────────┤
│  System Layer      │  Linux Kernel (via /proc, raw sock) │
│  Root Access       │  su binary / CAP_NET_RAW            │
│  Network Interface │  wlan0 / WiFi Network Manager       │
└─────────────────────────────────────────────────────────┘
```

### Key Dependencies

| Component              | Technology                                  |
|------------------------|---------------------------------------------|
| Language               | Kotlin + Java (JNI for native ops)          |
| Build System           | Gradle (AGP)                                |
| UI Framework           | Jetpack / View System                       |
| Bluetooth Stack        | Android BluetoothLeScanner API              |
| Network Capture        | libpcap / Android VPN Service API           |
| Root Execution         | `su` binary / `Runtime.exec()`              |
| Unit Testing           | JUnit 4 + Robolectric                       |
| Test Runner            | `:app:testDebugUnitTest`                    |

---

## 🎨 Visual Identity & UI Theme

The application implements the **"Professional Polish" — Cyber Tactical Dark Theme** across all UI components, designed to evoke a tactical security tooling aesthetic.

### 🖤 Color Palette

| UI Element           | Color Token                                    |
|----------------------|------------------------------------------------|
| Primary Background   | `Slate 950` — Deep dark canvas                |
| Surface / Cards      | `Slate 900` — Elevated card layer             |
| Border / Divider     | `Slate 800` — Subtle separators               |
| Primary Accent       | `Emerald 400 / 500` — Neon green highlights   |
| Terminal Typography  | Monospaced font — IP, MAC, CLI output         |

### 🔖 Header & Live Status Bar

The application header renders:

- **`B` Emblem** — Bettercap tactical identity mark
- **Version Label** — `mobile.v2.4.0`
- **Horizontal Interactive Status Pills:**

  | Status Pill    | States                      |
  |----------------|-----------------------------|
  | 🌐 Network     | `ON` / `OFF`                |
  | 🔵 BT          | `SCANNING` / `OFF`          |
  | 👁️ Sniffer    | `IDLE` / `ACTIVE`           |
  | 🔑 Root        | `SU` / `USERLAND`           |

### 🃏 Target Cards

Each discovered device is rendered as a Target Card, displaying:

- 📍 **IP Address** of the target device
- 🚦 **Gateway Indicator** badge (marks the router)
- ☠️ **Poisoning Badge** — animated pulsing neon-green dot (indicates active ARP spoof)
- 🔗 **MAC Address** in monospaced format
- 🔓 **Open Port Tags** — inline tags showing active open ports

---

## ⚙️ Core Module Development

### 1. 🔭 Network Recon (`net.recon`)

Implements active and passive local network discovery.

**Implementation:**
- Reads the Linux ARP table directly from `/proc/net/arp`
- Performs **active sweep probing** across the WiFi subnet (`wlan0`)
- Detects local IP, subnet mask, and gateway router address
- Enumerates all live hosts on the network segment

```bash
# Interactive CLI usage
net.recon on
net.show
net.probe 192.168.1.0/24
```

---

### 2. 🔬 Port Scanner (`syn.scan`)

Implements multi-port TCP scanning against selected targets.

**Scanned Ports (default profile):**

| Port  | Service          |
|-------|------------------|
| 21    | FTP              |
| 22    | SSH              |
| 23    | Telnet           |
| 53    | DNS              |
| 80    | HTTP             |
| 443   | HTTPS / TLS      |
| 445   | SMB              |
| 3389  | RDP              |
| 8080  | HTTP Alternate   |

```bash
# Interactive CLI usage
syn.scan 192.168.1.105
```

---

### 3. 📡 Bluetooth Low Energy Scanner (`ble.recon`)

Implements BLE peripheral discovery using the Android BluetoothLeScanner API.

**Captured Data:**
- Device **MAC Address**
- Peripheral **device name** (advertised name)
- **RSSI signal strength** (in dBm)
- Advertised **Service UUIDs**

```bash
# Interactive CLI usage
ble.recon on
```

---

### 4. ☠️ MITM & ARP Spoofing Engine (`arp.spoof`)

Implements Full-Duplex Man-in-the-Middle attack management.

**Implementation Modes:**

| Mode             | Description                                               |
|------------------|-----------------------------------------------------------|
| **Root Mode**    | Raw socket injection via `su` — kernel-level ARP crafting |
| **Userland Mode**| Security Audit mode for non-rooted devices               |

**Features:**
- Specific target selection or full-subnet poisoning
- Simultaneous gateway + target ARP cache poisoning (Full-Duplex)
- Start/stop lifecycle managed as a foreground Android Service

```bash
# Interactive CLI usage
arp.spoof on
arp.spoof off
```

---

### 5. 🌐 DNS Hijacker (`dns.spoof`)

Implements DNS response forgery for domain redirection testing.

**Features:**
- Custom **host redirect rule** configuration
- Maps domain names → spoofed IP addresses
- Validates DNS resolution resilience in audit environments

---

### 6. 📦 Live Packet Sniffer (`net.sniff`)

Implements real-time network traffic inspection.

**Protocol Filters:**

| Filter    | Description                              |
|-----------|------------------------------------------|
| `DNS`     | DNS queries and responses                |
| `HTTP`    | Cleartext HTTP payload inspection        |
| `ARP`     | ARP request/reply packets                |
| `TLS`     | TLS handshake metadata                   |
| `TCP`     | Generic TCP segment capture              |

**Features:**
- ✅ **Payload keyword search** across captured packets
- ✅ **Interactive protocol filters** by type or IP/URL keyword

---

## 💻 Interactive CLI Terminal

An embedded terminal console providing full interactive command support, replicating the Bettercap CLI experience on mobile.

**Supported Commands:**

```
help                     — Display available command list
net.show                 — Show discovered target list
net.recon on/off         — Toggle network recon module
net.probe <ip>           — Active probe to specific IP
syn.scan <ip>            — TCP port scan on target
arp.spoof on/off         — Toggle ARP spoofing engine
ble.recon on/off         — Toggle BLE scanner
clear                    — Clear terminal output
```

- **Quick Command Buttons** — Tap-to-execute shortcuts for frequently used commands
- **Chronological log** — All output timestamped and scrollable in the terminal view

---

## 📜 Caplet Engine

Runs preset Bettercap caplet scripts for automated attack sequences.

| Caplet              | Function                                                     |
|---------------------|--------------------------------------------------------------|
| `recon-quick.cap`   | Fast network recon — ARP scan + active probe sweep          |
| `arp-mitm.cap`      | Automated full-duplex ARP MITM attack sequence              |
| `ble-hunter.cap`    | Aggressive BLE discovery across all nearby peripherals      |
| `http-dump.cap`     | HTTP payload dump from intercepted network traffic          |

---

## 📱 Navigation & UI Components

### Bottom Navigation Bar

| Tab          | Icon | Function                                             |
|--------------|------|------------------------------------------------------|
| **Radar**    | 📡   | Network map view & live target list                  |
| **Attack**   | ☠️   | ARP spoof / DNS spoof / MITM module controls         |
| **Sniff**    | 👁️  | Live packet sniffer & protocol filter panel          |
| **Conf**     | ⚙️   | Module configuration, caplets, app settings          |

### Floating Action Button (FAB)

Center FAB in the navigation bar for single-tap control:
- ▶️ **Start** — Launch active scan / attack module
- ⏹️ **Stop** — Halt all running modules immediately

---

## 📊 Output & Generated Reports

### 1. 🗺️ Network Device Inventory *(Topology Map)*

- **Active Target List** — Full IP + MAC address enumeration of all devices on the local subnet
- **Vendor Identification (OUI Lookup)** — Resolves MAC prefix against official hardware vendor database (Apple, Samsung, ASUS, Raspberry Pi, Espressif IoT, Intel, etc.)
- **Gateway Detection & Latency** — Router/gateway IP with measured ping latency (ms)

---

### 2. 🔓 Open Services Report *(Port Scan Report)*

- **TCP Port Status** — Scan results for default and custom port profiles
- **Service Mapping** — Identifies running network services per target device
- **Misconfiguration Flags** — Highlights ports that should be closed but are found open

---

### 3. 🔵 Wireless & Bluetooth Discovery *(BLE Report)*

- **BLE Device List** — Beacons, smartwatches, asset trackers (AirTag/Tile), IoT sensors
- **RSSI (dBm)** — Signal strength as a relative proximity estimator
- **Service UUIDs** — Advertised service identifiers from BLE peripherals

---

### 4. 📦 Network Traffic Log *(Packet Sniffer Log)*

- **Real-Time Packet Inspection** — Captured DNS, HTTP, ARP, TLS, and TCP packet streams
- **DNS Query Analysis** — Reveals all domain names being queried by devices on the network
- **Interactive Filtering** — Filter by protocol, IP address, or payload keyword

---

### 5. ⚔️ MITM Vulnerability Assessment

- **ARP Spoofing Simulation** — Tests local network resilience against ARP cache poisoning (specific target or full-duplex subnet)
- **DNS Spoofing Simulation** — Tests DNS resolution resistance against spoofed responses
- **Privilege Evaluation** — Reports root (`su` / `CAP_NET_RAW`) vs. Userland Audit execution context

---

### 6. 📋 Session Log & Caplet Automation Output

- **Terminal History** — Full chronological log of all commands, warnings, and execution results with timestamps
- **Caplet Output** — Automation results from `recon-quick.cap`, `arp-mitm.cap`, `ble-hunter.cap`, `http-dump.cap`

---

## ✅ Build & Test Results

```
╔══════════════════════════════════════════════════════════╗
║         BUILD & TEST SUMMARY — mobile.v2.4.0             ║
╠══════════════════════════════════════════════════════════╣
║  Build Status          :  ✅  BUILD SUCCESSFUL            ║
║  Unit Tests            :  ✅  PASSED                      ║
║  Robolectric Tests     :  ✅  PASSED                      ║
║  Gradle Task           :  :app:testDebugUnitTest          ║
╠══════════════════════════════════════════════════════════╣
║  Verified Components:                                     ║
║  ✅  Application configuration & initialization           ║
║  ✅  Visual rendering (Cyber Tactical Dark Theme)         ║
║  ✅  Navigation components (Bottom Nav + FAB)             ║
║  ✅  Target cards & status pills                          ║
║  ✅  Interactive CLI terminal console                     ║
║  ✅  All Bettercap modules (recon/scan/spoof/sniff)       ║
╚══════════════════════════════════════════════════════════╝
```

---

## ⚖️ Legal & Ethics

This application was developed strictly for **authorized security testing, academic research, and professional penetration testing engagements**. Users bear full legal responsibility for their use in compliance with:

- Applicable cybercrime and computer fraud laws in their jurisdiction
- Explicit written authorization from the network/system owner under test
- Relevant organizational security policies and responsible disclosure standards

**Always obtain written permission before conducting any penetration test.**

---

<p align="center">
  <em>Built with ❤️ for the cybersecurity community</em><br/>
  <code>mobile.v2.4.0 — Cyber Tactical Dark Theme — Professional Polish</code>
</p>
