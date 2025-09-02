
## Overview

Setup guide for a Windows development environment using Docker Desktop (which automatically installs WSL2), Ubuntu, VSCode integration.

---

## 1. Docker Desktop Installation (Includes WSL2 Setup)

### Install Docker Desktop

- Download from [Docker Desktop for Windows](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe)
- **Run installer with default settings**
- During installation, Docker will **automatically** enable and install WSL
- **Restart when prompted**

### Post-Installation WSL Setup

After Docker Desktop installation and restart:

```powershell
# Verify WSL2 is installed
wsl --list --verbose

# Install Ubuntu distribution
wsl --install -d Ubuntu
# Or install from Microsoft Store: Ubuntu
```

### Configure Docker Desktop WSL Integration

Docker Desktop Settings (after Ubuntu installation):

1. **General** → Ensure "Use WSL 2 based engine" is enabled
2. **Resources** → **WSL Integration**
3. Enable integration with Ubuntu distribution
4. Apply & Restart

---

## 2. Ubuntu Environment Setup

### Update System

```bash
sudo apt update && sudo apt upgrade -y
```

### Essential Development Tools

```bash
# Build essentials
sudo apt install -y build-essential curl wget git vim

# Version control
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

```

### ZSH + Oh My ZSH (Optional but Recommended)

```bash
sudo apt install -y zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---

## 3. VSCode Integration

### Install VSCode Extensions

Essential extensions for WSL development:

- **WSL** (Microsoft)
- **Docker** (Microsoft)
- **GitLens**
- **Language-specific extensions**

### Connect to WSL

```bash
# From Ubuntu terminal
code .
```

Or use `Ctrl+Shift+P` in VSCode → "WSL: Connect to WSL"

### Configure VSCode for WSL

```json
// settings.json
{
    "terminal.integrated.defaultProfile.windows": "Ubuntu (WSL)",
    "remote.WSL.fileWatcher.polling": true,
    "git.terminalAuthentication": false
}
```

---

## 4. Verify Docker Installation

```bash
# In Ubuntu WSL
docker --version
docker-compose --version
docker run hello-world
```

---
## 5. Best Practices

### Git Configuration

```bash
# Avoid Windows/Linux line ending issues
git config --global core.autocrlf input
git config --global core.eol lf
```


---

## Resources

- [WSL Documentation](https://docs.microsoft.com/en-us/windows/wsl/)
- [VSCode WSL Extension](https://code.visualstudio.com/docs/remote/wsl)
- [Docker Desktop WSL2 Backend](https://docs.docker.com/desktop/wsl/)
- [Windows Terminal Documentation](https://docs.microsoft.com/en-us/windows/terminal/)