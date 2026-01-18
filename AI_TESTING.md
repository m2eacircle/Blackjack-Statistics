# AI Debugging Instructions

## What I Changed

I completely rewrote the AI logic with:
1. **Simpler state management** - Direct state updates, no nesting
2. **Console logging** - See what AI is doing in browser console
3. **Clear function flow** - `executeAIAction(aiIndex)` does everything

## How to Test

### Step 1: Open Browser Console
1. Open the app in browser
2. Press `F12` (or `Cmd+Option+I` on Mac)
3. Click "Console" tab
4. Keep it open while playing

### Step 2: Start a Game
1. Accept Terms
2. Select "Regular" mode
3. Keep default setup (Player 1 human, AI 1, AI 2)
4. Click "Start Game"
5. Click "Place Bets"

### Step 3: Watch Console
You should see logs like:
```
AI 1 decision: hit hand: [{...}, {...}]
AI 1 decision: stand hand: [{...}, {...}, {...}]
AI 2 decision: hit hand: [{...}, {...}]
```

### Step 4: What to Look For

**If you see logs:**
- ✅ AI function is being called
- ✅ Decision logic works
- Problem is likely in state updates

**If NO logs appear:**
- ❌ `executeAIAction` not being called
- Check if `moveToNextPlayer` triggers correctly

## Common Issues & Fixes

### Issue 1: AI Never Starts
**Symptom:** No console logs, AI just sits there

**Check:**
- Is Player 1 human? (AI won't start if Player 1 is AI and trigger fails)
- Did you click "Place Bets"?

**Fix:** Set Player 1 to AI in setup, see if it triggers after 1.5 seconds

### Issue 2: AI Starts But Doesn't Continue
**Symptom:** See one log, then stops

**Problem:** Recursive call not working

**Check console for:**
```
AI 0 decision: hit hand: [...]
AI check failed: {...}  // <-- This means second call failed
```

### Issue 3: State Not Updating
**Symptom:** Logs show decisions but cards don't appear

**Problem:** React state updates failing

**Check:**
- Are `setPlayers` and `setShoe` being called?
- Is component re-rendering?

## Manual Test

If AI still doesn't work, try this manual test:

1. Open browser console
2. After game starts, type:
```javascript
// Check if function exists
console.log(typeof window.executeAIAction)

// Try calling it manually (if Player 1 is AI)
// This won't work directly but shows if function is accessible
```

## What Should Happen

**Correct Flow:**
```
1. Cards dealt
2. [1.5 sec delay]
3. executeAIAction(0) called for Player 1 (if AI)
4. Console: "AI 0 decision: hit hand: [...]"
5. Card added to AI hand
6. [1.5 sec delay]
7. executeAIAction(0) called again
8. Console: "AI 0 decision: stand hand: [...]"
9. [1 sec delay]
10. moveToNextPlayer() called
11. executeAIAction(1) called for AI 1
12. ... continues
```

## If Still Not Working

Please check console and tell me:
1. Do you see ANY console logs?
2. What does the last log say?
3. Does AI thinking box appear?
4. Does game freeze or continue?

This will help me identify exactly where it's failing!

## Quick Fix to Try

If nothing works, try changing Player 1 to AI:
1. Go to Setup screen
2. Click the 👤 icon next to "Player 1"
3. It should change to 🤖 and show "AI 1"
4. Start game
5. Watch if AI plays automatically

If it works with AI as Player 1 but not AI 2/3, then the issue is in `moveToNextPlayer` transition logic.
