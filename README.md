[![Test SysMon](https://github.com/strabo231/sysmon/actions/workflows/test.yml/badge.svg)](https://github.com/strabo231/sysmon/actions/workflows/test.yml)

# SysMon - System Resource Monitor

Make `top` and `htop` look ancient! SysMon is a beautiful, intuitive system monitor for the command line.

## Why SysMon?

**The Problem:** System monitoring tools suck
- `top` → Confusing, ugly, hard to read
- `htop` → Better but still cluttered
- GUI tools → Slow, resource-heavy

**The Solution:** SysMon - clean, fast, intuitive

## Installation

```bash
curl -sSL https://raw.githubusercontent.com/strabo231/sysmon/main/install.sh | bash
```

## Quick Start

```bash
# Dashboard view
sysmon

# Process list
sysmon process

# Top consumers
sysmon top

# Network activity
sysmon network

# Live monitoring
sysmon watch
```

## Commands

```
sysmon              Dashboard view (default)
sysmon process      List all processes
sysmon top          Top CPU/memory consumers
sysmon network      Network connections
sysmon disk         Disk usage and I/O
sysmon kill <pid>   Kill a process
sysmon watch        Live monitoring mode
```

## Features

📊 **Beautiful Dashboard** - CPU, RAM, Disk at a glance  
🎨 **Color-coded alerts** - Green/Yellow/Red based on usage  
📈 **Progress bars** - Visual usage indicators  
🔍 **Process management** - List, sort, search, kill  
🌐 **Network monitoring** - Active connections, interfaces  
💾 **Disk I/O** - Usage and statistics  
⚡ **Live updates** - Real-time monitoring mode  
🚀 **Fast & lightweight** - Won't slow your system  

## Dashboard View

```bash
sysmon
```

```
═══════════════════════════════════════════════════════════════
                      SYSTEM MONITOR
═══════════════════════════════════════════════════════════════

System: myserver | Uptime: 5 days, 3 hours
Kernel: 6.5.0-15-generic

CPU Usage: 23%
[███████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░]

Memory Usage: 45% (7.2GB/16GB)
[██████████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░]

Disk Usage: 68% (340GB/500GB)
[██████████████████████████████████░░░░░░░░░░░░░░░░]

Load Average: 0.89, 0.65, 0.48

TOP PROCESSES (by CPU)

PID      USER                 CPU%     MEM%     COMMAND
────────────────────────────────────────────────────────────────────
1234     node                 12.3%    4.2%     node server.js
5678     mysql                8.1%     15.6%    /usr/sbin/mysqld
```

## Process Management

**List all processes:**
```bash
sysmon process
```

**Top consumers:**
```bash
sysmon top
```

Shows both CPU and memory top 10.

**Kill a process:**
```bash
sysmon kill 1234
```
Confirms before killing (safe!).

## Network Monitoring

```bash
sysmon network
```

Shows:
- Network interfaces with stats
- Active connections
- Connection states
- Total connection count

## Disk Monitoring

```bash
sysmon disk
```

Shows:
- Filesystem usage
- Disk I/O statistics (if iostat available)
- Mount points

## Live Monitoring

```bash
sysmon watch
```

Dashboard updates every 2 seconds. Press Ctrl+C to exit.

## Color Coding

SysMon uses traffic light colors:
- 🟢 **Green** (0-69%): Normal
- 🟡 **Yellow** (70-89%): Warning
- 🔴 **Red** (90-100%): Critical

## Use Cases

**Quick System Check:**
```bash
sysmon
```
One command, full picture.

**Find Resource Hog:**
```bash
sysmon top
```
Instantly see what's eating resources.

**Monitor Production Server:**
```bash
sysmon watch
```
Real-time monitoring.

**Kill Frozen Process:**
```bash
sysmon process | grep myapp
sysmon kill 1234
```

**Debug Network Issues:**
```bash
sysmon network
```
See all active connections.

## Comparison

| Feature | top | htop | SysMon |
|---------|-----|------|---------|
| Easy to read | ❌ | ⚠️ | ✅ |
| Color-coded | ❌ | ✅ | ✅ |
| Progress bars | ❌ | ✅ | ✅ |
| Network view | ❌ | ❌ | ✅ |
| Disk I/O | ❌ | ❌ | ✅ |
| Dashboard | ❌ | ❌ | ✅ |
| Installation | Built-in | apt install | One-liner |

## Requirements

- Bash 4.0+
- Standard Linux tools (ps, free, df, uptime)
- Optional: iostat (for disk I/O stats)

## Tips

**Add to .bashrc for quick access:**
```bash
alias sm='sysmon'
alias smt='sysmon top'
alias smw='sysmon watch'
```

**Monitor specific user:**
```bash
sysmon process | grep username
```

**Find port users:**
```bash
sysmon network | grep :8080
```

## License

MIT License - see [LICENSE](LICENSE)

## Author

Sean - [@strabo231](https://github.com/strabo231)

---

**Monitor your system. The beautiful way.** 📊
