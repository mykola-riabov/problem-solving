# How to Run Windows Games on Linux using Bottles

This guide explains how to install and run **Windows games** on Linux using [Bottles](https://usebottles.com/), a user-friendly application that simplifies running `.exe` programs through Wine.

---

## ✅ Prerequisites

- [Flatpak](https://flatpak.org/setup/) must be installed and configured
- Bottles must be installed from Flathub
- You need a Windows game installer (`.exe` or `.msi`) or a platform account (e.g. GOG, Epic, Steam)

---

## 🧩 Step-by-step: Install a Windows Game via Bottles

### 1. Install Bottles (if not yet installed)

```bash
flatpak install flathub com.usebottles.bottles
```

> ⚠️ If the app doesn't appear in your menu after install, reboot your system or relogin.

---

### 2. Launch Bottles

```bash
flatpak run com.usebottles.bottles
```

Or open it from your application menu.

---

### 3. Create a new Bottle

- Click **“Create a new Bottle”**
- Choose a name (e.g. `MyGame`)
- Select **Gaming** as the environment type
- Wait until the bottle is ready

---

### 4. Install your game

- Open your bottle
- Click **Run Executable**
- Select the game installer (`.exe` or `.msi`)
- Complete the installation as you would on Windows

---

### 5. Run the game

- After installation, go to the **Programs** tab
- You should see the game shortcut
- Click it to launch

---

## 🎮 Alternative: Use Steam + Proton (for Steam games)

If your game is in your Steam library:

1. Open Steam
2. Go to **Settings > Compatibility**
3. Enable **“Force the use of a specific Steam Play compatibility tool”**
4. Select a version of **Proton**
5. Install and run your Windows game normally

Proton is Valve’s compatibility layer based on Wine, optimized for gaming.

---

## 🛠️ Tips and Troubleshooting

- Use the **Logs** tab in Bottles if a game fails to launch
- If performance is poor, try changing the **runner version** under Bottle Preferences
- Some games may require additional libraries (like `dotnet`, `vcrun`) — install via the **Installers** tab in Bottles
- Use **Gaming** environment type for best compatibility

---

Enjoy playing Windows games on Linux with Bottles! 🍷
