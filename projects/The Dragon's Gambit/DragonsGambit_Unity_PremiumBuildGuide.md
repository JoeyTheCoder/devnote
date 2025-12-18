# The Dragon’s Gambit — Unity Build Guide (Solo, Premium UI, Core Mechanics)

You’re building a **UI-first 2D card/table game**. This guide is a **professional-but-solo-friendly** way to set up Unity so the end result feels *premium*: clean UI, consistent typography, satisfying feedback, and maintainable code.

> Scope focus: **Main Menu**, **Player Table**, **Cards**, **Texts**, **Points/Scoring**, and the “glue” systems that make those feel polished.

---

## 0) Unity version and target stack

### Recommended Unity setup (as of late 2025)
- Use **Unity 6 LTS** (currently Unity 6.3 LTS is the supported LTS line).  
- Use **URP** (2D or standard) for consistent post-processing, crisp UI compositing, and better control.

### UI tech choice (simple + premium)
- For a first shipped project: **UGUI + TextMeshPro**.
  - It’s still the most straightforward for **screen-space HUD**, drag/drop cards, and quick iteration.
- Consider **UI Toolkit** later if you want UXML/USS workflows and data binding, but don’t let it slow you down early.

**Packages you’ll likely use**
- TextMeshPro (TMP)
- Input System (optional but recommended for clean pointer handling)
- Addressables (optional; useful later for content scaling)
- Unity UI (UGUI)

---

## 1) Create the project “like a studio”

### 1.1 Create project
1. Unity Hub → **New Project**
2. Template: **2D (URP)** (or URP if you plan subtle post-processing)
3. Name: `DragonsGambit`
4. Organization: use a reverse domain or personal namespace

### 1.2 Project settings checklist (10 minutes that save weeks)
**Edit → Project Settings**
- Player
  - Company Name / Product Name
  - Default icon (temporary is fine)
- Quality
  - Pick a single quality level per platform early
- Time
  - Fixed timestep default is fine; you’re UI-heavy
- Input
  - If using Input System: set “Active Input Handling” to **Both** (for compatibility)

### 1.3 Version control (non-negotiable)
- Use **Git** (GitHub/GitLab/Bitbucket).
- Add a Unity `.gitignore` (Unity provides standard patterns).
- Commit early: after project creation, after folder structure, after first playable loop.

### 1.4 Naming + organization rules
**Principles**
- Everything is searchable and predictable.
- Prefabs are composable, not monolithic.
- Data is separated from visuals.

**Naming convention (example)**
- Scenes: `SCN_MainMenu`, `SCN_Game`, `SCN_Loading`
- Prefabs: `PF_Card`, `PF_PlayerTable`, `PF_Popup`, `PF_ButtonPrimary`
- ScriptableObjects: `SO_Card_*`, `SO_Config_*`
- Scripts: `CardView`, `CardDefinition`, `GameState`, `ScoreSystem`

---

## 2) Folder structure (professional, scalable)

Create this under `Assets/_Project/`:

```
_Project/
  Art/
    AI/
    Icons/
    Sprites/
    UI/
    Fonts/
  Audio/
    SFX/
    Music/
  Materials/
  Prefabs/
    UI/
    Cards/
    Table/
  Scenes/
  Scripts/
    Core/
    Data/
    UI/
    Systems/
    Util/
  ScriptableObjects/
    Cards/
    Config/
  Shaders/
  Tests/
  ThirdParty/
```

**Rules**
- Never put your stuff at the root `Assets/` except `_Project/`.
- No “Misc” folders.
- Prefabs only reference assets inside `_Project/` (avoid hidden dependencies).

---

## 3) The “premium look” formula (UI game edition)

Premium feel is 80% *consistency* + 20% *feedback*.

### 3.1 Visual consistency checklist
- A **single** font family for body + a **single** display font for headers.
- A unified **color palette**: background / surface / accent / warning / success.
- A consistent **spacing system** (8px grid): padding and margins in steps of 8.
- Buttons have **three states**: Normal / Hover / Pressed + Disabled state.
- Cards have consistent **corner radius**, shadow, border weight.

### 3.2 Feedback checklist
- Every interaction gives:
  - A micro animation (scale 1.0 → 1.03 on hover, 1.0 → 0.97 on press)
  - A short UI sound (click, hover, confirm)
  - A tiny highlight/glow for focus
- Transitions:
  - Fade between scenes
  - Slide panels rather than hard swaps
- Readability:
  - Use TMP outline/underlay carefully, not heavy strokes

---

## 4) Scenes and flow

Create 3 scenes:

1. `SCN_Boot`
   - Initializes singletons/services
   - Loads config
   - Goes to Main Menu
2. `SCN_MainMenu`
   - Title, Play, Rules, Settings, Quit
