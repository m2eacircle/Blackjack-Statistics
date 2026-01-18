# Split Button Testing Guide

## What Split Should Do

When you click SPLIT with a pair (two cards of same value):

### Step 1: Click SPLIT
- **Cost:** 5 coins deducted immediately
- **Result:** Two separate hands created
- **Each hand:** Gets ONE additional card from shoe

### Step 2: Visual Display
You should see:
```
Playing Hand 1 of 2  ← Yellow indicator

[Card1] [NewCard]  |  [Card2] [NewCard]
    Hand 1         |      Hand 2
   (Active)        |    (Dimmed)
```

### Step 3: Play Sequence
1. Play Hand 1 completely (HIT/STAND/DOUBLE)
2. Automatically switch to Hand 2
3. Play Hand 2 completely
4. Both hands resolved against dealer

## Testing Instructions

### Open Browser Console
1. Press **F12** (or Cmd+Option+I on Mac)
2. Click **Console** tab
3. Keep it open during testing

### Start a Game
1. Accept Terms
2. Select Regular mode
3. Click Start Game
4. Click Place Bets (5 coins)

### Get a Pair
You need two cards of same value:
- 2-2, 3-3, 4-4, 5-5, 6-6, 7-7, 8-8, 9-9, 10-10, J-J, Q-Q, K-K, A-A
- Face cards (J, Q, K) all count as 10

**Note:** You might need to play several hands to get a pair

### Click SPLIT

**What to check in console:**
```
SPLIT clicked! Player: {...}
Can split? true true true true
Splitting: {suit: "♠", display: "8", value: 8} and {suit: "♥", display: "8", value: 8}
After split:
Hand 1: [{suit: "♠", display: "8", value: 8}, {suit: "♣", display: "3", value: 3}]
Hand 2 (split): [{suit: "♥", display: "8", value: 8}, {suit: "♦", display: "7", value: 7}]
Playing split? false
```

**What to see on screen:**
1. **Indicator:** "Playing Hand 1 of 2" in yellow
2. **Cards:** First hand normal, second hand dimmed
3. **Divider:** Golden vertical line between hands
4. **Totals:** "Hand 1: 11 | Hand 2: 15"
5. **Buttons:** HIT, STAND, DOUBLE (based on Hand 1)

### Play First Hand
Click HIT, STAND, or DOUBLE

**If you STAND or DOUBLE:**
- Screen updates to show Hand 2 active
- Indicator changes to "Playing Hand 2 of 2"
- First hand dims, second hand normal
- Buttons show probabilities for Hand 2

**If you HIT and BUST:**
- Automatically switches to Hand 2
- Same visual updates as above

### Play Second Hand
Click HIT, STAND, or DOUBLE

**When done:**
- Moves to next player (or dealer if last player)
- Both hands will be resolved against dealer

## Expected Results

### Successful Split Example

**Before Split:**
```
Player 1: 🪙 100
Cards: 8♠ 8♥
Total: 16

[SPLIT 62%]  ← Button available
```

**After Clicking SPLIT:**
```
Player 1: 🪙 95  ← 5 coins deducted
Bet: 10 (5 per hand)

Playing Hand 1 of 2  ← Indicator

8♠ 3♣  |  8♥ 7♦
  ↑        ↓
Active   Dimmed

Hand 1: 11 | Hand 2: 15

[HIT 65%] [STAND 45%] [DOUBLE 70%]
```

**After Playing Hand 1 (let's say you DOUBLE):**
```
Player 1: 🪙 90  ← Another 5 deducted for double
Bet: 15 (10 + 5 double)

Playing Hand 2 of 2  ← Changed!

8♠ 3♣ K♦  |  8♥ 7♦
   ↓           ↑
 Dimmed      Active

Hand 1: 21 | Hand 2: 15

[HIT 28%] [STAND 35%]
```

## Troubleshooting

### Issue 1: SPLIT Button Doesn't Appear

**Causes:**
- Cards aren't a pair (check values)
- Not enough coins (need 5+ coins)
- Already split (can only split once)

**Check console for:**
```
Can split? [should be all true]
```

### Issue 2: Cards Don't Show After Split

**What to check:**
1. Console should show: `Hand 1: [...]` and `Hand 2 (split): [...]`
2. Both arrays should have 2 cards each
3. If empty, shoe might be out of cards

**Look for errors in console**

### Issue 3: Can't See Second Hand

**What to check:**
1. Scroll down - screen might need scrolling
2. Check if `player.splitHand` exists in console
3. Look for the golden divider between hands

### Issue 4: Indicator Not Showing

**Should see:**
- "Playing Hand 1 of 2" (yellow text, centered)
- Changes to "Playing Hand 2 of 2" when switching

**If missing:**
- Check if `player.splitHand` is not null

### Issue 5: Cards Not Switching

**After STAND/DOUBLE on Hand 1:**
- Should automatically switch to Hand 2
- Indicator should change
- Active/dimmed should swap

**Check console:**
```
Playing split? false  ← Should change to true
```

## Common Scenarios

### Scenario 1: Split Aces
```
Before: A♠ A♥ (Total: 12 or 22)
After Split: 
  Hand 1: A♠ + K♦ = 21
  Hand 2: A♥ + 5♣ = 16
Result: One blackjack, one weak hand
```

### Scenario 2: Split 8s vs 10
```
Before: 8♠ 8♥ vs Dealer 10
After Split:
  Hand 1: 8♠ + 3♣ = 11 (HIT) + 7♦ = 18
  Hand 2: 8♥ + 9♦ = 17 (STAND)
Dealer: 10 + 8 = 18
Result: Hand 1 ties, Hand 2 loses
```

### Scenario 3: Split 10s (Not Recommended)
```
Before: K♠ Q♥ = 20 (should STAND!)
If you split:
  Hand 1: K♠ + 5♣ = 15
  Hand 2: Q♥ + 6♦ = 16
Result: Turned 20 into two bad hands!
```

## What Success Looks Like

✅ SPLIT button appears with pair
✅ Console shows split execution
✅ Coins deducted (5 for split)
✅ Two hands visible on screen
✅ Golden divider between hands
✅ "Playing Hand 1 of 2" appears
✅ First hand active, second dimmed
✅ Probabilities shown for active hand
✅ Can play first hand (HIT/STAND/DOUBLE)
✅ Switches to second hand automatically
✅ "Playing Hand 2 of 2" appears
✅ Second hand active, first dimmed
✅ Can play second hand
✅ Both hands resolved correctly

## Report Back

Please test and tell me:

1. **Does SPLIT button appear?** (with a pair)
2. **What does console show when clicked?**
3. **Do you see both hands on screen?**
4. **Does the indicator show "Playing Hand 1 of 2"?**
5. **Are the cards visible with divider?**
6. **Do probabilities update for active hand?**
7. **Does it switch to Hand 2 after completing Hand 1?**

This information will help me identify exactly where the issue is!
