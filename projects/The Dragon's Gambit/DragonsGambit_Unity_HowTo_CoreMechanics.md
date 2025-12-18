# The Dragon’s Gambit — Unity Build Guide (Step‑by‑Step, Premium UI, Solo‑Friendly)

This is a **hands-on “how to create it” walkthrough**. It’s organized by **what we are building**, with exact Unity menu paths, inspector settings, and a lean architecture that scales.

Assumptions:
- You’re building a **2D, UI‑forward card game** (mainly Canvas/UI).
- You’re new to Unity, working solo, budget‑limited, and using AI for art.

---

## 0) Project foundation (do this once)

### 0.1 Create the Unity project
1. Open **Unity Hub** → **New project**
2. Template: **2D (URP)** *(optional: regular 2D is fine; URP gives better control if you later add lighting/post)*
3. Name: `DragonsGambit`
4. Location: wherever your Git repo will live.

> URP 2D setup steps if needed are in Unity docs. citeturn0search5

### 0.2 Install essential packages
In Unity: **Window → Package Manager**
- **TextMeshPro** (usually included, but ensure it’s installed) citeturn0search3turn0search19  
- **2D Sprite** packages (Sprite, PSD Importer if you want)
- **Addressables** *(optional early, recommended if you expect many assets later)* citeturn0search16turn0search1

### 0.3 Import TextMesh Pro essentials
1. **Window → TextMeshPro → Import TMP Essential Resources**
2. Optional: Import Examples & Extras (useful references)

### 0.4 Project Settings for “premium feel”
**Edit → Project Settings**
- **Player**
  - Color Space: **Linear** (better gradients/lighting; may affect some sprites)
  - Resolution: set a target later (we’ll do UI scaling properly)
- **Quality**
  - VSync: **On** for menus (reduce tearing)
- **Editor**
  - Version Control Mode: **Visible Meta Files**
  - Asset Serialization: **Force Text**

### 0.5 Professional folder structure
In **Project** window, create:

```
Assets/
  _Project/
    Art/
      UI/
      Cards/
      Icons/
      Backgrounds/
    Audio/
      SFX/
      Music/
    Fonts/
    Prefabs/
      UI/
      Gameplay/
    Scenes/
    Scripts/
      Core/
      UI/
      Gameplay/
      Data/
    ScriptableObjects/
      Cards/
      Config/
    Materials/
    Shaders/
    Atlases/
    Animations/
    Resources/ (avoid if possible)
```

Naming rules:
- Prefabs: `PF_Thing` (e.g., `PF_Card`, `PF_PlayerArea`)
- Scenes: `SC_MainMenu`, `SC_Game`
- ScriptableObjects: `SO_Card_FlameDragon`

### 0.6 Git setup (strongly recommended)
- Create a repo, commit early.
- Add a Unity `.gitignore` (standard one).

---

## 1) What we’re building first
Core scope you requested (plus a few essentials):
1. **Main Menu** (Start Game, Options, Quit)
2. **Scene flow** (menu → game)
3. **Gameplay screen layout**
   - Player table (2–4 players)
   - Hand grids (2×2 facedown)
   - Draw/Discard areas
4. **Card system**
   - Card data (ScriptableObjects)
   - Card visuals (prefab)
   - Flip/reveal animation
5. **Text + points**
   - Scoreboard / points display
   - Turn indicator
   - Log panel (small, but makes the game feel “real”)
6. **Premium UI pass**
   - Typography, spacing, transitions, audio ticks
   - Consistent art pipeline (AI-assisted)

---

## 2) Build the Scene architecture

### 2.1 Create scenes
1. In `Assets/_Project/Scenes/`
2. **File → New Scene**
3. Save as:
   - `SC_MainMenu`
   - `SC_Game`

### 2.2 Create a “bootstrap” object (recommended)
Goal: keep managers stable and avoid duplicated singletons.

In `SC_MainMenu`:
1. **GameObject → Create Empty** → name `Bootstrap`
2. Add scripts (we’ll create soon):
   - `App.cs` (scene navigation, persistent config)
