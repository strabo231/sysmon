# Changelog

All notable changes to SysMon will be documented in this file.

## [1.1.0] - 2024-12-21

### Added
- 🎮 **GPU Monitoring** - Comprehensive GPU support
  - NVIDIA GPU monitoring via nvidia-smi
    - Real-time GPU utilization
    - Memory usage tracking
    - Temperature monitoring
    - Power draw and limits
    - Fan speed monitoring
    - Active GPU process tracking
  - AMD GPU monitoring via ROCm/sysfs
    - GPU utilization
    - Temperature monitoring
    - Memory usage (with ROCm)
  - Intel GPU basic support
    - GPU detection
    - Basic information display
    - Integration with intel_gpu_top
- 🔍 Automatic GPU detection
- 📊 GPU info in dashboard view (NVIDIA)
- 📈 Visual progress bars for all GPU metrics
- 🎨 Color-coded GPU usage indicators

### Changed
- Updated dashboard to show GPU status when available
- Improved help documentation
- Enhanced version display

## [1.0.0] - 2024-12-01

### Added
- 📊 Beautiful dashboard view with system overview
- 💻 Process management and listing
- 🔝 Top CPU/memory consumer tracking
- 🌐 Network monitoring and connections
- 💾 Disk I/O statistics and usage
- ⚡ Live monitoring mode (watch)
- 🎨 Color-coded alerts (Green/Yellow/Red)
- 📈 Visual progress bars for resources
- 🔪 Safe process killing with confirmation
- 📝 Comprehensive help system
- 🚀 One-line installer script

### Features
- CPU usage monitoring
- Memory usage tracking
- Disk space monitoring
- System load average display
- Uptime tracking
- Process sorting and filtering
- Network interface statistics
- Active connection tracking
- Real-time dashboard updates

---

## Legend
- ✨ New feature
- 🔧 Improvement
- 🐛 Bug fix
- 📝 Documentation
- 🔒 Security
- ⚠️ Deprecation
- 🗑️ Removal
