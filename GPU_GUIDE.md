# GPU Monitoring Guide

Complete guide to GPU monitoring with SysMon v1.1.0

## Quick Start

```bash
# View GPU status
sysmon gpu

# Include in live monitoring
sysmon watch
```

## Supported GPUs

### ✅ NVIDIA GPUs (Full Support)

**Features:**
- Real-time GPU utilization (%)
- Memory usage (MB/GB)
- Temperature (°C)
- Power draw (W)
- Fan speed (%)
- Active GPU processes with PID tracking
- Multi-GPU support

**Requirements:**
- NVIDIA GPU with drivers installed
- `nvidia-smi` command available

**Installation (if needed):**
```bash
# Ubuntu/Debian
sudo ubuntu-drivers autoinstall
# or
sudo apt install nvidia-driver-XXX

# Verify
nvidia-smi
```

**Example Output:**
```
NVIDIA GPU Detected

GPU 0: NVIDIA GeForce RTX 3080

GPU Utilization: 67%
[█████████████████████████████████░░░░░░░░░░░░░░░░░░░]

Memory Utilization: 45% (4532MB / 10240MB)
[██████████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░]

Temperature: 72°C
Power Draw: 245W / 320W (77%)
Fan Speed: 65%
```

---

### ⚠️ AMD GPUs (Basic/Full Support)

**Features:**
- GPU utilization
- Temperature monitoring
- Memory usage (with ROCm)

**Requirements:**
- AMD GPU
- Basic: Linux sysfs support
- Full: ROCm toolkit with `rocm-smi`

**Installation:**
```bash
# ROCm (for full features)
# Follow: https://rocm.docs.amd.com/

# Ubuntu example:
sudo apt install rocm-smi
```

**Fallback:** If ROCm not available, basic info via sysfs

---

### ℹ️ Intel GPUs (Basic Support)

**Features:**
- GPU detection
- Basic information
- Temperature (if available)

**Requirements:**
- Intel integrated graphics
- `lspci` for detection
- `intel-gpu-tools` for detailed stats

**Installation:**
```bash
# Ubuntu/Debian
sudo apt install intel-gpu-tools

# Fedora
sudo dnf install intel-gpu-tools

# Usage
sudo intel_gpu_top
```

---

## Usage Examples

### Monitor GPU During ML Training

```bash
# Terminal 1: Start training
python train.py

# Terminal 2: Monitor GPU
sysmon gpu

# Or use watch for continuous updates
watch -n 1 'sysmon gpu'
```

### Check GPU Before Job

```bash
# Ensure GPU is available
sysmon gpu && echo "GPU ready!" && ./run_job.sh
```

### Live Dashboard with GPU

```bash
# Shows CPU, RAM, Disk, GPU all at once
sysmon watch
```

### Find GPU Process

```bash
# Full GPU info
sysmon gpu

# Just the processes
sysmon gpu | grep -A 20 "GPU Processes"
```

### Multiple GPUs

```bash
# NVIDIA automatically shows all GPUs
sysmon gpu

# Output shows:
# GPU 0: NVIDIA GeForce RTX 3080
# GPU 1: NVIDIA GeForce RTX 3090
# ...
```

---

## Understanding the Output

### GPU Utilization

The percentage of time the GPU was actively processing:
- **0-30%**: Light load (idle, web browsing)
- **30-70%**: Moderate load (gaming, rendering)
- **70-100%**: Heavy load (ML training, intensive compute)

### Memory Utilization

GPU VRAM usage:
- Shows used/total memory
- Important for ML models and rendering
- High memory = might OOM on larger tasks

### Temperature

GPU chip temperature:
- **<70°C**: Cool (green)
- **70-85°C**: Warm (yellow)
- **>85°C**: Hot (red) - check cooling

### Power Draw

Current power consumption:
- Shows watts used vs. limit
- High power = high performance
- Useful for power budget monitoring

### GPU Processes

Active programs using the GPU:
- **Type C**: Compute (ML, mining, etc.)
- **Type G**: Graphics (games, desktop)
- **SM %**: Streaming Multiprocessor usage
- **MEM %**: Memory usage

---

## Troubleshooting

### "No supported GPU detected"

**Check GPU is present:**
```bash
lspci | grep -i vga
```

**NVIDIA:**
```bash
# Test nvidia-smi
nvidia-smi

# If not found, install drivers
sudo ubuntu-drivers devices
sudo ubuntu-drivers autoinstall
```

**AMD:**
```bash
# Check sysfs
ls /sys/class/drm/card*/device/vendor

# Should show "0x1002" for AMD

# Install ROCm for full features
```

**Intel:**
```bash
# Install tools
sudo apt install intel-gpu-tools

# Run as root
sudo intel_gpu_top
```

### GPU Shows 0% Usage

**Possible causes:**
1. No GPU workload running
2. GPU idle
3. Driver issue (restart system)
4. Monitoring delay (wait a few seconds)

### Can't See GPU Processes

**NVIDIA:** Make sure you have permissions:
```bash
# Try as root
sudo sysmon gpu
```

**AMD/Intel:** Process tracking limited - use `radeontop` or `intel_gpu_top`

### Temperature Shows [N/A]

Some GPUs don't report all metrics. This is normal for:
- Older GPUs
- Some laptop GPUs
- Limited driver support

---

## Integration Tips

### Alias for Quick Access

```bash
# Add to ~/.bashrc or ~/.zshrc
alias gpu='sysmon gpu'
alias gpuw='watch -n 1 sysmon gpu'
```

### Pre-commit Hook

```bash
# Check GPU before starting job
if sysmon gpu | grep -q "GPU Utilization: [0-9]%"; then
    echo "GPU available, starting job..."
    ./train.py
else
    echo "GPU busy or unavailable"
fi
```

### Monitoring Script

```bash
#!/bin/bash
# monitor_training.sh

while true; do
    clear
    sysmon gpu
    sleep 2
done
```

### Systemd Service

Monitor GPU continuously:
```ini
[Unit]
Description=GPU Monitor

[Service]
ExecStart=/usr/local/bin/sysmon watch
Restart=always

[Install]
WantedBy=multi-user.target
```

---

## FAQ

**Q: Why use SysMon instead of nvidia-smi?**
A: SysMon provides a cleaner, color-coded interface with progress bars. Plus it integrates GPU with CPU/RAM/Disk monitoring.

**Q: Can I monitor remote GPUs?**
A: Yes! SSH into the server and run `sysmon gpu`

**Q: Does it work with WSL2?**
A: Yes! As long as you have NVIDIA CUDA on WSL2 set up.

**Q: Multiple GPUs?**
A: Yes, NVIDIA automatically shows all GPUs. For AMD/Intel, might require manual checking.

**Q: Overhead?**
A: Minimal. Queries every 2 seconds in watch mode, negligible impact.

**Q: Can I export GPU data?**
A: Currently displays only. Future versions may add JSON export.

---

## Coming Soon

- 📊 GPU history graphs
- 📈 Memory timeline
- 🔔 Alert thresholds
- 💾 Data export (JSON/CSV)
- 🎮 Per-process GPU breakdown
- 🌡️ Fan curve display

---

For more info, visit: https://github.com/strabo231/sysmon