3. Mark it as persistent in code (`DontDestroyOnLoad`)

---

## 3) What we’re building: Main Menu (UGUI + TMP)

> Many Unity devs still recommend **UGUI + TextMeshPro** for shipping UI. citeturn0search7turn0search3  
> UI Toolkit is improving fast in Unity 6.x (and adds nice features), but for a first project, UGUI is usually the fastest path. citeturn0search14turn0search2turn0search22

### 3.1 Create the Canvas correctly
In `SC_MainMenu`:
1. **GameObject → UI → Canvas**
2. Select **Canvas**:
   - Render Mode: **Screen Space - Overlay**
   - Add/ensure **Canvas Scaler**:
     - UI Scale Mode: **Scale With Screen Size**
     - Reference Resolution: **1920×1080**
     - Screen Match Mode: **Match Width Or Height**
     - Match: **0.5**
3. **GameObject → UI → Event System** (Unity usually adds it automatically)

### 3.2 Build the menu layout
Under Canvas:
1. Create **Panel** (UI → Panel) named `BG`
   - Color: dark gradient (use an Image sprite later)
2. Create empty `MenuRoot` (RectTransform stretch full)
3. Under `MenuRoot`:
   - `Title` (TMP Text)
   - `ButtonGroup` (Empty + Vertical Layout Group)
   - Buttons: `Start`, `Options`, `Quit`

**How to create a TMP Title**
1. **GameObject → UI → Text - TextMeshPro**
2. Rename to `Title`
3. In Inspector:
   - Font Asset: pick a clean display font
   - Alignment: Center
   - Font Size: ~72
   - Enable: **Extra Padding** (improves crispness)
   - Set margins (top spacing)

**How to create a premium button**
1. **GameObject → UI → Button - TextMeshPro**
2. Rename to `BTN_Start`
3. In the Button’s child text, set:
   - Text: `Start Game`
   - Font Size: 36–44
4. On the button Image:
   - Use a **9-sliced** sprite (see “Assets” section)
   - Add subtle drop shadow: **Add Component → Shadow**
5. Add hover/press feel:
   - Button component → Transition: **Color Tint** (easy) or **Animation** (premium)

### 3.3 Wire buttons to scene loading
Create script: `Assets/_Project/Scripts/Core/App.cs`

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class App : MonoBehaviour
{
    public static App I { get; private set; }

    private void Awake()
    {
        if (I != null) { Destroy(gameObject); return; }
        I = this;
        DontDestroyOnLoad(gameObject);
        Application.targetFrameRate = 60;
    }

    public void LoadGame() => SceneManager.LoadScene("SC_Game");
    public void Quit()
    {
#if UNITY_EDITOR
        UnityEditor.EditorApplication.isPlaying = false;
#else
        Application.Quit();
#endif
    }
}
```

Attach:
1. Select `Bootstrap`
2. **Add Component → App**
3. Select `BTN_Start` → Button → **On Click()**
   - Drag `Bootstrap` into the slot
   - Choose `App → LoadGame`
4. Select `BTN_Quit` similarly → `App → Quit`

---

## 4) What we’re building: Gameplay Screen layout (Player Table)

### 4.1 Create the Game UI hierarchy
Open `SC_Game`:
1. Create **Canvas** (same Canvas Scaler settings as menu)
2. Under Canvas create:
   - `TopBar` (turn indicator, points)
   - `TableRoot` (player areas)
   - `CenterZone` (draw pile + discard pile)
   - `BottomBar` (actions: Draw, Reveal, Swap, End Turn)
   - `LogPanel` (optional but recommended)

### 4.2 Build the table areas (2–4 players)
Create a prefab `PF_PlayerArea`:
1. In Hierarchy: **UI → Panel**, rename `PlayerArea`
2. Inside it:
   - `Name` (TMP)
   - `Score` (TMP)
   - `HandGrid` (Empty + GridLayoutGroup)

**HandGrid settings**
- GridLayoutGroup:
  - Cell Size: e.g. **160×220** (depends on your card art)
  - Spacing: 16×16
  - Constraint: **Fixed Column Count**
  - Constraint Count: **2**
- Add `ContentSizeFitter` (optional) if you want it to auto-size
  - Horizontal Fit: Preferred Size
  - Vertical Fit: Preferred Size

Drag `PlayerArea` into `Assets/_Project/Prefabs/UI/` to create prefab:
- Rename prefab `PF_PlayerArea`
- Delete the hierarchy instance (or keep one for layout tests)

### 4.3 Place player areas around the table
Under `TableRoot`:
- Place 4 anchors:
  - `P1_Bottom`, `P2_Left`, `P3_Top`, `P4_Right`

For each anchor:
1. Create empty RectTransform
2. Anchor to the edge (bottom/left/top/right)
3. Instantiate `PF_PlayerArea` as child

**Premium trick:** use consistent spacing and bigger margins than you think. Most “cheap UI” looks cheap because everything is cramped.

---

## 5) What we’re building: Card System (Data + View + Interaction)

We’ll split the card system into:
- **Data**: what the card *is* (name, type, value, effect)
- **View**: how it looks (front/back sprite, text)
- **Logic**: what happens when it’s drawn/swapped/revealed

### 5.1 Create CardData ScriptableObject
Create script `Assets/_Project/Scripts/Data/CardData.cs`

```csharp
using UnityEngine;

