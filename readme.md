```markdown
# Blooket — Recreated + Admin Panel

A complete, single-file recreation of the **Blooket** quiz-game experience — built with vanilla HTML, CSS, and JavaScript. Includes the full Blook database (218 collectible creatures), pack-opening animations, solo trivia games, deep stats tracking, and a hidden 26-power admin panel.

> **Disclaimer:** This is an educational fan recreation. All Blook artwork is served from Blooket's public CDN for demonstration purposes only. Not affiliated with or endorsed by Blooket.

---

## Table of Contents

- [Quick Start](#quick-start)
- [Features](#features)
- [Admin Panel](#admin-panel)
- [Project Structure](#project-structure)
- [How It Works](#how-it-works)
- [Data Model](#data-model)
- [Customization](#customization)
- [Routes](#routes)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Browser Support](#browser-support)
- [License](#license)

---

## Quick Start

1. **Download** the `index.html` file.
2. **Open it** in any modern browser (Chrome, Firefox, Edge, Safari).
3. That's it — no build step, no dependencies, no server required.

The save system uses `localStorage`, so progress persists across page reloads and browser restarts.

### First-time Setup

1. Click **Sign in** (top-right) or navigate to `#/login`.
2. Enter a display name and pick an avatar Blook.
3. You start with **500 tokens** — spend them in the Marketplace to open packs.

---

## Features

### 🎯 Solo Games
- **Classic Mode** — Answer questions against 3 AI bots. Score points for correct answers, speed, and streaks.
- **Gold Quest Mode** — Answer correctly to earn a chest pick: find gold, steal from rivals, or swap balances.
- **Dynamic question engine** — Arithmetic, percentages, sequences, and 40+ general-knowledge trivia questions.
- **Speed & streak bonuses** — Faster answers score more; consecutive correct answers stack multipliers.
- **Live scoreboard** — Watch bots answer in real time with difficulty-scaled accuracy and speed.

### 📦 Marketplace & Pack Opening
- **17 themed packs** — Farm Animal, Pet, Arctic, Forest, Wonderland, Breakfast, Space, Bot, Aquatic, Safari, Dino, Ice Monster, Outback, Blizzard, Spooky, Color, and Mystical.
- **Weighted rarity system** — Common, Uncommon, Rare, Epic, Legendary, Chroma, and Mystical.
- **Animated reveals** — Pack-shake animation, burst effect, and card reveal with rarity-specific glow.
- **Confetti celebrations** — Triggered for Rare+ pulls (extra for Legendary/Chroma/Mystical).
- **Duplicate refunds** — Duplicate blooks automatically refund tokens based on rarity.

### 📚 Blook Collection
- **218 unique Blooks** across 17 sets.
- **Filter & search** — By set, rarity, owned/missing status, or name.
- **Collection progress** — Per-set progress bars and overall completion percentage.
- **Detail modal** — View team name, color, drop rate, and duplicate value.

### 📊 Stats & Progression
- **XP & Leveling** — Every game grants XP. Level-ups award bonus tokens automatically.
- **Deep stats tracking** — Games, wins, accuracy, best streak, packs opened, tokens earned/spent.
- **Performance chart** — Last 10 games visualized as a bar chart.
- **Collection breakdown** — Per-set progress and rarity distribution.
- **Game history** — Last 30 games with mode, place, score, accuracy, and timestamp.

### 🎨 Design System
- **Full light/dark theme** — Persisted across sessions.
- **Nunito typography** — Clean, rounded, and highly legible.
- **Fully responsive** — Works on desktop, tablet, and mobile.
- **Smooth animations** — Reveal-ups, hover effects, confetti, and pop-in modals.

### 💾 Local Save System
- **Auto-saving** — Every change writes to `localStorage`.
- **Export/Import** — Download your save as JSON or restore from a backup.
- **Factory reset** — Wipe everything from the stats page or admin panel.

---

## Admin Panel

A hidden 26-power admin panel for testing and messing around.

### Access

**Three ways to open it:**

1. **Triple-click the logo badge** in the top-left corner of any page.
2. **Navigate directly** to `#/admin` in the URL bar.
3. **Mobile menu** — the "Admin" link at the bottom of the hamburger drawer.

### Password

```
6171399
```

The admin session is stored in `sessionStorage` and clears automatically when the browser tab is closed.

### All 26 Powers

