# PostureBox — Possible Hardware Solution & Industrial Design (Prototype → Product)

## 1) Design goals
- Desk footprint ~ small alarm clock
- Wide field-of-view to capture torso/shoulders/head while seated
- Quiet, “calm tech” feedback (subtle LED, optional sound)
- Always-on processing with **no cloud video**
- Easy placement guidance + per-user calibration

---

## 2) Prototype hardware options (pick one)

### Option A — NVIDIA Jetson class (fastest path for edge AI dev)
**Best for:** rapid pose inference, GPU acceleration, prototyping.
- Compute: Jetson dev kit / module family
- Camera: USB UVC wide-angle or MIPI CSI module
- Pros: strong on-device inference performance; mature ecosystem
- Cons: higher power/heat; BOM cost higher than MCU/RPi

### Option B — Raspberry Pi + accelerator (cost-sensitive prototype)
**Best for:** quick Linux dev, moderate inference, lower power.
- Compute: Raspberry Pi 5 (or similar)
- Accelerator: Coral USB / M.2 (for TF Lite models) *or* NPU-based module if chosen
- Pros: affordable, huge ecosystem
- Cons: may require careful optimization; accelerator compatibility constraints

### Option C — x86 mini PC (easiest dev, less product-like)
**Best for:** proving experience fast, not final form.
- Compute: NUC-like mini PC
- Pros: easiest dev; runs anything
- Cons: cost, power, not productizable as-is

**Recommendation for you:** start with **Option B** if you want cost/power realism, or **Option A** if you want the smoothest pose inference and fastest “it just works” experience.

---

## 3) Core components (v1 prototype BOM outline)
- **Compute board** (Jetson/RPi/x86)
- **Camera**: 1080p, wide-angle (≈120°) preferred; fixed focus tuned for 0.5–1.5m
- **Feedback**
  - LED ring or light bar (diffused)
  - Optional small speaker/buzzer (very gentle)
  - One button (mute / focus mode toggle)
- **Connectivity**: Wi‑Fi (and optional Ethernet/USB-C)
- **Storage**: eMMC/SD + encrypted partition for local data
- **Power**: USB‑C PD (target stable 5V/3A+ depending on compute)
- **Enclosure**: venting + diffusion window for LED + privacy shutter for camera (strong trust signal)

---

## 4) Physical design (what it should look/feel like)
### Form
- Minimal wedge/rounded rectangle
- Camera at top-front with slight downward angle
- Rubber feet to avoid sliding
- Back: USB‑C power, optional Ethernet

### Human factors
- “Placement card” printed on underside: recommended distance + height
- Camera **privacy shutter** is a major trust feature
- LED semantics:
  - off = good / idle
  - soft pulse = “adjust posture”
  - hard flash avoided; never strobe

### Thermal
- Passive heatsink + vent channels
- Keep skin-touch surfaces below comfortable temps; place vents away from user-facing side

---

## 5) On-device software stack (reference architecture)
- OS: minimal Linux image
- Services:
  - Capture service (camera → frames)
  - Inference service (pose → landmarks)
  - Posture service (features → state/score → feedback policy)
  - Local web UI (setup, calibration, privacy settings)
  - Sync agent (aggregate upload only; opt-in more)
  - OTA updater (signed artifacts)

### Data handling (privacy-first defaults)
- Never store raw video by default
- Store per-minute aggregates locally
- Upload only aggregates unless user opts-in
- Encryption at rest; rotate device keys; secure provisioning

---

## 6) Prototype build steps (practical)
1. Choose compute option (A/B) + camera
2. Run the same pose pipeline as the desktop MVP
3. Add LED feedback via GPIO (or small microcontroller via USB)
4. Build local setup portal (Wi‑Fi, calibration, notification settings)
5. Implement “uncertain” detection (no cues when landmarks unreliable)
6. Put it in a simple 3D printed enclosure with diffused LED window + shutter

---

## 7) V2 upgrades (once v1 works)
- Depth/ToF sensor variant for better low-light and privacy comfort
- Multi-angle support (secondary camera) for occlusion-heavy setups
- Battery option (short runtime) for cable-free desks (usually not necessary)
- Enterprise management (fleet provisioning, aggregated analytics, policy controls)
