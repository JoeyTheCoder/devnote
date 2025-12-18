# The Dragon’s Gambit — Game Logic Breakdown

This file describes the detailed game loop and control flow for implementing The Dragon’s Gambit in code. It covers setup, turn structure, card effects, scoring, and round/game progression according to GameRules.md.

---

## 1. Game Initialization

1. **Create and Shuffle Deck**
   - Instantiate cards as per the defined composition (see GameRules.md)
   - Implement card values, identities, and effect hooks
   - Shuffle deck randomly
2. **Deal Hands**
   - Deal 4 cards facedown to each player (arranged as 2x2 grid per hand)
3. **Initial Player Peek**
   - For each player:
      - Prompt to look at exactly 2 cards (e.g., first two by position)
      - Show cards temporarily, then hide
      - Store cards as hidden from the player unless revealed
4. **Set Up Discard and Draw Pile**
   - Place remaining deck as facedown draw pile
   - Flip top card face-up for discard pile

---

## 2. Game Loop: Player Turn

Repeat until a player calls "Final Gambit"

### Each Player’s Turn:
  1. **Present Options**
     - Draw from draw pile
     - Take top discard card
     - (If conditions met) Call "Final Gambit"

  2. **Player Makes Choice**
     - **Draw from Draw Pile:**
        - Reveal to only that player
        - Player chooses:
            - Swap with one card from own hand • Swapped card goes face-up to discard
            - Discard drawn card face-up (no hand change)
     - **Take Top Discard:**
        - Must swap with one of own hand cards
        - Swapped card goes to top of discard pile, face-up
     - **Call Final Gambit:**
        - If believed to have lowest hand, ends game loop after all other players get one more turn

  3. **Trigger Card Abilities (if applicable)**
     - If card drawn or discarded has an effect (see GameRules.md):
         - Immediately apply effect
         - For choices (peek, swap, etc.), prompt player as needed
     - Enforce numeric value caps (0–13) on all modifications

  4. **Advance Turn**
     - Proceed to next player in turn order
     - If Final Gambit triggered, give each remaining player one last turn then advance to scoring

---

## 3. End of Round & Scoring

1. **Reveal Hands**
   - Show all player hands and card values
2. **Calculate Scores**
   - Sum each hand’s value
   - Apply penalty if Final Gambit caller does not have the lowest total
3. **Record Round Results**
   - Add penalties if needed
   - Store round scores for each player

---

## 4. Multi-Round/Game End

1. **Check Game End Condition**
   - If any player’s total cumulative score >= threshold (e.g., 100), end game
2. **Declare Winner**
   - Player with lowest total score wins
   - Handle tiebreaks as needed

---

## 5. Edge Cases and Rule Handling

- Deck depletion: If draw pile is empty, reshuffle discard pile (except top card) to recreate draw pile.
- All card value modifications are clamped between 0 and 13 (inclusive).
- Card ability effects trigger immediately when drawn, unless specified otherwise in rules.
- All swaps, peeks, and effect prompts must be clearly presented and confirmable by the UI.
- Ensure all player information/hand visibility is correctly restricted based on game state.

---

## Pseudocode Overview (for reference)

```
StartGame()
  shuffleDeck()
  dealHands()
  playerInitialPeek()
  topCardToDiscard()
  currentPlayer = determineFirstPlayer()
  while not FinalGambitCalled:
      playTurn(currentPlayer)
      checkIfFinalGambit()
      currentPlayer = nextPlayer()

  for player in remainingPlayers:
      playTurn(player)

  revealHands()
  computeScores()
  updateCumulativeScores()
  checkGameEnd()
  if not gameOver:
      startNextRound()
```

---

_This file is intended as guidance for code/logic structure. Adjust for platform-specific requirements, multiplayer, or AI integration as needed._
