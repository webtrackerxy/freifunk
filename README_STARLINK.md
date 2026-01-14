# Starlink (Uplink/Gateway) + Freifunk (Distribution Mesh)

This document explains how to deploy a **Starlink-backed Freifunk Mesh Network** for wide coverage and scalable community connectivity.

---

## 0) What you’re building (in 1 sentence)

**Starlink provides internet**, and **Freifunk routers distribute it via mesh**, so users can connect anywhere in the area.

---

## 1) Hardware checklist (recommended)

### A) Minimum working setup (small area)
✅ 1× **Starlink Kit**  
✅ 1× **Gateway Freifunk Router** (WAN connected to Starlink)  
✅ 2× **Freifunk Mesh Routers** (to extend coverage)

This is enough to test a real mesh.

---

### B) Recommended routers (easy + stable)

#### Indoor nodes (cheap & easy)
- GL.iNet **GL-AR150 / AR300M / MT3000**
- TP-Link **Archer C7** (must match correct hardware version)
- Any OpenWrt-friendly router with decent RAM

#### Outdoor nodes (for long distance / streets)
- TP-Link **CPE210 / CPE510**
- Ubiquiti **NanoStation / LiteBeam** (not always Freifunk-ready but great links)
- Outdoor OpenWrt routers with directional antennas

---

## 2) Best network topology (practical)

### ⭐ Recommended layout
- **1 Gateway Node** = connected to Starlink
- **Multiple Mesh Nodes** = placed around the area
- If possible: outdoor nodes create a “backbone”

```
Starlink Dish
   |
Starlink Router (or bypass)
   |
Gateway Freifunk Router  <-- Internet enters mesh here
   |
   +---- Mesh Node A ---- Mesh Node B ---- Mesh Node C
          |                  |               |
       Users connect      Users connect    Users connect
```

---

## 3) Step-by-step setup (real-world deployment)

### Step 1 — Install Starlink and confirm internet
1. Install the Starlink dish and make sure it works
2. Confirm:
   - You can browse websites
   - Speed is stable

✅ If Starlink is unstable, the mesh won’t fix it (mesh only distributes).

---

### Step 2 — Prepare your Freifunk firmware
You have 2 ways:

#### Option A (easiest): use a community firmware build
Example: Saarland firmware you used before  
- **factory** image (first-time flash)
- **sysupgrade** image (upgrade existing)

⚠️ Must match router model + hardware version exactly.

#### Option B (advanced): build your own Gluon firmware
Best if you want your own “Hong Kong community” config.

---

### Step 3 — Configure the Gateway Freifunk Router (most important node)
1. Connect PC → Gateway Router LAN
2. Flash Freifunk firmware
3. Reboot
4. Connect the WAN port of the gateway router to Starlink LAN

Now this router becomes:
✅ **mesh node + internet gateway uplink**

---

### Step 4 — Add Mesh Nodes (distribution routers)
For each additional Freifunk router:
1. Flash the same Freifunk firmware
2. Power on
3. Place it in the area (different rooms / floors / street corners)

They will automatically:
- join the mesh
- share the same SSID
- route traffic back to the gateway

---

## 4) Placement strategy (this decides coverage)

### Indoor coverage rules
✅ Place nodes:
- near stairs / hallways
- near windows (better reach)
- high position (shelf / wall)

❌ Avoid:
- behind thick concrete walls
- inside metal cabinets
- too close to microwave / heavy interference

---

### Outdoor coverage rules (big range)
For outdoor nodes, aim for:
- **line-of-sight**
- higher mounting points
- directional antennas for long links

Outdoor nodes are how you go from:
🏠 “one building” → 🏙️ “whole street / block”

---

## 5) User experience (what people see)
Users just:
1. Open Wi-Fi
2. Select SSID (example: `freifunk` or `saar.freifunk.net`)
3. No password
4. Internet works automatically (via Starlink gateway)

✅ No app required.

---

## 6) Performance tuning (highly recommended)

### A) Add traffic shaping (SQM)
Starlink can get congested fast if many people stream video.

If your gateway router is OpenWrt-based:
- Enable **SQM (cake)** on WAN
- Set download/upload slightly below Starlink real speed

This makes browsing + messaging stable even under load.

---

### B) Split backhaul and client Wi-Fi (best practice)
If routers support dual band:
- use one band mainly for mesh backhaul
- one band mainly for users

This improves speed and stability.

---

## 7) Security notes (important)
Freifunk SSID is usually **open Wi-Fi**, so recommend users:
✅ use HTTPS websites  
✅ use end-to-end encrypted apps (Signal/WhatsApp)  
✅ optionally use VPN/Tor  

For the gateway:
- use VPN tunneling if your Freifunk community supports it
- keep router firmware updated

---

## 8) Expansion plan (how to scale)

### Phase 1: 3 nodes test
- 1 gateway + 2 nodes
- test roaming + speed

### Phase 2: 5–10 nodes (building / block)
- add outdoor nodes at key points
- create a “mesh backbone”

### Phase 3: multi-gateway redundancy
- add second Starlink gateway at another location
- mesh auto-routes if one gateway fails

---

## 9) Quick test checklist (to confirm it works)

### On phone
- connect to Freifunk SSID
- disable mobile data
- confirm you get an IP (not `169.254.x.x`)
- open YouTube / WhatsApp / Gmail

### Move around
- walk closer to Node B
- confirm Wi-Fi stays connected (roaming may switch nodes)

### Unplug Starlink
- confirm local mesh still exists (local routing stays alive)
- internet stops (expected)

