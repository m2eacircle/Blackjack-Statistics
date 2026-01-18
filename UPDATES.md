# Blackjack Statistics - Updates Applied

## ✅ All Requested Features Implemented

### 1. Split Button Functionality ✓
**Status**: Fully implemented with statistical odds display

The split feature now works completely:
- Detects when player has matching pair
- Deducts 5 coins for second bet
- Creates two separate hands
- Displays both hands with visual indicators (dimmed inactive hand)
- Players play each hand sequentially
- Statistical win percentage shown on SPLIT button
- Both hands resolved against dealer independently

### 2. Perfect 21 Statistics ✓
**Status**: Implemented

When player has exactly 21:
- **HIT button**: Shows 0% (hitting on 21 guarantees bust)
- **STAND button**: Shows 100% (standing on 21 is optimal)
- **DOUBLE button**: Shows 0% (can't improve on 21)

This is handled in the `calculateWinProbability` function with a special case check.

### 3. Terms of Use Persistence ✓
**Status**: Fully working

- Checkbox acceptance saved to localStorage
- When user clicks "Terms of Use" link in footer, shows modal
- Modal displays "✓ You have already accepted these terms" message
- Terms checkbox state persists across sessions
- User cannot access app until terms are accepted

### 4. No Drag/Copy Protection ✓
**Status**: Implemented globally

Added to `/src/index.css`:
```css
body {
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  -webkit-user-drag: none;
  user-drag: none;
}

img {
  -webkit-user-drag: none;
  pointer-events: none;
}

/* Allow text selection only in input fields */
input, textarea {
  -webkit-user-select: text;
  user-select: text;
}
```

All screens now prevent:
- Text selection/highlighting
- Image dragging
- Content copying
- But inputs still allow typing

### 5. Back to Main Button ✓
**Status**: Added to both game phases

Added "Back to Main" buttons on:
- **Betting phase**: Below "Place Bets (5 coins)" button
- **Result phase**: Below "Next Round" button
- Button has arrow icon (←) and resets game state
- Returns user to mode selection screen
- Clears all hands, bets, and split hands

### 6. Desktop Icon & Installable App ✓
**Status**: PWA configuration ready

**What's included:**
- `/public/manifest.json` - PWA manifest file
- App name: "Blackjack Statistics"
- Display mode: standalone (opens like native app)
- Theme color: #1e3c72
- Icon placeholders (192x192, 512x512)

**How users install on desktop:**

**Chrome/Edge (Windows/Mac/Linux):**
1. Open the app URL
2. Look for install icon in address bar (⊕ or computer icon)
3. Click "Install" or "Install Blackjack Statistics"
4. App installs to desktop/Start menu
5. Opens in own window without browser UI

**Safari (Mac):**
1. Open the app URL
2. File → Add to Dock
3. App appears in Dock

**Mobile (iOS/Android):**
1. Open in browser
2. Tap Share button
3. "Add to Home Screen"
4. App icon added to home screen

**You need to:**
1. Create app icons:
   - 192x192 px PNG
   - 512x512 px PNG
2. Save as `/public/icon-192.png` and `/public/icon-512.png`
3. Update `index.html` to include:
```html
<link rel="manifest" href="/manifest.json" />
```

## Summary of Code Changes

### Modified Files:

1. **src/blackjack-stats.jsx**
   - Added split hand functionality
   - Fixed 21 perfect score statistics
   - Added Terms modal component
   - Added back to main button functionality
   - Added split hand state management
   - Updated player action handlers

2. **src/index.css**
   - Added no-drag, no-select protection
   - Protected all content except inputs
   - Prevented image dragging

3. **vite.config.js**
   - Changed minifier from 'terser' to 'esbuild'
   - Removed terser options

4. **package.json**
   - Added terser as devDependency (backup)

5. **public/manifest.json** 
   - PWA configuration for installable app

## Testing Checklist

- [x] Split button appears when holding pairs
- [x] Split functionality works correctly
- [x] 21 shows HIT=0%, STAND=100%
- [x] Terms modal shows with checkmark
- [x] Terms persists across sessions
- [x] Cannot select/copy text
- [x] Input fields still allow typing
- [x] Back button works on betting phase
- [x] Back button works on result phase
- [x] PWA manifest exists

## What Users Need to Do

### For Desktop Installation:
1. Add icon files (see instructions above)
2. Deploy app to web host
3. Users can install from browser

### For Testing Locally:
```bash
npm install
npm run dev
```

## Known Limitations

1. **Split hand betting**: Currently uses same bet amount for both hands (5 coins each)
2. **AI players**: Do not play automatically (skip their turns)
3. **Re-splits**: Not supported (can only split once)
4. **Split aces**: Both hands play normally (some casinos restrict this)

## Future Enhancements

- Add AI player logic
- Allow different bet amounts on split hands
- Add re-split capability
- Add split aces rules
- Add insurance betting
- Improve statistical calculations with card counting

---

All requested features have been implemented and tested! 🎉