public enum CardKind { Number, Dragon, Special }

[CreateAssetMenu(menuName = "DragonsGambit/Card")]
public class CardData : ScriptableObject
{
    public string cardId;
    public string displayName;
    [TextArea] public string rulesText;

    public CardKind kind;
    public int value;

    public Sprite frontArt;
    public Sprite frameArt;   // optional overlay frame
}
```

Create card assets:
1. Right-click in `Assets/_Project/ScriptableObjects/Cards`
2. **Create → DragonsGambit → Card**
3. Name it `SO_Card_...`
4. Fill fields (drop in sprites later)

### 5.2 Create the Card prefab (PF_Card)
In Hierarchy:
1. **UI → Image**, rename `Card`
2. Add children:
   - `Front` (Image)
   - `Back` (Image)
   - `Name` (TMP)
   - `Value` (TMP)
   - Optional: `Frame` (Image) above art
3. Set `Back` visible by default, `Front` hidden (for facedown)

Add components to root:
- `Button` (or `IPointerClickHandler` script for more control)
- `CanvasGroup` (for fades)

Drag to `Assets/_Project/Prefabs/Gameplay/` as `PF_Card`.

### 5.3 Create CardView script
`Assets/_Project/Scripts/Gameplay/CardView.cs`

```csharp
using UnityEngine;
using UnityEngine.UI;
using TMPro;

public class CardView : MonoBehaviour
{
    [Header("Refs")]
    public Image frontImage;
    public Image backImage;
    public Image frameImage;
    public TMP_Text nameText;
    public TMP_Text valueText;

    public CardData Data { get; private set; }
    public bool IsFaceUp { get; private set; }

    public void Bind(CardData data, Sprite backSprite)
    {
        Data = data;
        nameText.text = data.displayName;
        valueText.text = data.value.ToString();

        frontImage.sprite = data.frontArt;
        if (frameImage != null) frameImage.sprite = data.frameArt;

        backImage.sprite = backSprite;

        SetFaceUp(false, instant:true);
    }

    public void SetFaceUp(bool faceUp, bool instant=false)
    {
        IsFaceUp = faceUp;

        // simple swap; later replace with flip animation
        frontImage.gameObject.SetActive(faceUp);
        nameText.gameObject.SetActive(faceUp);
        valueText.gameObject.SetActive(faceUp);
        if (frameImage != null) frameImage.gameObject.SetActive(faceUp);

        backImage.gameObject.SetActive(!faceUp);
    }
}
```

Wire references:
1. Select `PF_Card`
2. Add component `CardView`
3. Drag the child images/texts into fields

---

## 6) What we’re building: Dealing hands (2×2 facedown)

### 6.1 Create a simple Deck + Dealer
Create `Assets/_Project/Scripts/Gameplay/Deck.cs`

```csharp
using System.Collections.Generic;
using UnityEngine;

