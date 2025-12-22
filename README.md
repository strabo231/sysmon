[![Test SysMon](https://github.com/strabo231/sysmon/actions/workflows/test.yml/badge.svg)](https://github.com/strabo231/sysmon/actions/workflows/test.yml)

# SysMon - System Resource Monitor

Make `top` and `htop` look ancient! SysMon is a beautiful, intuitive system monitor for the command line.

## 🆕 What's New in v1.1.0

**🎮 GPU Monitoring** - Full support for NVIDIA, AMD, and Intel GPUs
- Real-time GPU utilization and memory usage
- Temperature and power monitoring
- GPU process tracking
- Visual progress bars for all metrics
- Automatic GPU detection

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

# GPU monitoring
sysmon gpu

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
sysmon gpu          GPU monitoring (NVIDIA/AMD/Intel)
sysmon network      Network connections
sysmon disk         Disk usage and I/O
sysmon kill <pid>   Kill a process
sysmon watch        Live monitoring mode
```

## Features

📊 **Beautiful Dashboard** - CPU, RAM, Disk, GPU at a glance  
🎨 **Color-coded alerts** - Green/Yellow/Red based on usage  
📈 **Progress bars** - Visual usage indicators  
🎮 **GPU Monitoring** - NVIDIA, AMD, and Intel support  
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
═══════════════════════════════════════════════════════════
                      SYSTEM MONITOR
═══════════════════════════════════════════════════════════

System: myserver | Uptime: 5 days, 3 hours
Kernel: 6.5.0-15-generic

CPU Usage: 23%
[████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░]

Memory Usage: 45% (7.2GB/16GB)
[██████████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░]

Disk Usage: 68% (340GB/500GB)
[██████████████████████████████████░░░░░░░░░░░░░░░░░░]

Load Average: 0.89, 0.65, 0.48

GPU (NVIDIA GeForce RTX 3080): 45% | 72°C

TOP PROCESSES (by CPU)

PID      USER                 CPU%     MEM%     COMMAND
────────────────────────────────────────────────────────────────────
1234     node                 12.3%    4.2%     node server.js
5678     mysql                8.1%     15.6%    /usr/sbin/mysqld
```

## 🎮 GPU Monitoring

### NVIDIA GPUs

SysMon provides comprehensive NVIDIA GPU monitoring with:
- Real-time GPU and memory utilization
- Temperature monitoring
- Power draw and limits
- Fan speed
- Active GPU processes

```bash
sysmon gpu
```

**Requirements:** `nvidia-smi` (included with NVIDIA drivers)

**Output:**
```
═══════════════════════════════════════════════════════════
                      GPU MONITORING
═══════════════════════════════════════════════════════════

NVIDIA GPU Detected

GPU 0: NVIDIA GeForce RTX 3080

GPU Utilization: 67%
[█████████████████████████████████░░░░░░░░░░░░░░░░░░░]

Memory Utilization: 45% (4532MB / 10240MB)
[██████████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░]

Temperature: 72°C

Power Draw: 245W / 320W (77%)

Fan Speed: 65%

────────────────────────────────────────────────────────────────────

GPU Processes:

GPU      PID      TYPE     SM %       MEM %      NAME
────────────────────────────────────────────────────────────────────
0        12345    C        67%        45%        python
0        23456    G        12%        8%         Xorg
```

### AMD GPUs

AMD GPU monitoring supports:
- GPU utilization via sysfs or ROCm
- Temperature monitoring
- Memory usage (with ROCm)

**Requirements:**
- Basic: Linux kernel with sysfs support
- Full features: `rocm-smi` (ROCm toolkit)

**Installation:**
```bash
# Ubuntu/Debian
sudo apt install rocm-smi

# Or follow: https://rocm.docs.amd.com/
```

### Intel GPUs

Intel GPU monitoring provides:
- Basic GPU information via sysfs
- Detailed stats with intel_gpu_top

**Requirements:**
- Basic: lspci
- Full features: `intel-gpu-tools`

**Installation:**
```bash
# Ubuntu/Debian
sudo apt install intel-gpu-tools

# Fedora
sudo dnf install intel-gpu-tools

# Run with: sudo intel_gpu_top
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

**Monitor GPU During Training:**
```bash
sysmon gpu
# Or for live monitoring:
sysmon watch
```
Perfect for ML/AI workloads.

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

| Feature | top | htop | nvidia-smi | SysMon |
|---------|-----|------|------------|---------|
| Easy to read | ❌ | ⚠️ | ⚠️ | ✅ |
| Color-coded | ❌ | ✅ | ❌ | ✅ |
| Progress bars | ❌ | ✅ | ❌ | ✅ |
| GPU monitoring | ❌ | ❌ | ✅ | ✅ |
| Multi-GPU | ❌ | ❌ | ✅ | ✅ |
| GPU processes | ❌ | ❌ | ✅ | ✅ |
| Network view | ❌ | ❌ | ❌ | ✅ |
| Disk I/O | ❌ | ❌ | ❌ | ✅ |
| Dashboard | ❌ | ❌ | ❌ | ✅ |
| Installation | Built-in | apt install | Built-in | One-liner |

## Requirements

- Bash 4.0+
- Standard Linux tools (ps, free, df, uptime)
- Optional: iostat (for disk I/O stats)
- Optional: nvidia-smi (for NVIDIA GPU monitoring)
- Optional: rocm-smi (for AMD GPU monitoring)
- Optional: intel-gpu-tools (for Intel GPU monitoring)

## Tips

**Add to .bashrc for quick access:**
```bash
alias sm='sysmon'
alias smg='sysmon gpu'
alias smt='sysmon top'
alias smw='sysmon watch'
```

**Monitor GPU during ML training:**
```bash
watch -n 1 sysmon gpu
```

**Monitor specific user:**
```bash
sysmon process | grep username
```

**Find port users:**
```bash
sysmon network | grep :8080
```

**Check GPU before starting job:**
```bash
sysmon gpu && python train.py
```

## Troubleshooting

### GPU not detected

**NVIDIA:**
```bash
# Check if nvidia-smi is available
nvidia-smi

# If not, install NVIDIA drivers
sudo ubuntu-drivers autoinstall  # Ubuntu
```

**AMD:**
```bash
# Check GPU
lspci | grep -i vga

# Install ROCm for full features
# https://rocm.docs.amd.com/
```

**Intel:**
```bash
# Install tools
sudo apt install intel-gpu-tools

# Run as root for full access
sudo intel_gpu_top
```

## License

MIT License - see [LICENSE](LICENSE)

## Author

Sean - [@strabo231](https://github.com/strabo231)

## Changelog

### v1.1.0 (2024)
- ✨ Added GPU monitoring for NVIDIA, AMD, and Intel
- 📊 GPU utilization and memory tracking
- 🌡️ Temperature and power monitoring
- 🎮 GPU process tracking
- 📈 Visual progress bars for GPU metrics
- 🔍 Automatic GPU detection

### v1.0.0 (2024)
- 🎉 Initial release
- 📊 Dashboard view
- 💻 Process management
- 🌐 Network monitoring
- 💾 Disk I/O monitoring
- ⚡ Live monitoring mode

---

**Monitor your system. The beautiful way.** 📊🎮
