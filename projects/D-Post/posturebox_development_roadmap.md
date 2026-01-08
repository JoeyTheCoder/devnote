# PostureBox — Development Roadmap (Prototype → Pilot)

## Phase 0 — Clarify the “minimum lovable loop” (1–3 days)
**Goal:** define posture states and feedback rules.
- Posture states (v1): Neutral / Slouched / Lean-L / Lean-R / Twisted / Away
- Feedback policy: only alert if “bad posture” persists for N seconds; cooldown; focus mode
- Local-only mode supported from day 1

**Outputs**
- Spec: posture features, thresholds, and confidence gating
- UX wireframe: calibration + daily summary + settings

---

## Phase 1 — Software MVP (webcam, on-device) (1–3 weeks)
**Goal:** validate user value and retention before hardware.

### Core features
- On-device pose estimation (2D/3D landmarks)
- Skeleton overlay (local only)
- Feature extraction + smoothing
- Rule-based posture scoring
- Nudges: desktop notifications (or subtle sound), cooldowns, focus mode
- Daily summary (minutes neutral vs slouched; streaks; basic insights)
- Data storage locally; optional cloud sync of aggregates only

### Engineering tasks
- Pose pipeline wrapper (frame capture → landmarks)
- Confidence gating (no judgement when uncertain)
- Temporal smoothing (EMA / window majority vote)
- Feature extraction (angles/distances)
- Notification engine with rate limits
- Local database (SQLite) + export
- Optional API for aggregate upload

### Success criteria
- ≥20 testers run it for at least 3 workdays
- ≥40% day-7 retention (indicative)
- Users report “helpful without being annoying”
- Early willingness-to-pay signals

---

## Phase 2 — Edge prototype (“the box”) (2–6 weeks)
**Goal:** prove real desk placement and hardware feedback.

### Features
- Same on-device pipeline running on edge hardware
- Physical feedback: LED ring + optional gentle chime
- Local setup portal (Wi‑Fi, calibration, thresholds)
- Secure device identity + encrypted transport
- OTA update mechanism (signed updates)

### Engineering tasks
- Hardware selection + OS image
- Camera driver + low-latency capture
- On-device inference optimization
- LED/buzzer control firmware (if using microcontroller) or direct GPIO
- Device provisioning + keys
- Local web UI (config + diagnostics)
- Telemetry (aggregate only; opt-in detailed)

### Success criteria
- Runs stable for 8+ hours/day without overheating
- <2% of time in “uncertain” mode after calibration (target; adjust)
- Users accept desk placement + cues

---

## Phase 3 — Calibration + personalization (2–6 weeks)
**Goal:** reduce false positives and improve comfort.
- Guided calibration: “sit neutral”, “slouch”, “lean left/right”
- Per-user threshold learning (simple)
- Adaptive cue scheduling based on user response
- Multiple monitor / rotated posture handling via calibration profiles

---

## Phase 4 — Pilot readiness (4–10 weeks)
**Goal:** small paid pilots and procurement-friendly posture.

### Security & privacy
- Encryption in transit and at rest
- Clear data controls: local-only default; retention controls
- Transparent privacy policy and on-device processing statement

### Product
- Companion app/dashboard
- Admin basics (for B2B pilot): device status, aggregated team stats (opt-in)

### Compliance planning (later, but prep now)
- FCC/CE path, data protection considerations, avoid medical claims

---

## Phase 5 — Manufacturing feasibility (later)
**Goal:** transition from dev kit to product.
- Compute module/custom board
- Injection-mold enclosure
- Camera module selection and supply chain
- Cost-down roadmap + reliability testing
