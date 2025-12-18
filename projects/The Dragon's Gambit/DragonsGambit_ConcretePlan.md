# The Dragon’s Gambit — Concrete Dev Plan (Beginner-Friendly)

This plan is based on your design docs: **concept & pillars** fileciteturn0file2, **rules** fileciteturn0file1, and your **logic/flow breakdown** fileciteturn0file0.

---

## 1) “Is Unity the best option?” — Engine choices for *this* game

Your game is a **2D, UI-heavy, turn-based card game** with:
- lots of menus + readable animations
- deterministic rules + hidden information
- PC (Steam) + Mobile (iOS/Android) target

### Best-fit options (ranked)

**Option A — Unity (recommended if you want widest platform + learning resources)**
- Pros: huge ecosystem + tutorials, excellent multiplatform shipping story (Steam + iOS/Android), solid tooling and profilers.
- Considerations: licensing is subscription-based for larger revenue/funding tiers; Unity cancelled the controversial Runtime Fee and updated pricing (effective 2025) so the main “risk” is predictable seat pricing rather than per-install fees. citeturn0search0turn0search4turn0search26
- Unity 6.3 LTS is current and intended for production stability. citeturn1search0turn1search2turn1search6

**Option B — Godot (recommended if you want lightweight + fully open source)**
- Pros: fast iteration, very approachable for small 2D games, no licensing cost.
- Considerations: mobile export is doable, but iOS export still requires macOS + Xcode, and (if you choose C#) mobile has extra platform constraints that you’ll want to validate early. citeturn1search3turn1search14

**Option C — Web stack (React + Phaser / PixiJS) packaged for desktop/mobile**
- Pros: great for UI-heavy card games, extremely fast UI iteration, web-native animations.
- Considerations: shipping to iOS/Android via wrappers (Capacitor/Cordova) adds integration/testing complexity; Steam is fine (Electron) but memory footprint can be higher.

**Option D — Unreal**
- Overkill for a UI-first 2D card game; best reserved for 3D/real-time rendering heavy titles. (Still great tech, just not the best “time-to-fun” here.) citeturn0search1turn0search3

### Recommendation
If you’re new to game dev and want the smoothest path to **Steam + mobile**, stick with **Unity** (Option A) — *but* structure the code so your rules are **engine-agnostic**, so switching later is survivable.

---

## 2) Target product: a “premium-feel” vertical slice

Your stated goal is a **calm, restrained fantasy tone with strong UX clarity** fileciteturn0file2.  
So the plan is to build **one polished slice** first:

**Vertical Slice Definition (what “done” looks like):**
- 2–4 players local hot-seat
- full round loop: setup → turns → Final Gambit → reveal → scoring fileciteturn0file0
- all special abilities implemented fileciteturn0file1
- clean UI with short, readable animations (draw / swap / reveal / ability)
- basic bot (optional) OR “pass-and-play” UI that feels great

Once this slice feels premium, expanding to online, cosmetics, progression, etc. becomes much safer.

---

## 3) Unity project setup (modern, stable defaults)

### Unity version
- Use **Unity 6.3 LTS** for production stability and longer support window. citeturn1search0turn1search6

### Render pipeline
- Use **URP** (even for 2D) for consistent cross-platform lighting/post and easy “premium” polish.

### Repository + structure (do this on day 1)
- Git repo + `.gitignore` for Unity
- Folder layout:
  - `Assets/DragonsGambit/Core/` *(engine-agnostic rules & state)*
  - `Assets/DragonsGambit/Presentation/` *(UI, VFX, anim, audio)*
  - `Assets/DragonsGambit/Content/` *(ScriptableObjects, sprites, fonts)*
  - `Assets/DragonsGambit/Tests/` *(PlayMode + EditMode tests)*
  - `Assets/DragonsGambit/Tools/` *(editor utilities)*

### UI tech choice (pragmatic)
Unity has two main UI approaches:
- **UGUI**: mature, lots of examples, great for quick shipping.
- **UI Toolkit**: improving quickly in Unity 6 (and has dedicated guides), but still has practical tradeoffs depending on what you need. citeturn1search7turn1search21turn1search12

**Recommendation for a first game:** start with **UGUI** for core gameplay screens; optionally experiment with UI Toolkit for menus later. (You can mix.)

---

## 4) Architecture: make the rules “pure” and testable

Your docs already hint at the right split (Rules vs State vs Presentation) fileciteturn0file2 — lean into that.

### Core principles
- **No Unity references** in Core (no `MonoBehaviour`, no `GameObject`)
- Core runs from a single function: `Apply(Action, GameState) -> GameState + Events`
- Presentation listens to Events and plays animations; UI dispatches Actions

### Minimal data model (start small)
- `CardDefinition` (id, baseValue, abilityType)
- `CardInstance` (definitionId, currentValue, revealedToPlayersMask)
- `PlayerState` (hand[4], knownInfo, score)
- `GameState` (deck, discard, currentPlayerIndex, phase, finalGambitCalledBy?)
- `Action` (DrawFromDeck, TakeDiscard, Swap(handIndex), DiscardDrawn, CallFinalGambit, ResolveAbilityChoice…)
- `Event` (CardDrawn, CardSwapped, CardRevealedToPlayer, AbilityTriggered, RoundEnded…)

### Why this matters
- You can unit test your entire ruleset (including edge cases like deck depletion reshuffle) fileciteturn0file0
- You can later add online multiplayer by syncing Actions, not GameObjects

---

## 5) Milestone plan (step-by-step)

### Milestone 0 — Learn enough Unity to be dangerous (1–2 days)
- Scenes, prefabs, script basics
- Canvas + basic UGUI buttons
- ScriptableObjects basics (for cards/abilities)

Deliverable: “Hello menu” scene + a dummy hand UI.

---

### Milestone 1 — Implement the rules in pure C# (no UI) (3–7 days)
Implement exactly what’s in your rules + logic docs:
- setup: deck composition, shuffle, deal 2×2 hands, initial peek fileciteturn0file1
- turn loop: draw deck / take discard / call Final Gambit fileciteturn0file0
- scoring + penalty rule fileciteturn0file1
- edge cases: deck depletion reshuffle, value clamp 0–13 fileciteturn0file0

Deliverable: run 1000 simulated rounds with deterministic seeds and assert invariants:
- hand size always 4
- discard always has a top card
- values always clamped 0–13
- endgame order correct (caller stops; others get one final turn) fileciteturn0file0

---

### Milestone 2 — Minimal playable prototype (ugly but functional) (3–7 days)
- One scene: tabletop layout
- “Pass device” flow between players
- Buttons for the three turn choices
- Simple reveal panel for “private” info (drawn card, peek effects)

Deliverable: complete a full round with 2 players without bugs.

---

### Milestone 3 — Abilities + UX clarity pass (5–10 days)
Implement your abilities list fileciteturn0file1 with a consistent pattern:
- Ability triggers create an `Event: AbilityTriggered(abilityType)`
- UI shows a **single** modal prompt describing the choice
- Player makes choice → sends an Action back to Core → Core emits resolution events

Deliverable: every ability is playable and readable (even if art is placeholder).

---

### Milestone 4 — Premium look & feel “vertical slice” (2–4 weeks)
This is where the game becomes *sellable*.

**UI/UX**
- Strong hierarchy: “what can I do?” and “what changed?” always obvious fileciteturn0file2
- Consistent spacing, typography, and iconography
- Animations:
  - Draw: card lifts, slides, settles
  - Swap: quick exchange + discard snap
  - Reveal: short flip
  - Ability: subtle “rune pulse” + highlight target

**Audio**
- 6–10 high-quality UI sounds (draw, swap, confirm, cancel, reveal, end-round)
- One calm ambient loop + light stingers (Final Gambit call)

**VFX**
- Subtle particles only (no fireworks): frost motes, rune glow, soft bloom
- URP post: mild vignette, subtle color grading (cool/ice tone)

Deliverable: vertical slice that feels “premium” with placeholder art still acceptable.

---

### Milestone 5 — Content + progression (optional, after slice)
- deck variants / rule toggles
- cosmetics (card backs, table skins, dragon frames)
- onboarding tutorial (first 2 rounds guided)

Deliverable: “shipping candidate” feature set for Steam Early Access or mobile soft launch.

---

## 6) Concrete UI screens checklist (don’t skip)

**Required**
- Title screen
- Lobby: player count (2–6), local/AI toggle
- Rules reference (in-game)
- Gameplay HUD
- Ability prompt modal
- End-of-round summary (who won, totals, penalty explanation)
- Match summary / rematch

**Nice-to-have**
- “Memory notes” UI (player can mark what they think they have; doesn’t reveal truth)
- Accessibility: text size, colorblind-friendly highlights, reduce motion

---

## 7) Quality bar: the “premium feel” checklist

- **Input feels instant** (no UI button delays)
- Every state change has:
  - a clear visual change
  - a short sound
  - a 0.2–0.6s readable animation
- No clutter: only show *current* player decisions; others see a clean waiting state
- Always explain *why* something happened (ability log line)

---

## 8) If you want to pick Godot instead (quick translation)

This exact plan still works:
- Core rules remain engine-agnostic
- Replace Unity UI with Godot UI scenes
- Validate iOS export early (macOS + Xcode required) citeturn1search3

---

## 9) Next actions (do these in order)

1) **Lock rules + abilities wording** (tiny clarifications now prevents UI rework later) fileciteturn0file1  
2) Create repo + Unity 6.3 LTS project citeturn1search0  
3) Build Core rules + tests (Milestone 1) fileciteturn0file0  
4) Build ugly playable prototype (Milestone 2)  
5) Make the vertical slice premium (Milestone 4)  

---

## Appendix: “rule clarifications” you should decide early
These don’t change the fun, but they change implementation/UI:

- When you draw a card with an ability: does it trigger **only if discarded**, **only if swapped**, or **always when drawn**? (Your logic doc assumes “if applicable, trigger immediately” fileciteturn0file0 — make this explicit per card.)
- Does taking top discard ever trigger ability, or only the card you *place* on discard?
- Exact penalty amount for a failed Final Gambit call fileciteturn0file1
- Tiebreak rules when multiple players share lowest hand fileciteturn0file0

