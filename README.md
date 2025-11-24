# Karmic Dice Roller (Stealth Edition)

A web-based tabletop dice roller featuring a hidden **"Invisible Hand" algorithm**. To the player, it looks like a standard RNG tool. Under the hood, it tracks roll history and subtly intervenes to prevent frustration and long streaks of bad luck.

## 🎲 Features

  * **Stealth UI:** Clean, dark-mode interface with no "Success/Fail" color coding. It looks like a neutral tool.
  * **Multi-Dice Support:** Roll anywhere from 1 to 10 dice at once.
  * **Visual Cues for the GM:** When the "Luck Debt" gets dangerously high (80%+ chance of intervention), the dice buttons subtly glow/pulse. This is a signal to the operator that the system is "primed" to save the next roll.
  * **Responsive Design:** Works on desktop and mobile.

## ⚙️ The "Invisible Hand" Logic

The core feature of this app is the **Karmic Engine**. It operates continuously in the background using a variable called `luckDebt`.

### 1\. Defining a "Good" Roll

The system tracks success based on "Heroic" standards, not just mathematical averages:

  * **d20:** A result of **13 or higher** is Good.
  * **Other Dice:** A result in the **upper 50%** (e.g., 4, 5, 6 on a d6) is Good.

### 2\. Debt Accumulation

  * **Bad Roll:** If a player rolls poorly, `luckDebt` increases by **+1**.
  * **Natural Good Roll:** If a player rolls well naturally, `luckDebt` decreases by **-1** (Soft Decay).

### 3\. Intervention (The Nudge)

Before displaying a result, the system calculates an **Intervention Chance**:
$$Chance = luckDebt \times 10\%$$

  * If the intervention triggers, the system secretly **re-rolls** the die in milliseconds.
  * If the re-roll is "Good," it keeps it and reduces `luckDebt` by **-2** (The "cost" of being saved).
  * If the re-roll is also "Bad," the system accepts fate, shows the bad number, and increases debt further.

### 4\. The "Charged" State

When `luckDebt` reaches **8** (an 80% intervention chance), the UI buttons trigger a CSS pulsing animation (`.charged` class). This indicates the system is heavily weighted to force a good outcome on the next click.

## 🚀 How to Run

1.  Download `dice.html`.
2.  Open the file in any modern web browser (Chrome, Firefox, Safari, Edge).
3.  **Developer Mode:** Press `F12` (or Right Click -\> Inspect) and open the **Console** tab. You will see real-time logs showing:
      * Raw rolls vs. Final rolls.
      * Current Luck Debt.
      * When the system secretly intervenes ("SAVED" vs "Natural Good").

## 🛠️ Configuration

You can tweak the logic by editing the `<script>` section in `index.html`.

**Change the "Hero" Threshold:**
Modify the `isGoodRoll` function:

```javascript
function isGoodRoll(val, sides) {
    if (sides === 20) return val >= 13; // Change 13 to 11 for standard math
    return val > (sides / 2);
}
```

**Change the Intervention Rate:**
Modify the `interventionChance` variable inside `getSingleRoll`:

```javascript
// Currently set to 0.10 (10%)
let interventionChance = luckDebt * 0.10; 
```

## 📝 License

This project is open source. Feel free to modify the luck algorithms to suit your specific tabletop group's needs\!
