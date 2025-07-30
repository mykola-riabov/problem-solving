# Fixing NVIDIA Driver After Linux Kernel Upgrade

## ❓ Problem

After upgrading the Linux kernel, the proprietary NVIDIA driver may stop working.

### Symptoms:

- Graphical environment does not start (black screen or console login only)
- `startx` or `startxfce4` fails to launch
- `nvidia-smi` outputs:

  ```
  NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver
  ```

- `lsmod | grep nvidia` shows no NVIDIA kernel module loaded
- `/var/log/Xorg.0.log` shows fallback to `vesa`, `nouveau`, or `llvmpipe`
- Poor performance, software rendering (`llvmpipe`)

---

## ✅ Solution: Step-by-Step

### 1. Check your running kernel

```bash
uname -r
```

---

### 2. Install kernel headers for your current kernel

- **Debian/Ubuntu**:
  ```bash
  sudo apt install linux-headers-$(uname -r)
  ```

- **Arch/Manjaro**:
  ```bash
  sudo pacman -S linux-headers
  ```

- **Fedora**:
  ```bash
  sudo dnf install kernel-devel
  ```

---

### 3. Install `dkms` (Dynamic Kernel Module Support)

- **Debian/Ubuntu**:
  ```bash
  sudo apt install dkms
  ```

- **Arch**:
  ```bash
  sudo pacman -S dkms
  ```

- **Fedora**:
  ```bash
  sudo dnf install dkms
  ```

---

### 4. Rebuild and load NVIDIA module

```bash
sudo dkms autoinstall
sudo depmod -a
sudo modprobe nvidia
```

Verify:
```bash
lsmod | grep nvidia
nvidia-smi
```

---

### 5. Check OpenGL renderer

```bash
glxinfo | grep "OpenGL renderer"
```

- ✅ If `NVIDIA GeForce ...` — everything is fine
- ❌ If `llvmpipe` — software rendering is in use

---

### 6. Disable `nouveau` if it interferes

```bash
echo -e "blacklist nouveau\noptions nouveau modeset=0" | sudo tee /etc/modprobe.d/blacklist-nouveau.conf
```

Then update initramfs:

- **Debian/Ubuntu**:
  ```bash
  sudo update-initramfs -u
  ```

- **Arch**:
  ```bash
  sudo mkinitcpio -P
  ```

- **Fedora**:
  ```bash
  sudo dracut --force
  ```

Reboot:

```bash
sudo reboot
```

---

### 7. (Optional) Remove old kernels

- **Debian/Ubuntu**:
  ```bash
  dpkg --list | grep linux-image
  sudo apt remove linux-image-<oldversion>
  sudo apt autoremove
  sudo update-grub
  ```

- **Arch**:
  ```bash
  sudo pacman -R linux-lts
  ```

- **Fedora**:
  ```bash
  sudo dnf remove kernel-<oldversion>
  ```

---

## ✅ Final Checks

| Command | Purpose |
|---------|---------|
| `uname -r` | Check running kernel |
| `lsmod \| grep nvidia` | Check if NVIDIA module is loaded |
| `nvidia-smi` | Check if GPU is available |
| `glxinfo \| grep renderer` | Verify OpenGL rendering method |
| `cat /var/log/Xorg.0.log \| grep EE` | Check X server errors |

---

## 🛡️ Prevention

- Install generic headers package:
  ```bash
  sudo apt install linux-headers-generic
  ```

- Keep `dkms` installed
- Always install kernel headers immediately after kernel upgrade

---
