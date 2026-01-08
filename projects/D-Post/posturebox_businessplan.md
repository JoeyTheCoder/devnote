# PostureBox — Short Business Plan (Draft)

## 1) Summary
**PostureBox** is a small desk device (or software-first app) that **detects sitting posture locally** using on-device pose estimation, then provides **low-friction coaching** to reduce sustained slouching and asymmetry—without streaming video to the cloud. The core differentiator is **privacy by design**: camera frames are processed on-device; by default, only **posture scores and aggregates** sync to the user account.

## 2) Problem
Desk workers spend hours sitting; many develop habits like forward-head posture, rounded shoulders, or asymmetrical leaning. Existing solutions often fail because:
- Wearables require daily compliance and can be annoying.
- Cloud camera solutions raise privacy concerns.
- One-off ergonomic assessments don’t create lasting behavior change.

## 3) Solution
A desk companion that:
- Runs **pose → skeleton** on-device.
- Computes posture quality with confidence gating and temporal smoothing.
- Nudges gently when **poor posture is sustained** (not constantly).
- Tracks trends over time (minutes “neutral” vs “slouched”, asymmetry, breaks).
- Syncs **aggregated stats** (no images; skeleton optional opt-in).

## 4) Target Customers & Beachhead
### Beachhead (0→1)
- Remote/hybrid knowledge workers (25–45), tech-savvy, already buying productivity/wellness tools.
- Pain is acute: long hours, minimal ergonomic oversight.

### Expansion
- Teams / employers (HR, People Ops, Workplace): ergonomic wellness programs, RSI prevention, remote-work support.

## 5) Value Proposition
- **Privacy-first** posture coaching: “No video leaves the device.”
- **No wearable** friction; always available on the desk.
- **Behavior change** system: nudges + trends + goals + focus modes.
- Potential employer benefits: fewer complaints, higher comfort, measurable engagement.

## 6) Differentiation
- On-device processing + default “aggregates only” cloud sync.
- Calm UX: low-interruption nudges, “deep work” suppression, user-tuned thresholds.
- Setup guidance + per-user calibration to handle real-world desks.

## 7) Business Model
### D2C
- Hardware margin + optional subscription.
  - Example bundles (illustrative): device + free basic tracking; “Pro” adds advanced analytics, multi-device sync, coaching programs, and employer reporting.

### B2B
- Per-seat or per-device monthly fee.
- Admin dashboard with **privacy-preserving aggregated reporting** (no individual surveillance positioning; opt-in, employee-controlled).

## 8) Go-to-Market
- **Software-first MVP** (webcam app) to validate retention and willingness-to-pay.
- Communities: remote work, developers, gamers, coworking groups, ergonomics subreddits, LinkedIn.
- Content: “Desk posture score”, “ergonomics checklist”, “30-day posture challenge”.
- Partnerships later: ergonomic chair/desk sellers, workplace wellness vendors.

## 9) Key Metrics (what you’ll track early)
- **Activation**: completes calibration + first day running > 2 hours.
- **Retention**: % active on day 7 / day 30.
- **Nudge acceptance**: nudges per hour and % followed by improvement.
- **Outcome proxy**: reduction in “sustained slouch minutes/day.”
- **WTP**: survey + conversion to paid plan/pilot.

## 10) Risks & Mitigations
- **Robustness**: lighting/occlusion/chair backs → confidence gating, placement guide, wide-angle lens, “pause when uncertain.”
- **Annoyance**: too many alerts → cooldowns, focus mode, “only sustained” policy, silent cues.
- **Privacy**: skeleton data sensitivity → default aggregates only, local-only mode, user-controlled retention, encryption.
- **Claims**: avoid medical positioning → wellness coaching language + disclaimers.

## 11) Milestones
1. MVP (software-only): on-device pose + posture score + gentle nudges + daily trend.
2. Pilot with 20–50 users: retention + WTP signals.
3. Prototype box: edge compute + LED/sound + companion app.
4. Small paid pilots (teams): validate B2B dashboard and procurement path.
