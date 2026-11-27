<p align="center">
  <img src="favicon.ico" alt="Reezium Logo" width="100">
</p>

<h1 align="center">Reezium Imager</h1>

<p align="center">
  <strong>Memory Acquisition · Live Threat Hunting · AI-Assisted Analysis · Cloud Intelligence</strong><br>
  <sub>A forensic-grade DFIR tool for RAM capture, real-time anomaly detection, and AI-powered investigation on Windows & Linux</sub>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/python-3.9+-blue?logo=python&logoColor=white" alt="Python 3.9+">
  <img src="https://img.shields.io/badge/platform-Windows%20%7C%20Linux-lightgrey" alt="Platform">
  <img src="https://img.shields.io/badge/license-MIT-green" alt="License">
  <img src="https://img.shields.io/badge/version-1.0-orange" alt="Version">
</p>

<p align="center">
  <a href="#-what-is-reezium-imager">Overview</a> •
  <a href="#-key-features">Features</a> •
  <a href="#%EF%B8%8F-how-it-works">Architecture</a> •
  <a href="#-ai-assistant">AI Assistant</a> •
  <a href="#-cloud-threat-intelligence">Cloud Intel</a> •
  <a href="#-installation">Install</a> •
  <a href="#-usage">Usage</a> •
  <a href="#-configuration">Config</a> •
  <a href="#-api-reference">API</a> •
  <a href="#-detection-methodology">Methodology</a>
</p>

---

## 📖 What is Reezium Imager?

**Reezium Imager** is an open-source digital forensics and incident response (DFIR) tool that combines **physical memory acquisition**, a **real-time threat-hunting engine**, **AI-powered investigation guidance**, and **cloud threat intelligence enrichment** into a single platform. It provides investigators with a unified dashboard to monitor system health, hunt for suspicious processes, get AI-generated analysis of detections, cross-reference threats against global reputation databases, and capture forensically-sound RAM dumps — all from a browser-based UI or headless CLI.

> Reezium Imager captures full physical memory dumps and performs real-time threat detection using a hybrid approach combining Modified Z-Score statistical analysis, Isolation Forest machine learning, Shannon entropy analysis, and MITRE ATT&CK-mapped heuristic rules. Detections are enriched with cloud intelligence from VirusTotal and AbuseIPDB, and can be explained by an AI co-pilot powered by Google Gemini, OpenAI, or local Ollama — all through an intuitive web dashboard or headless CLI.

---

## ✨ Key Features

| Feature | Description |
|:--------|:------------|
| 🧠 **RAM Acquisition** | Full physical memory dump using WinPMEM (Windows) or AVML (Linux) with MD5/SHA256 chain-of-custody hashing |
| 🔍 **Hybrid Threat Detection** | Three detection modes — Statistical, Machine Learning, and Hybrid — for comparative analysis |
| 🤖 **AI-Powered Analysis** | LLM co-pilot (Gemini / OpenAI / Ollama) explains detections, suggests investigation steps, and assesses risk |
| ☁️ **Cloud Threat Intelligence** | Real-time enrichment via VirusTotal (file hash reputation) and AbuseIPDB (IP reputation) |
| 📊 **Live Dashboard** | Real-time system monitoring with CPU, RAM, process, and network connection widgets |
| 🎯 **MITRE ATT&CK Mapping** | Every detection rule is tagged with its corresponding MITRE ATT&CK technique ID |
| 🛡️ **Heuristic Rules** | Detects masquerading, reverse shells, suspicious paths, and malicious parent-child process chains |
| 📈 **Entropy Analysis** | Shannon entropy + English bigram frequency analysis to identify randomly-named malware |
| 🌲 **Isolation Forest ML** | Unsupervised machine learning anomaly detection with warm-up training strategy |
| 📋 **Process Manager** | Full process listing with search, filtering, and JSON export |
| 🌐 **Network Monitor** | Active connection viewer with status filtering (ESTABLISHED, LISTEN, SYN_SENT, etc.) |
| ⏱️ **Event Timeline** | Chronological view of process creation events for temporal analysis |
| 🖥️ **Memory Map** | System memory overview, top-20 RAM consumers, and per-process file/library inspection (UC06) |
| 💻 **Multi-Mode** | Web UI, Desktop App (pywebview), and Headless CLI modes |

