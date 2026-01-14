# Outdoor Point-to-Point (PtP) Backbone Links (5km+)

This guide explains how to deploy **outdoor PtP wireless links for 5km+ distances** to improve **backbone stability** for a **Starlink (uplink/gateway) + Freifunk (distribution mesh)** network.

For long distances, a PtP link acts like a **wireless ethernet cable** between two sites (e.g. rooftops), allowing Freifunk nodes to expand coverage reliably without relying on multi-hop omni mesh.

---

## 1) Why PtP backbone is important for 5km+

Freifunk mesh using normal indoor routers is great for short distances, but over large areas it becomes:
- slower (each hop reduces throughput)
- less stable (walls, reflections, interference)
- unpredictable (node placement changes)

A **PtP backbone** gives you:
✅ stable long-range connectivity  
✅ predictable throughput  
✅ fewer hops for internet access  
✅ easier scaling to villages / blocks / campuses  

---

## 2) Requirements for 5km+ links

For **5km+**, you must have:

### A) True line-of-sight (LoS)
- No buildings/trees blocking the direct path
- Best from rooftop-to-rooftop or hill-to-hill

### B) Fresnel zone clearance (very important)
Even if you can “see” the other side, the signal still needs clearance around the path.
- Avoid the link passing close to rooftops or tree tops
- Higher mounting improves Fresnel clearance significantly

### C) Stable mounting + weatherproofing
- Strong pole mount
- Weatherproof connectors
- Drip loops on cables
- Proper grounding / lightning protection (recommended)

---

## 3) Recommended hardware for 5km+ PtP

### Best practical choices (proven)
- **Ubiquiti LiteBeam / NanoBeam (5GHz AC/AX)**
- **Ubiquiti PowerBeam (higher gain dish)**
- **MikroTik LHG series** (high gain, good value)
- **TP-Link Pharos CPE710** (good budget option)

### Selection guidance
| Distance | Recommended Antenna Type |
|----------|---------------------------|
| 5–8 km   | 5GHz directional dish / high gain panel |
| 8–15 km  | higher gain dish (PowerBeam / LHG high gain) |
| 15km+    | specialist dish + careful RF planning |

> For 5km+, prefer **high-gain directional antennas** (dish style), not omni routers.

---

## 4) Topology (recommended)

### ⭐ Best practice: PtP is ONLY the backbone, Freifunk serves clients locally

```
[Site A - Starlink + Gateway]
   Starlink Dish
        |
   Starlink Router / Bypass
        |
   Gateway Router (Freifunk gateway)
        |
   Switch / LAN
        |
   PtP Radio (AP/Master)  <==== 5km+ PtP Link ====>
                                PtP Radio (Station/Client)
                                         |
                                      Switch / LAN
                                         |
                                Freifunk Node / Local AP
                                (phones/laptops connect here)
```

Why this is best:
- PtP radios focus on stable backhaul only
- Freifunk handles local Wi-Fi distribution
- Users do not overload the PtP radio

---

## 5) Step-by-step deployment (5km+)

### Step 1 — Plan the two sites
Pick two locations with:
✅ rooftop / tower / high balcony  
✅ clear line-of-sight  
✅ safe mounting access  
✅ stable power supply  

Recommended: use a map + on-site visual check.

---

### Step 2 — Install and mount radios
1. Mount both radios firmly on poles/walls
2. Keep the radios as high as possible
3. Use outdoor-rated ethernet cable if possible
4. Use PoE injectors (most radios require PoE)

**Safety**
- Avoid installing during storms
- Secure ladders and mounting points

---

### Step 3 — Align antennas (critical for 5km+)
Alignment quality is the #1 reason long links fail.

1. Power both radios
2. Open the vendor alignment tool (web UI)
3. Slowly adjust left/right/up/down
4. Lock in the best:
   - signal strength (RSSI)
   - SNR
   - link quality (CCQ)

#### Target signal (rough guideline)
- **-40 to -60 dBm** = excellent
- **-60 to -70 dBm** = good/usable
- **worse than -75 dBm** = unstable

---

### Step 4 — Configure radio roles (AP + Station)
Typical setup:

#### Radio A (Site A): AP / Master
- Mode: **Access Point**
- Network: **Bridge**
- Security: WPA2 AES (or vendor proprietary encryption)
- Channel: fixed (not auto)
- Channel width: **20 MHz** (recommended for stability)
- TX power: moderate (not always maximum)

#### Radio B (Site B): Station / Client
- Mode: **Station**
- Network: **Bridge**
- Connect to Radio A SSID
- Same channel/security settings

---

### Step 5 — Choose frequency/channel wisely
For 5km+ stability, avoid “auto” settings.

Recommended:
- Use **5GHz** for speed and clean spectrum
- Use a **fixed channel**
- Prefer **20 MHz** channel width for long links

⚠️ DFS channels can be forced to change if radar is detected.
If you experience random drops, try a non-DFS channel if allowed in your region.

---

### Step 6 — Connect PtP into your Freifunk + Starlink network

#### At Site A (Starlink / Gateway site)
- Starlink provides internet
- Gateway router connects to PtP radio via LAN switch
- PtP carries upstream network to Site B

#### At Site B (Remote site)
- PtP radio connects to a small switch
- Connect:
  - Freifunk node (local distribution)
  - optional extra APs for more coverage

---

## 6) Best integration model with Freifunk

### ⭐ Model 1 (recommended): PtP = LAN extension
Treat the PtP link as a **transparent Layer-2 bridge**.
- Easy to debug
- Fast
- Stable

This works well with:
- Starlink uplink at Site A
- Freifunk distribution at Site B

---

## 7) Stability tuning (high impact)

### A) Use 20 MHz channel width
- most stable
- best interference resistance
- best long-range reliability

### B) Avoid max TX power
More power is not always better.
Too much TX power can cause:
- distortion
- worse performance
- more interference

### C) Enable watchdog / auto recovery
Enable:
- ping watchdog reboot
- scheduled reboot (optional)

Useful if radios get stuck after long uptime.

### D) Protect power and cabling
- use good PoE injectors
- avoid cheap cables
- weatherproof all connectors
- add surge protection if possible

---

## 8) Troubleshooting checklist

### Problem: Link drops randomly
- check PoE power stability
- check water ingress in connectors
- reduce channel width to 20 MHz
- switch channel (interference/DFS issues)
- re-align antennas (even 1–2 degrees matters)

### Problem: Good signal but low speed
- interference (scan spectrum)
- Fresnel zone blocked
- wrong channel width (too wide)
- misalignment

### Problem: Works in day but fails at night/rain
- weak link margin (upgrade antenna gain)
- rain fade / environmental changes
- Fresnel zone partially blocked

---

## 9) Expansion (multi-site scaling)

Once you have one stable 5km+ backbone:
- Add more remote sites using PtMP (hub-and-spoke)
- Or build a chain of PtP links between multiple rooftops

Recommended:
- keep PtP as the backbone
- keep Freifunk nodes local to each site

---

## 10) Quick deployment checklist (copy/paste)

- [ ] Confirm line-of-sight between Site A and Site B
- [ ] Mount radios high and stable
- [ ] Weatherproof cables + connectors
- [ ] Configure AP + Station in bridge mode
- [ ] Use fixed channel + 20 MHz width
- [ ] Align antennas for best RSSI/SNR
- [ ] Connect Site A PtP to Starlink gateway LAN
- [ ] Connect Site B PtP to Freifunk node + local AP
- [ ] Test speed + stability for 24 hours