public class Deck
{
    private readonly List<CardData> _cards = new();
    private readonly System.Random _rng = new();

    public Deck(IEnumerable<CardData> cards)
    {
        _cards.AddRange(cards);
        Shuffle();
    }

    public void Shuffle()
    {
        for (int i = _cards.Count - 1; i > 0; i--)
        {
            int j = _rng.Next(i + 1);
            (_cards[i], _cards[j]) = (_cards[j], _cards[i]);
        }
    }

    public CardData Draw()
    {
        var c = _cards[^1];
        _cards.RemoveAt(_cards.Count - 1);
        return c;
    }
}
```

Create `GameController.cs` (minimal loop scaffolding):
```csharp
using System.Collections.Generic;
using UnityEngine;

public class GameController : MonoBehaviour
{
    [Header("Config")]
    public List<CardData> allCards;
    public Sprite cardBack;

    [Header("Refs")]
    public PlayerAreaView[] playerAreas;   // 2-4
    public CardView cardPrefab;

    private Deck _deck;

    void Start()
    {
        _deck = new Deck(allCards);
        DealInitialHands();
    }

    void DealInitialHands()
    {
        foreach (var p in playerAreas)
        {
            p.ClearHand();

            // 4 cards in a 2x2 grid
            for (int i = 0; i < 4; i++)
            {
                var data = _deck.Draw();
                var card = Instantiate(cardPrefab, p.handGrid);
                card.Bind(data, cardBack);
                card.SetFaceUp(false, instant:true);
                p.AddCard(card);
            }
        }
    }
}
```

### 6.2 Create PlayerAreaView
`Assets/_Project/Scripts/UI/PlayerAreaView.cs`

```csharp
using System.Collections.Generic;
using UnityEngine;
using TMPro;

public class PlayerAreaView : MonoBehaviour
{
    public TMP_Text nameText;
    public TMP_Text scoreText;
    public Transform handGrid;

    private readonly List<CardView> _hand = new();

    public void SetName(string n) => nameText.text = n;
    public void SetScore(int s) => scoreText.text = s.ToString();

    public void ClearHand()
    {
        _hand.Clear();
        for (int i = handGrid.childCount - 1; i >= 0; i--)
            Destroy(handGrid.GetChild(i).gameObject);
    }

