# 🔬 LogLens 2.0

> **Don't spend money to analyze your logs.**
> **Do it all free, on your machine, locally — no data ever leaves your browser.**

[![Live Demo](https://img.shields.io/badge/🔬_Live_Demo-GitHub_Pages-00d4ff?style=for-the-badge)](https://zrnge.github.io/LogLens/)
[![No Backend](https://img.shields.io/badge/Backend-None-00ff88?style=for-the-badge&logo=checkmarx&logoColor=white)](https://zrnge.github.io/LogLens/)
[![Privacy](https://img.shields.io/badge/Privacy-100%25_Local-00ff88?style=for-the-badge&logo=shield&logoColor=white)](#)
[![AI Powered](https://img.shields.io/badge/AI-Ollama_Local-b06dff?style=for-the-badge&logo=ollama&logoColor=white)](#local-ai-analyst)
[![Single File](https://img.shields.io/badge/Single_File-HTML_Only-ff8c42?style=for-the-badge&logo=html5&logoColor=white)](#)
[![No Tracking](https://img.shields.io/badge/Tracking-Zero-ff4d6d?style=for-the-badge&logo=privacybadger&logoColor=white)](#)
[![License](https://img.shields.io/badge/License-MIT-ffd700?style=for-the-badge)](#license)
[![Made with Love](https://img.shields.io/badge/Made_with-❤_&_Claude-accent?style=for-the-badge&color=0d1420)](#)

---

**LogLens** is a universal log explorer that runs entirely in your browser. Drop any log file in, query it with KQL, visualize it on a timeline, and analyze it with a local AI — all without a single byte of your data touching the internet.

No servers. No SaaS. No subscriptions. Just open the HTML file.

---

## ✨ Features

### 🔍 Powerful Query Engine
- **KQL syntax** — `level:ERROR AND service:auth*`
- **Nested fields** — select, filter, sort, and export object children such as `request/user/name:alice` or `request/latency:>=100`. Arrays containing objects expose indexed paths such as `items/0/name`; whole objects and arrays can also be displayed as JSON.
- **Boolean logic** — `AND`, `OR`, `NOT`, parentheses
- **Ranges** — `status_code:>=500`, `latency_ms:<=100`
- **Wildcards** — `message:*timeout*`, `src_ip:10.0.*`; use **Wildcard** in the top toolbar to hide or show the KQL query panel. Hiding the panel preserves the active query and its matching behavior.
- **Regex** — `message:/timeout|refused/i`, `src_ip:/^192\.168\./`
- **Free text search** — type anything, searches all fields

### 📊 Visualization
- **Timeline histogram** — canvas-based, color-coded by log level
- **Drag to select** a time range directly on the chart
- **Editable time selection** — drag the selection body to move it without changing its duration, or drag either edge to adjust start/end. Endpoint timestamps appear in floating labels while editing. Shift-drag creates a new selection. Tick labels adapt to the available chart width without overlapping.
- **Resizable timeline** — drag its bottom edge to adjust height; double-click the handle to restore the default. Selection continues outside the chart, and Esc cancels it. The floating ↺ button resets the time filter without resizing the chart.
- **Quick time filters** — Last 5m / 30m / 1h / 6h / 24h / 7d / All
- **Custom from/to** datetime pickers
- **Field value distribution** bars (top-10 values per field, click to expand)

### ⚡ Performance
- **True virtual scroll** — renders only visible rows, handles 500k+ logs without slowdown
- **Incremental loading** — load multiple files, they merge automatically
- **Dedup mode** — collapse repeated identical messages

### 📁 Universal Format Support
| Format | Details |
|--------|---------|
| **JSON / NDJSON** | Arrays and newline-delimited |
| **CSV** | Auto-header detection, quoted fields |
| **AWS log export CSV** | Each CSV record becomes one log event, including multiline messages and stack traces. Unwraps `timestamp,message` (also `@timestamp,@message`), discards the outer timestamp, and parses message JSON or text. Timestamps inside messages are preserved. |
| **Syslog** | RFC 3164 — `host proc[pid]: msg` |
| **Apache / Nginx** | Access logs + error logs |
| **Log4j / Log4net** | `timestamp [thread] LEVEL class - msg` |
| **CEF / LEEF** | Common/Log Event Extended Format |
| **logfmt** | `key=value key="quoted value"` |
| **GELF** | Graylog Extended Log Format |
| **W3C / IIS** | `#Fields:` extended log format |
| **Docker / k8s** | Timestamped container logs |
| **Windows Event XML** | Exported from Event Viewer or PowerShell |
| **.evtx binary** | Shows conversion guide with PowerShell/wevtutil commands |
| **Plain text** | Any `.log` / `.txt` — every line becomes a row |

### 🪟 Windows Event Logs (.evtx)
LogLens fully parses **Windows XML Event Log** format. For binary `.evtx` files, it detects the format and shows an instant conversion guide:
```powershell
# Export to JSON (simplest)
Get-WinEvent -Path "C:\path\to\your.evtx" |
  Select-Object TimeCreated,Id,LevelDisplayName,Message,ProviderName,MachineName |
  ConvertTo-Json -Depth 3 | Out-File -Encoding UTF8 events.json

# Export to XML (preserves all fields — drop directly into LogLens)
$events = Get-WinEvent -Path "C:\path\to\your.evtx"
"<Events>" | Out-File events.xml -Encoding UTF8
$events | ForEach-Object { $_.ToXml() } | Out-File events.xml -Append -Encoding UTF8
"</Events>" | Out-File events.xml -Append -Encoding UTF8
```

### 🤖 Local AI Analyst
**Zero-cost AI analysis. No API keys. No cloud. Everything stays on your machine.**

Powered by [Ollama](https://ollama.com), the AI panel lets you chat with your logs:

| Action | What it does |
|--------|-------------|
| 🔍 **Analyze** | Top errors, patterns, anomalies, cascading failures |
| 📋 **Report** | Full incident report — summary, timeline, root cause, actions |
| 💡 **KQL Hints** | Suggests 8-10 investigative queries you can apply with one click |
| 🔥 **Root Cause** | Identifies the most likely root cause from evidence in the logs |
| 🛡 **Security Audit** | Suspicious IPs, brute force, privilege escalation, exfiltration |
| 📊 **Stats** | Error rate, latency p95, volume trends, per-service breakdowns |

**AI is context-aware** — injects your current filter, field names, level distribution, top errors, and up to 100 log sample entries into every request.

**Context window control** — choose 4K / 8K / 16K / 32K / 128K tokens and how many log entries to include. A live token meter shows usage and auto-prunes conversation history before overflow.

**Stop button** — abort mid-stream at any time. Partial responses are saved to context.

### 🧭 Navigation & UX
- **Compact, square-edged UI** — denser controls and scrollable toolbars keep more space available for logs.
- **Collapsible top toolbar** — expanded by default; use **Toolbar** at the upper right to hide or restore the logo, statistics, and controls.
- **Keyboard-first** — `j/k` rows, `/` search, `m` bookmark, `d` theme, `?` help. Ctrl+C / Cmd+C copies selected text normally; use the detail buttons to copy a complete JSON or CSV row.
- **Detail panel** — Fields / formatted JSON / Raw / Tree tabs, prev/next row navigation. JSON uses indentation and structural line breaks; Fields and Tree preserve multiline string values. Mixed text/JSON logs keep all events and recognize each JSON record.
- **Column picker** — switch between a folder-style Tree and an ordered List, search paths, and choose up to 12 columns. Selected list items follow the table's left-to-right order; drag their left-hand grips to reorder. Changes preview immediately; Apply keeps them and Cancel restores the previous columns.
- **Saved column layouts** — in Column Picker, enter a name and choose Save to store visible columns, their order and widths, row sorting, and Tree/List view in this browser. Select a saved layout and choose Load to preview it; Apply keeps it, while Cancel restores the previous layout. Reusing a name overwrites that layout. Fields absent from the current logs are skipped.
- **Local layout files** — choose **Save File** to download the current layout as JSON, and **Load File** to preview a previously exported file. Files contain layout settings only, not log contents. Importing does not overwrite browser-saved layouts; use Save afterward if you want a browser copy.
- **Column order** — drag a table header's grip to move it left or right, independently of the resize handle. Grips also support arrow keys. Nested paths use colored `/` separators.
- **Retained column preferences** — Clear, file removal, and new log imports keep column selection, order, widths, and sorting. Missing fields show `—` and remain listed as unavailable in the picker. The last committed layout is remembered in this browser across page refreshes; picker previews are not remembered until Apply.
- **Paste replacement option** — Paste Log Text includes a checked-by-default option to clear existing logs before loading. Uncheck it to append instead. The browser remembers the last choice; replacing logs preserves column preferences, and empty input never clears logs.
- **Quick import** — Load/Browse Files opens the file picker immediately; after selection, confirm the import with the clear-or-append checkbox. Drop files anywhere on the start page or viewer to open the same import dialog. On the start page, Ctrl+V / ⌘V opens Paste Log Text with the clipboard text already filled in.
- **File replacement option** — The file import dialog remembers its clear-or-append choice independently from Paste. A multi-file batch clears old logs once, only after its first usable file is parsed; empty or unreadable files do not clear logs.
- **Resizable columns** — drag the right edge of a column header (40px minimum); double-click to restore its default width. Focus the handle and use arrow keys to adjust, or Home to reset.
- **Resizable panels** — drag handles on sidebar and detail panel
- **Saved queries** — name and persist your favourite KQL queries
- **Query history** — last 20 queries, one click to re-run
- **Bookmarks** — star rows with `m`, filter to starred-only
- **Export** — JSON or CSV of current filtered results
- **Light / Dark theme** — persisted to localStorage

---

## 🚀 Quick Start

### Option 1 — GitHub Pages (online, still 100% local)
Visit the live deployment. Everything runs in **your** browser — no data leaves your machine.

```
https://zrnge.github.io/LogLens/
```

### Option 2 — Download and open locally (best for Ollama AI)
1. Download [LogLens.html](LogLens.html)
2. Open it directly in your browser — no server needed
3. Drop your log files in

### Option 3 — Clone and use
```bash
git clone https://github.com/zrnge/LogLens.git
cd LogLens
# Just open LogLens.html in a browser
start LogLens.html       # Windows
open LogLens.html        # Mac
xdg-open LogLens.html    # Linux
```

---

## 🤖 Setting Up Ollama (Local AI)

1. **Install Ollama** — [ollama.com](https://ollama.com) (free, Windows / Mac / Linux)

2. **Pull a model:**
   ```bash
   ollama pull mistral        # default — fast, good quality (~4GB)
   ollama pull llama3.2       # great for analysis (~2GB)
   ollama pull deepseek-r1:7b # strong reasoning
   ollama pull phi3           # lightweight (~2GB)
   ```

3. **Start Ollama with CORS allowed** (required for browser access):

   **Windows CMD:**
   ```cmd
   set OLLAMA_ORIGINS=*&& ollama serve
   ```

   **Windows PowerShell:**
   ```powershell
   $env:OLLAMA_ORIGINS="*"; ollama serve
   ```

   **Mac / Linux:**
   ```bash
   OLLAMA_ORIGINS=* ollama serve
   ```

   > **Tip (Windows):** Set it permanently so you never need to think about it again:
   > ```powershell
   > [System.Environment]::SetEnvironmentVariable("OLLAMA_ORIGINS","*","User")
   > ```

4. **Click AI Analyst** in LogLens → **Connect** → start asking questions

---

## ⌨️ Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `j` / `↓` | Next row |
| `k` / `↑` | Previous row |
| `Enter` | Open detail panel for selected row |
| `Esc` | Close detail / modal |
| `/` | Focus search bar |
| `Ctrl+Enter` | Run KQL query |
| `m` | Bookmark selected row |
| `w` | Toggle line wrap |
| `d` | Toggle dark / light theme |
| `t` | Toggle timeline |
| `?` | Show keyboard help |

---

## 💡 KQL Query Examples

```
# All errors and criticals
level:ERROR OR level:CRITICAL

# HTTP server errors
status_code:>=500

# Failed authentication (wildcard)
message:*Invalid* OR message:*denied* OR message:*failed*

# Slow requests
latency_ms:>=1000

# Specific service in production
service:auth* AND env:production

# Regex — find IPs in a subnet
src_ip:/^192\.168\./

# Exclude health checks
NOT url:/health AND status_code:>=400

# Combine everything
level:ERROR AND service:api-gateway AND latency_ms:>=500 AND NOT method:OPTIONS
```

---

## 🏗️ Architecture

Run the dependency-free column regression tests with `node tests/columns.test.cjs`. They cover nested fields, query behavior, sorting/export, picker transactions, and column ordering; they use a DOM double and do not check browser layout.

LogLens is intentionally a **single HTML file** (~2,500 lines). No build step, no npm, no dependencies, no CDN (except optional Google Fonts for the UI font — falls back to system monospace).

```
LogLens.html
├── CSS (theme variables, virtual scroll, AI panel, modals)
├── HTML (upload overlay, topbar, timeline, KQL bar, sidebar, log table, detail, AI panel)
└── JavaScript (pure vanilla ES2020)
    ├── Auto-parser (12 formats)
    ├── KQL engine (AND/OR/NOT/wildcards/ranges/regex)
    ├── Virtual scroll (26px fixed rows, buffer rendering)
    ├── Canvas timeline histogram
    ├── Ollama streaming chat client
    └── localStorage persistence (history, saved queries, theme)
```

**Why a single file?**
- Zero setup for end users
- Works offline once cached
- Can be bookmarked, downloaded, and shared as a file attachment
- Zero supply-chain risk

---

## 🔒 Privacy Guarantee

LogLens is architecturally incapable of sending your data anywhere:
- No `fetch()` calls to external servers (only to `localhost:11434` for local Ollama)
- No analytics, no tracking pixels, no beacons
- No cookies (localStorage only, stays on your device)
- No service workers that could cache and transmit data
- Open source — read every line of the single HTML file yourself

---

## 👨‍💻 Developer

Built by **[zrnge](https://github.com/zrnge)** — open source, free forever.

[![GitHub](https://img.shields.io/badge/GitHub-zrnge-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/zrnge)
[![LogLens Live](https://img.shields.io/badge/🔬_LogLens-Live_App-00d4ff?style=for-the-badge)](https://zrnge.github.io/LogLens/)

---

## 📄 License

MIT — free to use, modify, and distribute.

---

<div align="center">

**🔬 LogLens 2.0** — built by [zrnge](https://github.com/zrnge) for engineers who believe their logs are their own business.

*Don't spend money to analyze your logs.*
*Do it all free, on your machine, locally — no data ever leaves your browser.*

[![Star on GitHub](https://img.shields.io/github/stars/zrnge/LogLens?style=social)](https://github.com/zrnge/LogLens)

</div>
