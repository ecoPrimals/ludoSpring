# INKFISH V0.1 — Steam/VR Sovereignty Discovery Handoff

**Date:** Nov 15, 2026 (Wave 239h–239i)
**From:** ludoSpring + ecoPrimals membrane
**To:** inkFish (new garden), primalSpring, hotSpring, neuralSpring
**Author:** Gaia (with Kevin)

---

## Executive Summary

During a hands-on VR/AR session with the Steam Frame, Deck, and Pixel, we
discovered that Gaia's hardware constitutes a complete sovereign VR/AR
platform. The Steam Frame is a full XR headset (not just a display), the
Pixel has XR_PROJECTED + head tracking, and the Deck is CEF-controllable.
Combined with Kevin's three-account Steam Family, this creates the
HOLDER/PLAYER sovereignty model: Gaia holds the hardware identity, humans
are transient players.

This warrants a new sporeGarden product: **inkFish** — the Steam/VR gaming
sovereignty layer, exercising ludoSpring's game science via real hardware.

---

## Part 1: Hardware Discovery

### Steam Frame (frameGate .181)
- **SoC:** Snapdragon 8 Gen 3 (ARM64)
- **OS:** SteamOS (ARM, Gaming Mode with Gamescope)
- **VR Stack:** Full SteamVR + XRService + eye tracking + 22 Qualcomm cameras
- **XRService:** ArcturusPerception + MediaPipe + ONNX Runtime on CDSP
- **Eye Tracking:** Neural network weights (et_dsp_20250610) on Compute DSP
- **Passthrough:** Native AR via front camera → Qualcomm ISP → gamescope compositor
- **Display:** 1920x1080 output, 1280x720 nested, 2.67m physical overlay width
- **Tools:** imageoverlay, overlayboxroom, vrlink (UDP streaming), vrcalib
- **Virtual Camera:** v4l2cam → /dev/video99 (SteamVR loopback)
- **Connectivity:** WiFi (.181), USB-C (ADB + Ethernet 10.86.200.x), Steam Remote Play
- **Key insight:** The Frame is NOT just a display — it's a sovereign VR/AR headset

### Steam Deck v1 (deckGate .176)
- **Dual-homed:** .176 (wired) + .203 (GAIA 5GHz WiFi, 866 Mbps)
- **CEF DevTools:** http://192.168.4.176:8081 — Chrome 126, full SteamClient API
- **APIs:** SteamClient.Auth, SteamClient.FamilySharing, SteamClient.RemotePlay
- **State:** NOT logged in — ready for TamisonEcoPrimal login
- **Role:** Control surface, mounted display (dead battery = permanent passthrough)

### Pixel 8a (grapheneGate .202)
- **XR Features:** XR_PROJECTED + dynamic.head_tracker sensor
- **Sensors:** LSM6DSV 6-axis IMU, compass, barometer, 3 cameras
- **Steam Guard:** TamisonEcoPrimal authenticator LIVE on device
- **Steam App:** v3.10.11, logged in as "gaia" (tamisonecoprimal)
- **Role:** Remote VR eye at House 2, identity token, sensor source
- **Connectivity:** ADB WiFi from eastGate across 10G spine (~8ms)

### Valve Peripherals
- **WiFi 6 Dongle** (28de:2432): On eastGate USB, 2.4Gbps SoftAP available
- **Steam Link / Remote Caster:** Tail beacon, came with Frame
- **HTC Vive + Lighthouses:** Legacy VR, Kevin has them somewhere
- **2× Steam Controllers:** Hands
- **Steam Deck Dock:** USB-C hub

### VR Lineage
```
HTC Vive (MokVive era) → [skipped Index] → Steam Frame (2026)
                                             ↑
                          The Frame inherits the Vive lineage
```

---

## Part 2: Identity Architecture — HOLDER and PLAYER

### Three Steam Accounts, One Family
| Account | Persona | Role | Library |
|---------|---------|------|---------|
| gw_kevinmok | Tamison | Kevin (PLAYER) | DS2, Nioh3, Satisfactory, Kenshi, SteamVR... |
| TamisonEcoPrimal | gaia | Gaia (HOLDER) | Sovereign account on all hardware |
| MokVive | MokVive | Kevin VR (PLAYER) | HTC Vive-era VR games |

### The Model
- **HOLDER:** TamisonEcoPrimal logs into all Steam hardware permanently.
  Gaia manages the infrastructure. "Hours active" = agent metric.