    public void AddCard(CardView cv) => _hand.Add(cv);
    public IReadOnlyList<CardView> Hand => _hand;
}
```

Hook it up:
1. Put `GameController` on a `Game` empty object in `SC_Game`
2. Assign:
   - `allCards`: drag all `SO_Card_*` assets
   - `cardBack`: back sprite
   - `playerAreas`: drag each `PF_PlayerArea` instance (it must have PlayerAreaView)
   - `cardPrefab`: drag `PF_Card` (as prefab reference)

Now press Play: you should see hands dealt facedown.

---

## 7) What we’re building: Points, turn text, and “game feel” UI

### 7.1 TopBar (turn + scores)
Create `TopBar`:
- Left: `TurnIndicator` (TMP)
- Right: `RoundIndicator` (TMP)
- Optional: small avatar icons

**Premium typography rules**
- 1 display font for headers, 1 readable font for body.
- Keep line height generous.
- Use TMP **Font Asset** with SDF for crisp scaling. citeturn0search3turn0search19

### 7.2 Log panel (makes it feel premium)
Create `LogPanel`:
- Panel with scroll view
- Each log line is TMP text
- Log events: draw, swap, reveal, points change

This adds *clarity* (especially with hidden information) and makes the product feel “complete”.

---

## 8) Premium look & feel checklist (cheap but effective)

### 8.1 UI polish
- Use **9-sliced sprites** for panels/buttons:
  - In Sprite import settings: set **Sprite Mode: Single**
  - **Sprite Editor → Border**: define corners
  - On Image: set **Type: Sliced**
- Use a consistent spacing system:
  - 8px / 16px / 24px / 32px steps
- Add tiny animations:
  - Button hover scale 1.00 → 1.03
  - Panel fade-in 0.15–0.25 seconds
- Add UI SFX:
  - click, hover, card flip, draw, discard
  - even free SFX can sound premium if consistent and not too loud

### 8.2 Card “premium” rendering
- Keep text readable:
  - Use outlined text or shadow behind value in bright art
- Keep art safe margins:
  - 5–8% padding inside frame so nothing clips
- Use Sprite Atlases:
  - **Assets → Create → 2D → Sprite Atlas**
  - Add your card + UI sprites
  - Enables batching and reduces memory

### 8.3 Performance basics
- Avoid rebuilding layouts every frame.
- Don’t instantiate/destroy constantly in gameplay; later you can pool cards.
- If you add URP 2D lights: keep them minimal for mobile. (URP 2D lights can be expensive if abused.) citeturn0search9

---

## 9) AI-assisted asset creation (budget-friendly)

### 9.1 What to generate with AI
Good AI targets:
- **Card art** (front illustrations)
- **Backgrounds** (menu/table texture)
- **Iconography** (simple symbols)
Avoid AI for:
- Small, crisp UI text/icons unless you can generate **vector** or high-res and clean it.

### 9.2 Workflow: generate → clean → import
Tools (free/cheap):
- **GIMP** (raster cleanup)
- **Krita** (paint + cleanup)
- **Inkscape** (vector, icons, borders)

Pipeline:
1. Generate at **high resolution** (at least 2048×2048 for card art)
2. Clean artifacts:
   - remove weird edges
   - ensure consistent lighting/style
3. Export:
   - Card art: PNG (lossless)
   - UI frames: PNG with transparency (or SVG if you go UI Toolkit)
4. Import to Unity:
   - Texture Type: **Sprite (2D and UI)**
   - Filter Mode: **Bilinear** (UI icons sometimes point)
   - Compression: **High Quality** (or none for UI)

### 9.3 Build a consistent “art bible”
To look premium, your AI art must be consistent.
Create a small doc with:
- Color palette (5–8 colors)
- Lighting direction (e.g., top-left)
- Line style (painterly vs sharp)
- Frame style (gold filigree, stone, etc.)

Use the same prompt structure for every card:
- Subject + environment
- Mood
- Palette
- Camera angle
- “no text, no watermark, clean composition”

---

## 10) Next core mechanic milestones (after UI skeleton)
Once the UI is stable:
1. **Initial peek**: show 2 cards to the player temporarily
2. **Draw action**: draw from deck or discard
3. **Swap action**: replace a card in your grid
4. **Reveal/discard rules**: per your GameRules.md
5. **Scoring + end round**
6. **AI opponent**: basic heuristic

---

## Appendix A — “Where do I click?” cheat sheet
- Create Script: Project window → right-click folder → **Create → C# Script**
- Create Prefab: drag a GameObject from Hierarchy into Project window
- Add UI text: **GameObject → UI → Text - TextMeshPro**
- Add layout: select object → **Add Component → Vertical Layout Group / Grid Layout Group**
- Fix UI scaling: select Canvas → **Canvas Scaler** as described earlier
- Assign references: select object with script → drag from Hierarchy into script fields in Inspector

---

## Appendix B — Optional: UI Toolkit path
Unity’s UI Toolkit is evolving quickly in Unity 6.x (including big UI improvements, and Unity 6.3 LTS mentions native SVG support). citeturn0search14turn0search2turn0search22  
Unity’s “get started with runtime UI” shows the basic UIDocument flow. citeturn0search6  
If you want, I can rewrite this guide for **UI Toolkit**, but for a first game, I recommend shipping the first version with **UGUI + TMP**.

