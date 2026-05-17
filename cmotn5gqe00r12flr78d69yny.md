---
title: "Automating Cloudflare WARP Based on WiFi SSID (Linux Guide)"
datePublished: 2026-05-06T05:53:11.418Z
cuid: cmotn5gqe00r12flr78d69yny
slug: automating-cloudflare-warp-based-on-wifi-ssid-linux-guide
cover: https://cdn.hashnode.com/uploads/covers/5f95116240346172a86c20c6/6bb7d31a-9f88-478b-84cb-9d94f8f51d5e.png
ogImage: https://cdn.hashnode.com/uploads/og-images/5f95116240346172a86c20c6/c5c8bf08-5c9c-4fd2-a249-d7994e4b00f7.png
tags: linux, automation, networking, vpn

---

If you’re switching between networks with different trust levels—home, café, coworking space—you probably don’t want your VPN behavior to be static.

This guide walks through a clean, system-level way to **automatically connect or disconnect Cloudflare WARP depending on your WiFi network name (SSID)** using NetworkManager on Linux.

* * *

## 🧠 Why This Matters

Not all networks are equal:

*   🏠 **Trusted WiFi (Home)** → You may not need WARP
    
*   ☕ **Public WiFi** → You *definitely* want WARP
    
*   🏢 **Office networks** → Might conflict with VPN routing
    

Instead of manually toggling WARP every time, we can hook into **network state changes** and automate it.

* * *

## ⚙️ How It Works

Linux systems using NetworkManager support **dispatcher scripts**—these are triggered automatically when network events occur (e.g., connecting to WiFi).

We leverage this to:

1.  Detect the current SSID
    
2.  Apply conditional logic
    
3.  Toggle WARP accordingly via CLI
    

* * *

## 🔧 Step-by-Step Implementation

### 1\. Ensure WARP CLI is Installed

You should already have `warp-cli` available. If not, install Cloudflare WARP first.

Then register and test:

```shell
warp-cli register
warp-cli connect
warp-cli status
```

* * *

### 2\. Create a NetworkManager Dispatcher Script

Dispatcher scripts live here:

```shell
/etc/NetworkManager/dispatcher.d/
```

Create a new script:

```shell
sudo nano /etc/NetworkManager/dispatcher.d/99-warp-toggle
```

* * *

### 3\. Add Logic Based on SSID

Paste the following:

```shell
#!/bin/bash

INTERFACE="$1"
STATUS="$2"

# Trigger only when a connection is established
if [ "$STATUS" = "up" ]; then
    SSID=$(iwgetid -r)

    if [ "$SSID" = "home_wifi" ]; then
        echo "Connecting WARP for $SSID"
        warp-cli connect

    elif [ "$SSID" = "office_wifi" ]; then
        echo "Disconnecting WARP for $SSID"
        warp-cli disconnect

    else
        echo "Unknown network: $SSID — no action taken"
    fi
fi
```

* * *

### 4\. Make the Script Executable

```shell
sudo chmod +x /etc/NetworkManager/dispatcher.d/99-warp-toggle
```

* * *

### 5\. Apply Changes

Restart NetworkManager:

```shell
sudo systemctl restart NetworkManager
```

Or simply reconnect your WiFi.

* * *

## 🧪 Testing

Switch between your networks:

*   Connect to `home_wifi` → WARP should **connect**
    
*   Connect to `office_wifi` → WARP should **disconnect**
    

Verify with:

```shell
warp-cli status
```

* * *

## ⚠️ Things to Watch Out For

*   **SSID detection relies on** `iwgetid` — ensure it’s installed
    
*   Dispatcher scripts run as **root**, so be careful with permissions and logging
    
*   Some networks may **block WARP traffic**, causing connection failures
    
*   Avoid adding too many rapid toggles (though WARP CLI is fairly tolerant)
    

* * *

## 🧩 Optional Enhancements

If you want to level this up:

### 🔹 Add Logging

```shell
echo "$(date): Connected to $SSID" >> /var/log/warp-toggle.log
```

### 🔹 Handle More Networks

Expand your conditions into a case statement:

```shell
case "$SSID" in
  "home_wifi")
    warp-cli connect
    ;;
  "office_wifi")
    warp-cli disconnect
    ;;
esac
```

### 🔹 Default Behavior

Set a fallback (e.g., always connect WARP unless explicitly disabled)

* * *

## 💡 Final Thoughts

This approach is powerful because it’s:

*   **Event-driven** (no polling loops)
    
*   **Lightweight** (no extra services needed)
    
*   **Extensible** (you can hook in more automations)
    

You’re essentially turning your machine into a **context-aware system**—reacting intelligently to its environment.

Once you get comfortable with dispatcher scripts, this pattern opens up a lot of automation possibilities beyond VPNs.

* * *

If you’re building out a more advanced workflow system (especially with tools like n8n or custom daemons), this can serve as a solid foundation for network-aware automation.

🚀