| # | Power | Description |
|---|-------|-------------|
| 1 | **Unlock Blook** | Grant a specific blook by name (case-insensitive) |
| 2 | **Unlock All** | Add every one of the 218 blooks |
| 3 | **Open Packs** | Bulk-open N packs free (no animation, supports 100,000+) |
| 4 | **Unlock Set** | Grant every blook in a chosen set |
| 5 | **Unlock Rarity** | Grant every blook of a specific rarity |
| 6 | **Random N** | Grant N random missing blooks |
| 7 | **Remove Blook** | Delete a blook from your collection |
| 8 | **Set Count** | Set exact quantity of a specific blook |
| 9 | **Max Out** | Unlock all + 1M tokens + 1M XP |
| 10 | **Set Tokens** | Set token balance to an exact value |
| 11 | **Grant Tokens** | Add or subtract tokens |
| 12 | **Add XP** | Directly add XP |
| 13 | **Set Level** | Jump to any level (1–999) |
| 14 | **Reset Stats** | Wipe stats only (keeps collection) |
| 15 | **Fake Wins** | Add +50 wins to your stats |
| 16 | **Reset Daily** | Clear daily reward cooldown instantly |
| 17 | **Set Streak** | Set daily streak value |
| 18 | **Edit Profile** | Change name and avatar |
| 19 | **Export Save** | Download save data as JSON |
| 20 | **Import Save** | Upload a JSON save to restore |
| 21 | **Factory Reset** | Wipe absolutely everything |
| 22 | **Monte Carlo** | Simulate N rolls without mutating the save |
| 23 | **God Mode** | Always answer correctly in games |
| 24 | **Infinite Time** | Remove the timer in games |
| 25 | **Instant Win** | +500 tokens + win stat + 10 correct answers |
| 26 | **Fake History** | Generate N fake game history entries |

### Admin Panel Safety

- All destructive actions (reset, factory reset, clear collection) trigger a **confirmation modal**.
- The password gate uses `sessionStorage`, so closing the tab immediately revokes access.
- Cheat toggles (God Mode, Infinite Time) are **idempotent patches** — safe to toggle on and off.

---

## Project Structure

The entire application lives in a **single HTML file** for maximum portability:

```
index.html
├── <head>
│   └── <style> ................. Full design system (~700 lines)
├── <body>
│   ├── <nav> ................... Sticky top bar with tokens, theme, profile
│   ├── <main id="view"> ........ Route-rendered content
│   ├── <div id="modal-root"> ... Modal container
│   └── <div id="toast-root"> ... Toast notifications
└── <script>
    ├── Data Layer
    │   ├── BLOOKS .............. 218 blooks parsed from pipe-delimited string
    │   ├── PACKS ............... 17 packs with pricing and metadata
    │   └── RARITY_* ............ Rarity weights, rewards, and ordering
    ├── State ................... localStorage save/load
    ├── UI ...................... toast(), modal(), confirm(), confetti()
    ├── Admin ................... 26 admin powers
    ├── Market .................. Pack opening + reveal animation
    ├── Trivia .................. Question generators
    ├── Game ................... Solo game engine with bots
    ├── Auth .................... Local profile sign-in/out
    ├── Views ................... Route renderers (home, market, blooks, etc.)
    ├── Mounts .................. Event handler attachment per route
    └── App ..................... Router, theme, nav
```

---

## How It Works

### Rarity System

Each rarity has a **weight** used for weighted random selection:

| Rarity | Weight | Duplicate Reward |
|--------|--------|------------------|
| Common | 1000 | 5 tokens |
| Uncommon | 120 | 10 tokens |
| Rare | 25 | 25 tokens |
| Epic | 6 | 50 tokens |
| Legendary | 1.2 | 100 tokens |
| Chroma | 0.08 | 250 tokens |
| Mystical | 0.02 | 500 tokens |

Weights are divided by the count of blooks in each rarity within a pack, so drop rates stay balanced even when a pack has few Legendaries.

### Pack Opening Flow

```
User clicks pack → pay tokens → roll N blooks → animate reveal per blook
                                  ↓
                       new blook? → add to collection
                       duplicate? → refund tokens
```

### Game Flow

```
Start → generate question → bots schedule answers → user answers
                                    ↓
                        correct? → score + speed + streak bonus
                        wrong?   → reveal answer, reset streak
                                    ↓
                        next question → repeat → finish → rewards
```

### Question Engine

Four generators run in rotation:

1. **Arithmetic** — Addition, subtraction, multiplication, division, squares
2. **Percentages** — "What is 25% of 200?"
3. **Sequences** — "What comes next? 3, 7, 11, 15, ___"
4. **Trivia** — 40+ hardcoded general knowledge questions

All questions have 4 multiple-choice options with one correct answer.

---

## Data Model

The save lives in `localStorage` under the key `blooket-recreation-v2`:

```json
{
  "user": {
    "name": "BlookMaster",
    "avatar": "Chick",
    "joined": 1700000000000
  },
  "tokens": 1250,
  "xp": 4820,
  "owned": {
    "Chick": 3,
    "Penguin": 1,
    "Ghost": 2
  },
  "stats": {
    "games": 42,
    "wins": 18,
    "correct": 320,
    "wrong": 84,
    "packsOpened": 156,
    "bestStreak": 14,
    "tokensEarned": 8400,
    "tokensSpent": 7100,
    "blooksFound": 87
  },
  "lastDaily": 1700000000000,
  "dailyStreak": 3,
  "theme": "dark",
  "history": [],
  "createdAt": 1700000000000
}
```

---

## Customization

### Adjust Pack Prices

Edit the `PACKS` array:

```js
const PACKS = [
  { id:"Farm Animal", name:"Farm Animal", price:10, icon:"Chick", color:"#4ab96d", desc:"..." },
  // ...
];
```