---

## 🏗️ How It Works

Reezium Imager operates across four integrated layers:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        REEZIUM IMAGER ARCHITECTURE                      │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│   ┌──────────────┐   ┌──────────────┐   ┌──────────────────────────┐   │
│   │  Web UI /     │   │  Headless    │   │  Desktop App             │   │
│   │  Dashboard    │   │  CLI Mode    │   │  (pywebview)             │   │
│   └──────┬───────┘   └──────┬───────┘   └──────────┬───────────────┘   │
│          └──────────────────┼──────────────────────┘                    │
│                             ▼                                           │
│   ┌─────────────────────────────────────────────────────────────────┐   │
│   │                   Flask REST API Layer                           │   │
│   │   /api/stats · /api/processes · /api/suspicious · /api/capture  │   │
│   │   /api/ai/explain · /api/intel/hash · /api/intel/ip             │   │
│   └─────────────┬───────────────┬───────────────┬───────────────────┘   │
│                 │               │               │                       │
│   ┌─────────────▼─────┐ ┌──────▼──────┐ ┌──────▼──────────────────┐   │
│   │  Detection Engine  │ │  AI Module  │ │  Cloud Intel Module     │   │
│   │  ┌──────────────┐  │ │  (core/ai)  │ │  (core/cloud)          │   │
│   │  │ Statistical   │  │ │             │ │                        │   │
│   │  │ ML (IsoForest)│  │ │  Gemini     │ │  VirusTotal v3         │   │
│   │  │ Entropy       │  │ │  OpenAI     │ │  AbuseIPDB v2          │   │
│   │  │ Heuristics    │  │ │  Ollama     │ │  TTL Cache + Rate Lim. │   │
│   │  └──────────────┘  │ │  TTL Cache  │ │                        │   │
│   └────────────────────┘ └─────────────┘ └────────────────────────┘   │
│                 │                                                       │
│   ┌─────────────▼──────────────────────────────────────────────────┐   │
│   │              Memory Acquisition Engine (core/capture_engine)    │   │
│   │              WinPMEM (Windows) · AVML (Linux)                   │   │
│   │              RFC 3227 · MD5/SHA256 hashing · JSON sidecar       │   │
│   └────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────┘
```

### 1. Memory Acquisition

The capture engine wraps platform-specific memory acquisition drivers:

- **Windows**: [WinPMEM](https://github.com/Velocidex/WinPmem) — kernel-level physical memory access
- **Linux**: [AVML](https://github.com/microsoft/avml) — user-space memory acquisition

Before capturing, Reezium snapshots volatile data (process list, network connections, CPU/RAM state) per **RFC 3227** order-of-volatility guidelines. After capture, it computes **MD5 + SHA256** hashes and writes a JSON sidecar metadata file for chain-of-custody verification. Both the dump and sidecar are write-protected automatically.

### 2. Threat Detection Engine

The detection engine scores every running process using **four independent signal types**:

```
┌──────────────────────────────────────────────────────────────┐
│                    THREAT SCORING PIPELINE                    │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────┐  ┌─────────────┐                           │
│  │ Statistical  │  │  ML (IsoFor) │  ← MAX(score), not SUM  │
│  │ Modified Z   │  │  Anomaly    │    (anti-stacking)       │
│  └──────┬──────┘  └──────┬──────┘                           │
│         └───────┬────────┘                                  │
│                 ▼                                            │
│  ┌─────────────────────┐                                    │
│  │  + Entropy Score     │  Shannon entropy + bigram analysis │
│  ├─────────────────────┤                                    │
│  │  + Heuristic Scores  │  Name, path, network, parent-child│
│  └──────────┬──────────┘                                    │
│             ▼                                               │
│  ┌─────────────────────┐                                    │
│  │  Multi-Signal Gate   │  1 strong OR 2+ weak signals      │
│  └──────────┬──────────┘                                    │
│             ▼                                               │
│  ┌─────────────────────┐                                    │
│  │  Cloud Enrichment    │  VirusTotal + AbuseIPDB (optional)│
│  ├─────────────────────┤                                    │
│  │  AI Explanation      │  LLM co-pilot (on demand)         │
│  ├─────────────────────┤                                    │
│  │  Reporting Threshold │  Score > 30 → surface as threat   │
│  └─────────────────────┘                                    │
└──────────────────────────────────────────────────────────────┘
```

> **Key calibration principle**: A single weak signal does NOT trigger an alert. Threats require either **(a) one strong signal** (known malware name, system process impersonation, reverse shell) or **(b) two or more independent weak signals** combined. This mirrors real-world EDR design and minimizes false positives.

### Detection Modes

| Mode | Components | Use Case |
|:-----|:-----------|:---------|
| `statistical` | Modified Z-Score + Entropy + Heuristics | Lightweight baseline, fast polling |
| `ml` | Isolation Forest + Entropy + Heuristics | ML-driven anomaly detection |
| `hybrid` | All four signal types | Production default, best detection rate |

---

## 🧪 AI Assistant

Reezium includes an **LLM-powered SOC analyst co-pilot** that provides context-aware investigation guidance for every threat detection. When an analyst clicks "Explain" on a flagged process, the AI assistant generates:

1. **Why This Is Suspicious** — plain-English explanation of what behavioral signals triggered the detection
2. **Attack Technique** — the real-world attack this resembles, mapped to MITRE ATT&CK
3. **Investigation Steps** — 3–5 concrete actions the analyst should take immediately
4. **Risk Assessment** — honest assessment of true positive vs. false positive likelihood

### Supported Providers

| Provider | Cost | Latency | Quality | Offline |
|:---------|:-----|:--------|:--------|:--------|
| **Google Gemini** *(recommended)* | Free tier: 60 req/min | ~1–3s | Excellent | ❌ |
| **OpenAI GPT** | ~$0.01–0.03/request | ~2–5s | Excellent | ❌ |
| **Ollama** (local) | Free | ~5–15s | Good (model-dependent) | ✅ |

### How It Works

The AI module ([`core/ai/llm_assistant.py`](core/ai/llm_assistant.py)) operates as follows:

1. **Configuration** is loaded from environment variables or `config.json` (env vars take priority)
2. A **system prompt** establishes the AI as a senior SOC analyst with forensics expertise
3. For each threat, a **structured user prompt** is built containing the process name, PID, score, severity, detection reasons, MITRE techniques, and any cloud intelligence data
4. The prompt is sent to the configured LLM provider via **direct REST calls** (no SDK dependencies)
5. Responses are **cached for 5 minutes** (keyed on PID + score + message) to avoid redundant calls
6. If cloud intelligence is available (VirusTotal/AbuseIPDB), it's **automatically injected** into the prompt context for more informed analysis

### Graceful Degradation

- If no AI provider is configured → returns a helpful setup guide instead of crashing
- If the API call fails → returns an error message with troubleshooting advice
- If Ollama isn't running → logs a warning with the startup command (`ollama serve`)
- Gemini calls **auto-fallback** through multiple model names if the primary model returns a 503/429

> [!TIP]
> The fastest way to get started is with **Google Gemini** — get a free API key from [aistudio.google.com](https://aistudio.google.com/apikey) and add it to `config.json`. No credit card required.

---

## ☁️ Cloud Threat Intelligence

Reezium enriches threat detections with real-time reputation data from two cloud providers, implemented in [`core/cloud/threat_intel.py`](core/cloud/threat_intel.py):

### VirusTotal (File Hash Reputation)

Queries the **VirusTotal v3 API** to check file hashes against 70+ antivirus engines:

- Accepts **SHA256, SHA1, or MD5** hashes
- Returns malicious/suspicious engine counts, reputation score, threat classification, and tags
- **Free tier**: 500 requests/day, 4 requests/minute

### AbuseIPDB (IP Reputation)

Queries the **AbuseIPDB v2 API** to assess remote IP addresses that flagged processes are connecting to:

- Returns abuse confidence score, total reports, ISP, country, Tor exit node status
- Automatically **skips private/loopback addresses** (127.x, 10.x, 192.168.x)
- **Free tier**: 1,000 requests/day

### How Enrichment Works

```
  Threat Scan Complete
         │
         ▼
  ┌──────────────────┐
  │ For each threat:  │
  │                   │
  │  PID → Connections│──→ Remote IPs ──→ AbuseIPDB Lookup
  │                   │                        │
  │  File Hash ───────│──→ VirusTotal Lookup   │
  │                   │         │               │
  │  threat['cloud_   │←────────┘───────────────┘
  │    intel'] = {...} │
  └──────────────────┘
```

### Built-in Safeguards

| Safeguard | Description |
|:----------|:------------|
| **TTL Cache** | VT results cached 1 hour, AbuseIPDB cached 30 minutes — avoids redundant API calls during repeated scans |
| **Rate Limiter** | Sliding-window rate limiter (VT: 3 req/min, AbuseIPDB: 30 req/min) — stays within free-tier quotas |
| **Thread Safety** | All caches and rate limiters use `threading.Lock` for safe concurrent access |
| **Graceful Degradation** | No API keys → enrichment silently disabled; failures → logged and skipped without interrupting local detection |
| **IP Filtering** | Private and loopback IPs are automatically excluded from AbuseIPDB queries |

### Auto-Enrichment

Cloud enrichment integrates at two points:

1. **During threat scans** (`/api/suspicious?enrich=true`) — automatically enriches all flagged processes' remote connections via AbuseIPDB
2. **During AI explanation** (`/api/ai/explain`) — if a threat hasn't been enriched yet, on-demand enrichment runs before sending context to the LLM, so the AI can factor in IP reputation

> [!NOTE]
> Cloud enrichment is **completely optional**. If no API keys are configured, Reezium operates with full local-only detection capabilities. The cloud modules never block or degrade local analysis.

---

## 📦 Installation

### Prerequisites

- **Python 3.9+**
- **Administrator/Root privileges** (required for memory capture and full process enumeration)

### Setup

```bash
# Clone the repository
git clone https://github.com/risscheese/Reezium_Beta.git
cd Reezium_Beta

# Install dependencies
pip install -r requirements.txt
```

### Dependencies

| Package | Purpose |
|:--------|:--------|
| `flask` | Web server and REST API |
| `psutil` | Process and system telemetry |
| `requests` | HTTP client for AI and cloud API calls |
| `scikit-learn` | Isolation Forest ML detection *(optional — degrades gracefully)* |
| `pywebview` | Desktop app mode *(optional)* |

> [!NOTE]
> If `scikit-learn` is not installed, the ML detection mode is silently disabled. Statistical, entropy, and heuristic detection continue to operate normally. The AI and cloud modules use `requests` for all HTTP calls — no SDK dependencies are required for Gemini, OpenAI, or VirusTotal.

---

## 🚀 Usage

### Web UI Mode (Development/Browser)

```bash
# Run as Administrator for full functionality
python app.py
```

This starts Flask on `http://127.0.0.1:5000` and auto-opens your browser. The dashboard provides:

- Real-time system stats with live charts
- Process manager with search and export
- Network connection viewer with filtering
- Threat hunter with auto-refresh and cloud enrichment
- AI-powered threat explanation on demand
- RAM capture with metadata and hash verification

### Headless CLI Mode (Forensic)

```bash
# Minimal footprint — no web server, no UI
python app.py --cli --case CASE001 --format raw

# With custom output filename
python app.py --cli --case IR2024 --format raw --output incident_dump.raw

# Interactive wizard (prompts for config)
python app.py --cli
```

> [!TIP]
> CLI mode is designed for forensic scenarios where minimizing the tool's memory footprint is critical.

### Windows Quick Launch

Double-click `run_admin.bat` — it auto-elevates to Administrator and starts the web server.

### Linux Quick Launch

```bash
# Run as root for full memory acquisition and process enumeration
sudo python3 app.py
```

Or, if using the pre-built binary:

```bash
sudo ./dist/reezium-linux --cli --case CASE001
```

### Desktop App Mode

When packaged as an `.exe` via PyInstaller, Reezium runs in a native window using pywebview instead of opening a browser tab.

---

## ⚙️ Configuration

All optional features are configured through `config.json` in the project root and/or environment variables (env vars take precedence).

### config.json

```json
{
  "ai_assistant": {
    "provider": "gemini",
    "api_key": "YOUR_GEMINI_API_KEY",
    "model": "gemini-2.5-flash"
  },
  "threat_intel": {
    "virustotal_api_key": "YOUR_VIRUSTOTAL_API_KEY",
    "abuseipdb_api_key": "YOUR_ABUSEIPDB_API_KEY"
  }
}
```

### Environment Variables

| Variable | Purpose | Example |
|:---------|:--------|:--------|
| `REEZIUM_AI_PROVIDER` | AI provider: `gemini`, `openai`, or `ollama` | `gemini` |
| `REEZIUM_AI_API_KEY` | API key for the AI provider | `AIzaSy...` |
| `REEZIUM_AI_MODEL` | Model override (optional — sensible defaults per provider) | `gemini-2.5-flash` |
| `REEZIUM_VT_API_KEY` | VirusTotal API key | `c5fdd5d...` |
| `REEZIUM_ABUSEIPDB_API_KEY` | AbuseIPDB API key | `8d3ff2e...` |

### Windows PowerShell (open it as Administrator)
$env:REEZIUM_AI_PROVIDER = "gemini"
$env:REEZIUM_AI_API_KEY = "your-gemini-key"
$env:REEZIUM_VT_API_KEY = "your-virustotal-key"
$env:REEZIUM_ABUSEIPDB_API_KEY = "your-abuseipdb-key"

### Windows Command Prompt (Administrator)
set REEZIUM_AI_API_KEY=your-gemini-key
set REEZIUM_VT_API_KEY=your-virustotal-key
set REEZIUM_ABUSEIPDB_API_KEY=your-abuseipdb-key

### Linux (with sudo)
sudo REEZIUM_AI_API_KEY="your-gemini-key" \
     REEZIUM_VT_API_KEY="your-virustotal-key" \
     REEZIUM_ABUSEIPDB_API_KEY="your-abuseipdb-key"

     
### Default Models per Provider

| Provider | Default Model |
|:---------|:-------------|
| Gemini | `gemini-3.8-flash` |
| OpenAI | `gpt-4o-mini` |
| Ollama | `llama3.1` |

> [!TIP]
> Environment variables are ideal for CI/CD, containers, and scenarios where you don't want API keys in the filesystem. `config.json` is more convenient for local development.

---

### 🔨 Building Standalone Binaries

Reezium ships with build scripts for both Windows and Linux. PyInstaller **does not cross-compile** — run each script on the target OS.

#### Windows — `build_exe.bat`

```bash
# Double-click or run from an admin terminal
build_exe.bat
```

The script will:

1. Install build dependencies (`pyinstaller`, `pywebview`, `flask`, `psutil`, `scikit-learn`, etc.)
2. Clean any previous `build/` and `dist/` directories
3. Run **PyInstaller** with `--onefile` to bundle everything into a single portable `.exe`

Bundled assets include Flask templates, static files, the detection whitelist, and the `winpmem.exe` driver. Output:

```
dist/Reezium Imager.exe
```

> [!NOTE]
> The `--windowed` flag is intentionally omitted so that `Reezium Imager.exe --cli` can still accept command-line arguments. When run without `--cli`, pywebview provides a native GUI window. If pywebview is unavailable at runtime, the app falls back to launching Flask in the default browser.

#### Linux — `build_linux.sh`

```bash
# Run on Kali, Ubuntu, Debian, or any Linux distro
chmod +x build_linux.sh
./build_linux.sh
```

The script will:

1. Verify it is running on Linux (refuses to run on Windows/macOS)
2. Create or reuse a **virtual environment** (`venv-build/`) — PEP 668 compliant for modern Kali/Debian 12+/Ubuntu 24+
3. Install build dependencies inside the venv (`pyinstaller`, `flask`, `psutil`, `scikit-learn`)
4. Verify that the **AVML** binary is present in the project root
5. Run **PyInstaller** with `--onefile` to produce a standalone ELF binary (includes the detection whitelist)

Output:

```
dist/reezium-linux
```

> [!IMPORTANT]
> **AVML is required** before building. Download it from the [official releases](https://github.com/microsoft/avml/releases/latest):
> ```bash
> wget https://github.com/microsoft/avml/releases/latest/download/avml
> chmod +x avml
> ```

> [!NOTE]
> `pywebview` is optional on Linux and may fail to install on headless systems (it requires GTK/Qt). This does not affect CLI mode or browser-based usage — only the native desktop window mode.

---

## 🔌 API Reference

All endpoints return JSON. The server binds to `127.0.0.1` only (never exposed to the network).

### Core Endpoints

| Endpoint | Method | Description |
|:---------|:-------|:------------|
| `/` | GET | Serves the single-page dashboard UI |
| `/api/stats` | GET | Live system stats (CPU, RAM, process count, threat count, uptime) |
| `/api/processes` | GET | Full process list with CPU%, memory%, threads, connections |
| `/api/connections` | GET | Active network connections with PID, status, type |
| `/api/suspicious` | GET | Threat scan results — Query: `?mode=statistical\|ml\|hybrid&enrich=true\|false` |
| `/api/suspicious/compare` | GET | Side-by-side comparison of all three detection modes |
| `/api/timeline` | GET | Process creation timeline (most recent 50) |
| `/api/memory` | GET | UC06: Virtual memory overview (total, used, available, percent), top-20 processes >50 MB RSS, and real system memory regions |
| `/api/dump/<pid>` | GET | UC06 (extend): Per-process PID inspection — process name, RSS, up to 20 open files, up to 20 unique loaded library paths |
| `/api/reveal/<file>` | GET | Opens the capture folder in Explorer (Windows), Finder (macOS), or file manager (Linux) |
| `/api/system` | GET | System specifications (OS, CPU, storage, network) |
| `/api/capture/start` | POST | Initiate RAM acquisition |
| `/api/capture/status` | GET | Poll capture progress and status |

### AI Assistant Endpoints

| Endpoint | Method | Description |
|:---------|:-------|:------------|
| `/api/ai/status` | GET | Check AI provider configuration (provider, model, configured status) |
| `/api/ai/explain` | POST | Generate an AI explanation for a threat. Body: `{"threat": {...}}` |

### Cloud Intelligence Endpoints

| Endpoint | Method | Description |
|:---------|:-------|:------------|
| `/api/intel/status` | GET | Check which cloud APIs are configured and cache sizes |
| `/api/intel/hash/<hash>` | GET | Look up a file hash on VirusTotal (MD5, SHA1, or SHA256) |
| `/api/intel/ip/<ip>` | GET | Look up an IP address reputation on AbuseIPDB |

---

## 📁 Project Structure

```
reezium-imager/
├── app.py                      # Entry point — CLI, Web Server, Desktop App modes
├── config.json                 # AI assistant + cloud threat intel configuration
├── requirements.txt            # Python dependencies
├── build_exe.bat               # PyInstaller build script → dist/ReeziumImager.exe (Windows)
├── build_linux.sh              # PyInstaller build script → dist/reezium-linux (Linux)
├── run_admin.bat               # Windows admin launcher
├── winpmem.exe                 # WinPMEM memory acquisition driver (Windows)
├── avml                        # AVML memory acquisition tool (Linux)
│
├── core/
│   ├── __init__.py             # Exports engine + cpu_sampler singletons
│   ├── capture_engine.py       # Memory acquisition controller (WinPMEM/AVML wrapper)
│   ├── utils.py                # Shannon entropy + CpuSampler background thread
│   │
│   ├── ai/                     # ★ AI-powered threat explanation
│   │   ├── __init__.py         # Package docstring
│   │   └── llm_assistant.py    # LLM provider implementations (Gemini/OpenAI/Ollama),
│   │                           #   system prompt, TTL cache, LLMAssistant class
│   │
│   ├── cloud/                  # ★ Cloud threat intelligence enrichment
│   │   ├── __init__.py         # Package docstring
│   │   └── threat_intel.py     # VirusTotal v3 + AbuseIPDB v2 clients,
│   │                           #   TTL cache, rate limiter, ThreatIntelClient class
│   │
│   └── detection/
│       ├── __init__.py
│       ├── scorer.py           # Orchestrator — combines all signals into threat scores
│       ├── statistical.py      # Modified Z-Score (MAD) anomaly detection
│       ├── entropy.py          # Shannon entropy + bigram frequency analysis
│       ├── heuristics.py       # Signature, path, masquerading, reverse-shell rules
│       ├── ml_models.py        # Isolation Forest unsupervised ML detector
│       └── whitelist.json      # Known-good process whitelist (single source of truth)
│
├── routes/
│   ├── __init__.py             # Flask Blueprint registration
│   ├── dashboard.py            # Home page + /api/stats
│   ├── processes.py            # /api/processes
│   ├── connections.py          # /api/connections
│   ├── threat_hunt.py          # /api/suspicious + /api/suspicious/compare + /api/timeline
│   ├── threat_intel.py         # ★ /api/intel/hash + /api/intel/ip + /api/intel/status
│   ├── ai_assistant.py         # ★ /api/ai/explain + /api/ai/status
│   ├── memory.py               # /api/memory endpoints
│   ├── system.py               # /api/system
│   └── capture.py              # /api/capture/start + /api/capture/status
│
├── templates/
│   └── index.html              # Single-page dashboard UI
│
├── static/
│   ├── css/style.css           # Dashboard styles
│   ├── js/script.js            # Frontend logic, charts, polling
│   └── favicon.ico             # Application icon

```

---

## 🔬 Detection Methodology

### Modified Z-Score (Statistical)

Uses **Median Absolute Deviation (MAD)** instead of standard deviation, making it robust to the heavy-tailed, non-Gaussian distributions typical of process resource data. Based on Iglewicz & Hoaglin (1993).

### Isolation Forest (ML)

**Unsupervised** anomaly detector that requires no labelled malware dataset. Uses a warm-up strategy (3 scans) before producing detections. Features: CPU%, memory%, thread count, connection count, name entropy, command-line length. Based on Liu, Ting, & Zhou (2008).

### Shannon Entropy + Bigram Analysis

Flags randomly-named processes (common in malware droppers and DGA) while exempting legitimate high-entropy names using English bigram frequency ratios.

### Heuristic Rules (MITRE ATT&CK Mapped)

| Rule | MITRE ID | Description |
|:-----|:---------|:------------|
| Suspicious Name | T1036 | Exact match against known malware tool names |
| Reverse Shell | T1059 | Shell process (cmd/powershell/bash) with active network connection |
| Suspicious Path | T1564 | Execution from Temp, Public, Recycle Bin, PerfLogs |
| Masquerading | T1036.005 | System process (svchost, lsass, etc.) running from wrong path |
| Process Chain | T1055 | Malicious parent-child combinations (e.g., Word → PowerShell) |
| Random Name | T1027 | High-entropy name with no English bigram structure |

### Cloud Enrichment (Post-Detection)

After local detection scores a threat, optional cloud enrichment adds real-time external intelligence:

- **VirusTotal (Automatic File Hashing)** — The engine automatically computes the SHA256 digest of the flagged process's on-disk executable (with chunked streaming and an in-memory `(path, mtime)` cache to prevent re-hashing unchanged binaries). It queries VirusTotal v3 for multi-engine detection stats and malware labels.
- **AbuseIPDB (Network Reputation)** — Enriches remote IP addresses from active network connections associated with the suspicious process, checking abuse confidence scores and report counts.

This intelligence is attached directly to each threat as `cloud_intel`, rendered as visual badges in the Threat Hunter UI, and injected into the AI assistant prompt for deep context-aware forensic analysis.

---

## ⚠️ Disclaimer

This tool is intended for **authorized forensic investigations, security research, and educational purposes only**. Memory acquisition requires administrator privileges and should only be performed on systems you own or have explicit authorization to investigate. Misuse of memory acquisition tools may violate local laws and regulations.

---

## 📄 License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for the full text.
