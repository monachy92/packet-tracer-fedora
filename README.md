# 🔒 Hardened Cisco Packet Tracer Setup

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)
![Distrobox](https://img.shields.io/badge/Distrobox-Compatible-green)

A secure, containerized installation script for Cisco Packet Tracer on Linux using Distrobox.

## 🎯 Why This Exists

Cisco Packet Tracer is traditionally installed directly on your system, which:
- Requires extensive system dependencies
- Can clutter your host OS
- Poses potential security risks from proprietary software

This script solves these issues by running Packet Tracer in an **isolated Ubuntu 22.04 container** with hardened security settings.

## ✨ Features

- 🛡️ **Containerized isolation** - Packet Tracer runs in its own environment
- 🔐 **Restricted X11 access** - Only your user can access the display
- 🧹 **Automatic cleanup** - X11 permissions close when app exits
- 📦 **No host pollution** - All dependencies stay in the container
- 🚀 **Easy launch** - Desktop menu integration included
- ✅ **Error handling** - Validates files and handles edge cases

## 📋 Prerequisites

- **Linux distribution** with X11 or Wayland
- **Distrobox** installed ([installation guide](https://github.com/89luca89/distrobox))
- **Podman** or **Docker** (Podman recommended)
- **Packet Tracer .deb file** from [Cisco NetAcad](https://www.netacad.com/courses/packet-tracer)

### Install Distrobox

**Fedora:**
```bash
sudo dnf install distrobox podman
```

## 🚀 Quick Start

### 1. Download the Script

```bash
git clone https://github.com/yourusername/packet-tracer-secure-setup.git
cd packet-tracer-secure-setup
chmod +x setup.sh
```

### 2. Get Packet Tracer

1. Go to [Cisco NetAcad](https://www.netacad.com/courses/packet-tracer)
2. Sign in (or create a free account)
3. Download the **Linux 64-bit .deb** file
4. Note the download location (usually `~/Downloads/`)

### 3. Run the Installer

```bash
./setup.sh
```

The script will:
1. Create an isolated container named `PTBox`
2. Install all necessary dependencies
3. Set up a secure launcher script
4. Create a desktop menu entry
5. Prompt you for the .deb file location
6. Install and extract Packet Tracer

### 4. Launch Packet Tracer

**Option A:** From your application menu
- Look for "Cisco Packet Tracer (Secure)"

**Option B:** From terminal
```bash
~/launch_pt.sh
```

## 🔧 What Gets Installed

### On Your Host System
- `~/.pt_container_home/` - Container home directory
- `~/launch_pt.sh` - Launcher script
- `~/.local/share/applications/packettracer.desktop` - Menu entry

### Inside the Container
- Ubuntu 22.04 base
- Required graphics and X11 libraries
- Packet Tracer application files in `~/.pt_app/`

## 🛡️ Security Features

### Container Isolation
- Separate home directory prevents access to your real home
- Container has limited system access
- Network namespace isolation (depending on Distrobox config)

### X11 Access Control
```bash
xhost +SI:localuser:$USER  # Only your user can access display
```
This is more restrictive than the default `xhost +local:docker` which would allow any container.

### Automatic Cleanup
The launcher automatically revokes X11 access when Packet Tracer closes, minimizing exposure time.

### Input Validation
- Verifies .deb file exists
- Checks file extension
- Validates it's actually a Debian package
- Handles errors gracefully

## 🔍 Troubleshooting

### "distrobox: command not found"
Install Distrobox first:
```bash
sudo apt install distrobox podman
```

### "Cannot connect to display"
Make sure X11 is running. On Wayland, ensure XWayland is installed:
```bash
sudo apt install xwayland
```

### "Permission denied" when accessing .deb file
Make sure the file path is correct and readable:
```bash
ls -l ~/Downloads/CiscoPacketTracer*.deb
```

### Container already exists
The script will detect this and ask if you want to recreate it.

### Graphics Issues
Some systems may need additional configuration. Try:
```bash
distrobox-enter -n PTBox -- glxinfo | grep "OpenGL"
```

## 🗑️ Uninstallation

Remove everything cleanly:

```bash
# Remove the container
distrobox rm -f PTBox

# Remove installed files
rm -rf ~/.pt_container_home
rm ~/launch_pt.sh
rm ~/.local/share/applications/packettracer.desktop

# Update desktop database
update-desktop-database ~/.local/share/applications
```

## 📚 How It Works

1. **Container Creation**: Creates an Ubuntu 22.04 container with isolated home
2. **Dependency Installation**: Installs X11, graphics, and Qt libraries
3. **Launcher Script**: Creates a wrapper that manages X11 permissions
4. **Desktop Integration**: Adds menu entry for easy access
5. **AppImage Extraction**: Extracts Packet Tracer from the .deb installer

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Test thoroughly on your system
4. Submit a pull request

## ⚠️ Disclaimer

This is an unofficial installation method. Cisco Packet Tracer is proprietary software owned by Cisco Systems. You must have a valid NetAcad account and agree to Cisco's terms of service.

This script does not modify or crack Packet Tracer in any way - it simply provides a secure containerized environment for the official software.

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

The script is open source, but Cisco Packet Tracer itself is proprietary software subject to Cisco's licensing terms.

## 🙏 Acknowledgments

- [Distrobox](https://github.com/89luca89/distrobox) - Container management
- Cisco NetAcad - For providing Packet Tracer
- The Linux community - For containerization tools

---

**Questions?** Open an issue on GitHub or check the [Distrobox documentation](https://distrobox.privatedns.org/).

**Enjoying this?** Give it a ⭐ on GitHub!
