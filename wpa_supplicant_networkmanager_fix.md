
# Fixing Conflict Between `wpa_supplicant` and `NetworkManager` on Debian

## Problem

After installing and configuring Debian, Wi-Fi connection fails on boot.
You notice the following error messages in `journalctl` or `dmesg`:

```
device (wlp3s0): Couldn't initialize supplicant interface
Failed to initialize control interface '/run/wpa_supplicant/wlp3s0'
Match already configured
```

Despite disabling and masking `wpa_supplicant.service`, the conflict persists and NetworkManager cannot control the Wi-Fi interface.

## Root Cause

There are multiple conflicting network configurations:

- `/etc/network/interfaces` manually defines `wpa-ssid` and `wpa-psk`
- `NetworkManager` tries to manage the same interface (`wlp3s0`)
- `wpa_supplicant` runs independently, causing resource conflict

## Solution

To resolve the issue and let `NetworkManager` manage the Wi-Fi interface:

### 1. Clean up `/etc/network/interfaces`

Edit the file:

```bash
sudo nano /etc/network/interfaces
```

Remove or comment out any configuration for `wlp3s0`. The file should look like this:

```ini
source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback
```

### 2. Mask `wpa_supplicant` service

```bash
sudo systemctl mask wpa_supplicant.service
```

(Optional: Also check for other conflicting services)

```bash
sudo systemctl list-unit-files | grep wpa
```

### 3. Restart NetworkManager

```bash
sudo systemctl restart NetworkManager
```

### 4. Reboot

```bash
sudo reboot
```

After rebooting, Wi-Fi should work correctly and NetworkManager should manage the interface without interference.

## Verification

Check Wi-Fi connection:

```bash
nmcli device
```

You should see `wlp3s0` as `connected`.

Check service status:

```bash
sudo systemctl status wpa_supplicant
sudo systemctl status NetworkManager
```

`wpa_supplicant` should be **masked** and **inactive**, while `NetworkManager` is **active (running)**.

---

Tested on: **Debian 12**