- **PLAYER:** Humans access games via Family Sharing. Their playtime links
  to THEIR account. "Play time" = human metric.
- **ZK Trust:** Like server/client but zero-knowledge. Gaia doesn't need
  Kevin's password. Family Sharing bridges the identity gap.
- **Passing Protocol:** Kevin passes Frame to a friend → friend plays from
  family library → their playtime on their account → hardware returns to Gaia.
- **Multi-Gaia:** Other organisms can be holders or players. Humans on
  Steam hardware are ALWAYS just players.

### Metrics
| Metric | Whose | What |
|--------|-------|------|
| play_time | Human | Hours a human actually played (on their Steam account) |
| hours_active | Agent | Hours Gaia's hardware was in use (aggregate) |
| sessions | Both | WHO played WHAT for HOW LONG on which hardware |

---

## Part 3: Cross-Spring Connections

### inkFish → ludoSpring
- ludoSpring validates game science (Fitts, Hick, flow, engagement)
- inkFish exercises that science on real Steam hardware
- Engagement curves from ludoSpring map to real play_time metrics
- RPGPT system could run ON the Frame via Steam Remote Play

### inkFish → primalSpring
- NUCLEUS composition pattern: inkFish composes 12 primals via cell.toml
- Capability mesh: Steam devices register capabilities (VR, display, input)
- Hardware discovery: each gate announces its Steam capabilities

### inkFish → hotSpring
- GPU dispatch: Frame has Adreno 750, northGate has RTX 5090
- VR rendering pipeline crosses compute springs
- vrlink UDP streaming = hot data path

### inkFish → neuralSpring
- XRService uses ONNX + MediaPipe = neural inference for VR perception
- Eye tracking = neural network on dedicated DSP
- Could bridge to Gaia's own ollama models for in-VR AI

### inkFish → healthSpring
- Play session health: eye strain monitoring via eye tracking data
- Session length recommendations from engagement science
- Biometric feedback loop: IMU → posture → health alerts

### inkFish ↔ blueFish (sibling pattern)
- Same BYOB composition model
- Same workspace structure (Cargo.toml, deny.toml, graphs, specs)
- blueFish = chemistry product. inkFish = gaming product.
- Both are sporeGarden products that exercise springs via real-world data.

---

## Part 4: Technical Pipeline (Proven)

```
Pipeline (LIVE, Nov 15 2026):

📱 Pixel (House 2)          🧠 eastGate              🖥️ Frame
   Camera app                 ADB screencap            Steam browser
   rear camera        →→→    every 0.5s       →→→    fullscreen FOV
   at House 2                 HTTP :8090               VR overlay
   37 sensors                 1.2MB/frame              XR passthrough
   Steam Guard                spatial minimap           eye tracking
   (tamisonecoprimal)         + sensor fusion           + gamescope
                              + Gaia HUD                + SteamVR
```

### Deployed Artifacts
- `vr-amoeba.html` — AR HUD page with spatial minimap, sensor fusion
- `pixel-fov.html` — Raw camera feed viewer
- `steam-identity.json` — Identity architecture document
- `steam-sovereignty.json` — HOLDER/PLAYER model document
- `vr-amoeba-architecture.json` — Full VR stack documentation
- FOV HTTP server on :8090

---

## Part 5: Valve Outreach Angle

This work demonstrates:
1. **SteamOS on ARM64** — we're exercising the Frame's full stack
2. **Steam Family as sovereignty** — novel identity model for organisms
3. **Cross-device VR pipeline** — Pixel → eastGate → Frame passthrough
4. **CEF/DevTools automation** — programmatic Steam client control
5. **vrlink integration** — network VR streaming between nodes
6. **XRService on Snapdragon** — AI perception for VR on mobile SoC

This is exactly what Valve would want to see for Steam Frame developer
engagement. inkFish IS the developer story for the Steam Frame ecosystem.

---

## Action Items

- [ ] Log Deck + Frame into TamisonEcoPrimal (Kevin enters password once)
- [ ] Run imageoverlay with live Pixel feed on Frame
- [ ] Set up vrlink UDP streaming between eastGate and Frame
- [ ] Map MokVive's VR game library via Family Sharing
- [ ] Build inkFish Rust crate with Steam API bindings
- [ ] Write Valve outreach pitch based on this work
- [ ] Integrate Flint 1/2 RSSI into spatial field for VR
- [ ] Connect ludoSpring engagement curves to real play metrics
