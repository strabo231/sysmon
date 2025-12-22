# Quick Setup Guide - SysMon v1.1.0 GPU Monitoring

Get GPU monitoring up and running in 5 minutes!

## 🚀 Quick Install

```bash
# One command to rule them all
curl -sSL https://raw.githubusercontent.com/strabo231/sysmon/main/install.sh | bash

# Verify
sysmon --version
# Should show: sysmon version 1.1.0
```

## ✅ GPU Setup by Manufacturer

### NVIDIA (Recommended - Full Features)

**1. Check if you already have drivers:**
```bash
nvidia-smi
```

If this works, you're done! Skip to Testing section.

**2. If not, install drivers:**

**Ubuntu/Debian:**
```bash
# Auto-detect and install
sudo ubuntu-drivers autoinstall

# Or list available drivers
sudo ubuntu-drivers devices

# Install specific version
sudo apt install nvidia-driver-535
```

**Fedora:**
```bash
sudo dnf install akmod-nvidia
```

**3. Reboot:**
```bash
sudo reboot
```

**4. Verify:**
```bash
nvidia-smi
# Should show your GPU info
```

---

### AMD (Basic Works Out-of-Box, ROCm for Full)

**Basic (Works Immediately):**
```bash
# No setup needed!
sysmon gpu
# Will show basic info via sysfs
```

**Full Features (Optional - Install ROCm):**

**Ubuntu 22.04:**
```bash
# Add repo
wget https://repo.radeon.com/amdgpu-install/latest/ubuntu/jammy/amdgpu-install_*_all.deb
sudo dpkg -i amdgpu-install_*_all.deb

# Install ROCm
sudo amdgpu-install --usecase=rocm

# Reboot
sudo reboot

# Verify
rocm-smi
```

For other distros: https://rocm.docs.amd.com/

---

### Intel (Basic Out-of-Box)

**Basic (Works Immediately):**
```bash
sysmon gpu
# Shows detection and basic info
```

**Full Features (Optional):**
```bash
# Ubuntu/Debian
sudo apt install intel-gpu-tools

# Fedora
sudo dnf install intel-gpu-tools

# Use with sudo for full access
sudo intel_gpu_top
```

---

## 🧪 Testing

### Test GPU Detection
```bash
sysmon gpu
```

**Expected Output - NVIDIA:**
```
═══════════════════════════════════════════════════════════
                      GPU MONITORING
═══════════════════════════════════════════════════════════

NVIDIA GPU Detected

GPU 0: NVIDIA GeForce RTX 3080

GPU Utilization: 0%
[░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░]
...
```

**Expected Output - AMD:**
```
═══════════════════════════════════════════════════════════
                      GPU MONITORING
═══════════════════════════════════════════════════════════

AMD GPU Detected

GPU 0: AMD Radeon RX 6800
  Usage: 0%
  Temp: 45°C
```

**Expected Output - No GPU:**
```
⚠ No supported GPU detected or drivers not installed
...
```

### Test Dashboard
```bash
sysmon
```

Should show GPU line (if NVIDIA):
```
GPU (NVIDIA GeForce RTX 3080): 0% | 45°C
```

### Test Live Monitoring
```bash
sysmon watch
```

Updates every 2 seconds with GPU included.

---

## 🎮 Real-World Test

### Generate GPU Load

**NVIDIA:**
```bash
# Run stress test
nvidia-smi -l 1  # In one terminal

# Monitor with SysMon in another
sysmon gpu       # In another terminal
```

**Or run a compute task:**
```bash
# Install CUDA samples
git clone https://github.com/NVIDIA/cuda-samples
cd cuda-samples/Samples/1_Utilities/deviceQuery
make
./deviceQuery

# While running something heavy, check:
sysmon gpu
```

---

## 🔧 Troubleshooting

### GPU Not Detected

**1. Check hardware:**
```bash
lspci | grep -i vga
# Should list your GPU
```

**2. Check drivers:**
```bash
# NVIDIA
nvidia-smi

# AMD
ls /sys/class/drm/card*/device/vendor
cat /sys/class/drm/card0/device/vendor
# Should show "0x1002" for AMD

# Intel
lspci | grep -i vga | grep -i intel
```

**3. Check dmesg for errors:**
```bash
dmesg | grep -i gpu
dmesg | grep -i nvidia
dmesg | grep -i amd
```

### NVIDIA Driver Issues

**Common fixes:**
```bash
# Purge old drivers
sudo apt purge nvidia-*
sudo apt autoremove

# Reinstall
sudo ubuntu-drivers autoinstall
sudo reboot

# Verify
nvidia-smi
```

### AMD ROCm Issues

**Check kernel version:**
```bash
uname -r
# ROCm requires kernel 5.x or newer
```

**Check GPU compatibility:**
```bash
rocm-smi --showproductname
```

### Permission Issues

**Some commands need sudo:**
```bash
# If regular user can't see GPU processes
sudo sysmon gpu

# Add user to video group (AMD/Intel)
sudo usermod -a -G video $USER
# Logout and login again
```

---

## 📱 Quick Reference

```bash
# GPU only
sysmon gpu

# Full dashboard with GPU
sysmon

# Live monitoring with GPU
sysmon watch

# GPU + process info
sysmon gpu | less

# Just GPU processes (NVIDIA)
sysmon gpu | grep -A 10 "GPU Processes"

# Check version
sysmon --version
```

---

## ⚡ Pro Tips

**1. Create aliases:**
```bash
echo "alias gpu='sysmon gpu'" >> ~/.bashrc
echo "alias gpuw='watch -n 1 sysmon gpu'" >> ~/.bashrc
source ~/.bashrc

# Now just:
gpu
gpuw
```

**2. Monitor during work:**
```bash
# Split terminal
tmux new-session \; split-window -h \; \
  send-keys 'python train.py' C-m \; \
  select-pane -t 1 \; \
  send-keys 'sysmon watch' C-m
```

**3. Log GPU stats:**
```bash
while true; do 
  echo "$(date): $(sysmon gpu | grep 'GPU Utilization')" >> gpu.log
  sleep 60
done
```

**4. Alert on high temp:**
```bash
#!/bin/bash
while true; do
  temp=$(nvidia-smi --query-gpu=temperature.gpu --format=csv,noheader)
  if [ "$temp" -gt 80 ]; then
    notify-send "GPU Hot!" "Temperature: ${temp}°C"
  fi
  sleep 30
done
```

---

## 🆘 Still Having Issues?

1. **Check GitHub Issues:** 
   https://github.com/strabo231/sysmon/issues

2. **Create New Issue:**
   Include:
   - OS and version (`lsb_release -a`)
   - GPU model (`lspci | grep VGA`)
   - Driver version (`nvidia-smi` or `rocm-smi --version`)
   - Error output
   - `sysmon --version` output

3. **Community:**
   - Check existing solutions
   - Ask questions
   - Share your setup

---

## ✨ Next Steps

Once GPU monitoring is working:

1. Try all commands:
   ```bash
   sysmon          # Dashboard
   sysmon gpu      # GPU details
   sysmon top      # Top processes
   sysmon network  # Network
   sysmon disk     # Disk I/O
   sysmon watch    # Live mode
   ```

2. Read the full guides:
   - `GPU_GUIDE.md` - Complete GPU documentation
   - `README.md` - Full feature list
   - `CHANGELOG.md` - What's new

3. Customize:
   - Add aliases
   - Create monitoring scripts
   - Set up alerts

---

Happy monitoring! 📊🎮

**Questions?** Open an issue: https://github.com/strabo231/sysmon/issues