3. `SCN_Game`
   - Table, players, hand, deck, points, end round popup

> Keep “Boot” tiny and stable. Everything else can change.

---

## 5) Data-driven cards (ScriptableObjects) — your core mechanic foundation

### 5.1 Create CardDefinition
Create a ScriptableObject:

**`CardDefinition` fields**
- `string id`
- `string displayName`
- `Sprite artwork`
- `Sprite frame` (optional)
- `int baseValue` (if you have)
- `CardType type` (enum)
- `Rarity rarity` (optional)
- `string rulesText`
- `Cost cost` (if needed)
- `Effect[] effects` (later; keep simple initially)

Store them in: `Assets/_Project/ScriptableObjects/Cards/`

**Why this matters**
- You can iterate balance and visuals without touching code.
- You can generate/replace art without prefab surgery.

### 5.2 CardView prefab
Create `PF_Card` under `Prefabs/Cards/`.

**Prefab UI structure (UGUI)**
- `CardRoot` (RectTransform + Image for background/frame)
  - `Artwork` (Image)
  - `Name` (TMP)
  - `RulesText` (TMP)
  - `ValueBadge` (Image + TMP)
  - `HoverGlow` (Image, disabled by default)
  - `DragHandle` (invisible Image to catch raycasts)

**CardView script responsibilities**
- `Bind(CardDefinition def)` sets sprites/text
- Handle hover/press visual states
- (Optional) Implement drag & drop hooks

### 5.3 Drag and drop (beginner-safe pattern)
Use Unity’s event interfaces:
- `IBeginDragHandler`, `IDragHandler`, `IEndDragHandler`, `IPointerEnterHandler`, `IPointerExitHandler`

**Guidelines**
- On begin drag: move card to a top-level `DragLayer` so it renders above everything.
- Use a **placeholder** where the card came from to avoid layout snapping.
- On end drag: raycast target area (`DropZone`) and decide accept/reject.

---

## 6) Player table layout (clean + scalable)

### 6.1 Table prefab
Create `PF_PlayerTable` with:
- `TableBackground` (Image)
- `PlayerArea_Top` (opponent)
- `PlayerArea_Bottom` (you)
- `CenterRow` (shared area: current round, announcements)
- `HUD_TopRight` (settings/help)
- `HUD_BottomLeft` (points summary)

Use Layout Groups:
- Horizontal/Vertical Layout Groups for hands/rows
- Content Size Fitter only where necessary (overuse causes jitter)

### 6.2 Hand zones
Create `DropZone` components for:
- Player hand
- Player played area
- Discard pile (if applicable)

Each zone defines:
- Accepts which card types
- Max cards
- Visual highlight rules

---

## 7) Text and points (readability first)

### 7.1 TextMeshPro setup
1. Window → TextMeshPro → Import TMP Essentials
2. Use TMP everywhere (no legacy Text).

**Fonts**
- Start with 1–2 free fonts:
  - A readable UI font (e.g., Inter, Source Sans)
  - A fantasy-ish display font for title only (don’t overuse)

**TMP style rules**
- Body: 28–40 px depending on resolution (use Canvas Scaler)
- Use line spacing slightly increased for rules text
- Avoid full caps for long sentences

### 7.2 Points UI
Create `PF_PointsPanel`:
- `PlayerPoints` (TMP)
- `OpponentPoints` (TMP)
- `RoundLabel` (TMP)
- `StatusLine` (TMP): short messages (“Opponent played…”, “You drew…”)

Use an animated “count up” effect:
- When score changes, animate number over 0.2–0.4s
- Add a subtle scale punch

---

## 8) Main menu (premium without effort)

### 8.1 Layout
`SCN_MainMenu` canvas:
- Background artwork (AI)
- Dark overlay gradient (for readability)
- Logo/title (TMP)
- Button column (Play / How to Play / Settings / Quit)
- Version label bottom corner (`v0.1.0`)

### 8.2 Menu polish
- Button hover glow + sound
- Scene transition fade
- Optional: subtle animated background (slow parallax, particles)

---

## 9) “Premium UI kit” you build once and reuse

Create prefabs:
- `PF_ButtonPrimary`, `PF_ButtonSecondary`
- `PF_Panel` (9-sliced)
- `PF_Popup` (title + content + buttons)
- `PF_Tooltip`
- `PF_Toasts` (small notifications)

Make a `UITheme` ScriptableObject:
- Colors (background/surface/accent)
- Font assets (TMP font assets)
- Default sizes

This lets you “reskin” the whole game later.

---

## 10) High-quality assets on a hobby budget (AI-first pipeline)

You want consistency more than detail. The best solo approach is:

### 10.1 Lock a style guide first
Write a tiny “style bible” (one page):
- Mood: dark fantasy, elegant, not noisy
- Lighting: soft rim light, high contrast focal point
- Palette: 3–5 main hues
- Composition: centered subject, negative space for UI text

### 10.2 AI artwork workflow (repeatable)
**Step A — Generate**
- Generate **large** (at least 1024–2048 square if possible).
- Always keep the same prompt skeleton:
  - subject + style + lighting + palette + background simplicity + “no text”.

**Prompt skeleton example**
> “fantasy playing card illustration of [SUBJECT], painterly digital art, soft rim lighting, rich but limited palette, subtle vignette, clean background with minimal noise, centered composition, no text, no watermark, high detail”

**Step B — Curate**
- Pick 1 “hero” image per card.
- Reject anything with messy hands, unreadable shapes, or inconsistent perspective.

**Step C — Post-process**
Use free tools:
- **Photopea** (browser) or **GIMP** for:
  - cropping to card aspect ratio
  - mild contrast curve
  - sharpening lightly
  - removing artifacts
- Optional: free upscalers (careful—don’t over-sharpen)

**Step D — Build UI-ready sprites**
- Export as PNG
- Create separate layers (if possible):
  - `artwork`
  - `frame`
  - `icons`
  - `rarity gem`

### 10.3 9-slice panels (huge premium boost)
Design a single panel image with corners + edges.
In Unity:
- Sprite Editor → set borders
- Now all your menus/panels scale cleanly.

### 10.4 Icons (cheap, consistent)
- Use a single icon set style (line icons OR filled icons).
- Keep stroke width consistent.
- Recolor in Unity via Image color rather than generating multiple files.

---

## 11) Implementation walk-through (core loop scaffold)

### 11.1 Create the minimum playable UI
1. Build `SCN_Game`:
   - Canvas (Screen Space - Overlay)
   - `PF_PlayerTable` placed
   - Add `DragLayer` (empty RectTransform on top)
2. Create 5 test `CardDefinition` assets.
3. Create a `HandController` that:
   - Instantiates `PF_Card` for each card in hand
   - Binds card data
4. Create `DropZone_PlayArea`:
   - Accept cards from hand
   - On drop: move card into played area

### 11.2 Add points
1. Add `ScoreSystem`:
   - Track player/opponent scores
   - Expose events: `OnScoreChanged`
2. `PointsPanel` listens and animates changes.

### 11.3 Add main menu → game transition
1. `SCN_MainMenu` “Play” button loads `SCN_Game` via a small `SceneLoader`
2. Use a fade:
   - Fullscreen Image alpha animate
   - Load scene
   - Fade out

---

## 12) Quality bar checklist (what makes it feel “shipped”)

**UI**
- No misaligned elements
- No inconsistent fonts
- No low-res art in final resolution
- Hover/press feedback everywhere

**Game feel**
- Card movement is eased (not linear)
- Sounds are subtle and consistent
- Clear state messaging (whose turn, what just happened)

**Engineering**
- No logic in UI scripts beyond view behavior
- Card rules/data live in ScriptableObjects
- Prefabs are reusable; no scene-only hacks

---

## 13) Suggested “first milestone” (1–2 weeks hobby pace)

Deliver:
- Main menu with Play/Rules/Quit
- Game scene with:
  - Player hand (5 cards)
  - Drag a card to play area
  - Points increase when played
  - End turn button toggles a simple “opponent plays” simulation
- 10 cards with consistent AI art style
- 1 polished popup (end of round)

---

## 14) Common beginner traps (avoid these)

- **Overusing Content Size Fitter** → causes layout jitter and performance issues.
- **Hard-coding rules in UI** → makes balance painful.
- **Mixing pixels and relative sizes** without Canvas Scaler strategy.
- **Chasing perfect art early** → lock style + generate iteratively.

---

## Appendix A — Unity editor “click path” quick notes

- Create ScriptableObject:
  - Right click in Project → Create → _Project → CardDefinition
- Create TMP text:
  - UI → Text - TextMeshPro
- Sprite slicing:
  - Select sprite → Sprite Editor → Slice / Borders
- Prefab workflow:
  - Build object in scene → drag to `Prefabs/` folder → apply changes via Prefab Mode

---

## Appendix B — Minimal scripts you’ll need (names only)

- `CardDefinition : ScriptableObject`
- `CardView : MonoBehaviour`
- `HandController : MonoBehaviour`
- `DropZone : MonoBehaviour`
- `ScoreSystem : MonoBehaviour`
- `SceneLoader : MonoBehaviour`
- `UIAudio : MonoBehaviour` (central click/hover sounds)
- `UITheme : ScriptableObject`

---

If you want, I can generate a **starter Unity project scaffold** (folder structure + empty scripts + prefab checklists) in another markdown file you can follow line-by-line while building.