### Tune Rarity Drop Rates

Edit `RARITY_WEIGHT`:

```js
const RARITY_WEIGHT = {
  Common: 1000,
  Uncommon: 120,
  Rare: 25,
  Epic: 6,
  Legendary: 1.2,
  Chroma: 0.08,
  Mystical: 0.02
};
```

Lower weight = rarer. Multiply all by the same factor to keep proportions but change overall generosity (though only ratios matter).

### Add New Blooks

Append a line to `RAW_BLOOKS` in pipe-delimited format:

```
Name|s3Path|mediaId|set|realSet|rarity|teamName|color
```

Example:
```
Dragon|mythical/dragon.svg|v1/Blooks/dragon.svg|Mythical||Legendary|Dragon Squad|#ff0000
```

### Change Starting Tokens

Edit `DEFAULT_STATE`:

```js
const DEFAULT_STATE = {
  tokens: 500, // ← change this
  // ...
};
```

### Change Daily Reward Amount

Find the daily reward logic in `Mounts.dashboard()`:

```js
const bonus = 75 + Math.min(State.data.dailyStreak, 7) * 15;
```

### Change Question Types

Each generator is a standalone function:

```js
function makeArithmetic() { /* ... */ }
function makePercentage() { /* ... */ }
function makeSequence()   { /* ... */ }
function makeTrivia()     { /* ... */ }
```

Add or remove them from the `generateQuestion()` pool:

```js
function generateQuestion(){
  return pick([makeArithmetic, makeArithmetic, makeArithmetic, makePercentage, makeSequence, makeTrivia])();
}
```

---

## Routes

| Hash | View | Description |
|------|------|-------------|
| `#/` | Home | Landing page with hero and features |
| `#/login` | Login | Create or edit local profile |
| `#/dashboard` | Dashboard | Stats overview, recent games, daily reward |
| `#/market` | Market | Pack marketplace with odds previews |
| `#/blooks` | Blooks | Full collection browser with filters |
| `#/play` | Play | Game mode selection and setup |
| `#/stats` | Stats | Deep statistics and account info |
| `#/admin` | Admin | 26-power admin panel (password required) |

The router listens to `hashchange`, so navigating with browser back/forward works.

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Enter` in login name field | Submit profile creation |
| `Enter` in admin password field | Submit admin login |
| `Enter` in blook name field | Trigger unlock |
| `Esc` (implicit) | Click backdrop to close modals |

---

## Browser Support

Tested and working on:

- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Edge 90+
- ✅ Safari 14+
- ✅ Mobile Chrome / Safari

Required features:
- `localStorage` (progress saving)
- `sessionStorage` (admin session)
- CSS custom properties (theming)
- `color-mix()` (some Firefox versions may fall back gracefully)

---

## How to Reset Everything

1. Open the browser DevTools console (`F12`).
2. Run: `localStorage.removeItem('blooket-recreation-v2'); location.reload();`

Or from the app:

- **Stats page** → "Reset All Progress" button
- **Admin panel** → "Factory Reset Everything" button

---

## License

This is a **fan-made educational recreation**. The Blooket name, artwork, and branding are property of their respective owners. Do not use this for commercial purposes.

You are free to:
- ✅ Modify and learn from the code
- ✅ Use it in personal projects
- ✅ Share it with friends

You may not:
- ❌ Sell it or claim it as your own
- ❌ Use Blooket's assets for commercial gain
- ❌ Imply official affiliation with Blooket

---

## Changelog

### v2.0 — Admin Edition
- ✨ Added 26-power admin panel (password `6171399`)
- ✨ Triple-click logo secret entrance
- ✨ God Mode & Infinite Time cheats
- ✨ Bulk pack opening (up to 100,000 packs)
- ✨ Monte Carlo pack simulation
- ✨ Export/import save system
- ✨ Unlock by set or rarity
- ✨ Profile editor
- 🐛 Fixed pack art not showing for some packs
- 🎨 Improved mobile responsiveness

### v1.0 — Initial Release
- 🎉 Full Blook database (218 entries)
- 🎉 17 themed packs
- 🎉 Classic & Gold Quest game modes
- 🎉 Pack opening with reveal animations
- 🎉 Deep stats tracking
- 🎉 Light/dark themes

---

## Credits

- **Blook assets** — Blooket CDN (public endpoints)
- **Design inspiration** — Blooket's official UI
- **Font** — [Nunito](https://fonts.google.com/specimen/Nunito) via system fallback
- **Built with** — Pure HTML, CSS, and JavaScript (no frameworks)

---

## Contributing

This is a single-file project by design. If you want to extend it:

1. Fork or copy the file.
2. Make your changes in the relevant section (marked by comments like `/* ADMIN */` and `/* GAME */`).
3. Test all routes and features.
4. Share your version.

Ideas for extensions:
- Multiplayer via WebRTC or WebSocket
- More question categories (vocabulary, science, history)
- Blook trading system
- Seasonal events with limited-time packs
- Sound effects and background music
- Achievements / badges system

---

**Enjoy the recreation! 🎮**
```
