# Infinite Luck Casino - Demo Version
This is an official Kitboga Codejam26 entry version of the Infinite Luck Casino minigame ads. The github.io page of this version uses the default index.html provided by the KitBoga team in their template (https://github.com/The-Kitboga-Show/codejam26) This is just a preview of how the final submission works within the template. The better preview of the entire project can be found in demo.

git.io entry version: https://gm-doc.github.io/Infinite-Luck-Casino-Codejam26/ <br>
The git.io page of the demo version: https://gm-doc.github.io/Infinite-Luck-Casino/ <br>
The repo with the demo version: [ https://github.com/gm-doc/Infinite-Luck-Casino-Codejam26](https://github.com/gm-doc/Infinite-Luck-Casino-Codejam26)

## Table of Contents
- [Core Skip Mechanics](#core-skip-mechanics)
- [Ad Elements Breakdown](#ad-elements-breakdown)
  - [Video Ad](#video-ad)
  - [Prize Wheel](#prize-wheel)
  - [Slot Machine](#slot-machine)
  - [Scratch Cards](#scratch-cards)
  - [Prize List](#prize-list)
  - [Ticket Counter](#ticket-counter)
  - [Live Winners](#live-winners)
  - [Idling Protection](#idling-protection)
  - [Dog](#dog)
  - [AWP Dragon Lore Skin](#awp-dragon-lore-skin)
  - [Bitcoin](#bitcoin)
  - [Abandon Prizes Confirmation](#abandon-prizes-confirmation)
  - [Out of Tickets Popup](#out-of-tickets-popup)
- [Reward Tiers](#reward-tiers)
  - [Tier 1 — Standard](#tier-1--standard-️)
  - [Tier 2 — Gold](#tier-2--gold-)
  - [Tier 3 — Diamond](#tier-3--diamond-)
- [Easter Eggs](#easter-eggs)
- [Additional Arguments](#additional-arguments)

---

This GitHub repository contains a contest entry version of the Infinite Luck Casino mini game ads. The minigame is designed to waste as much of your time as possible while forcing you to play the casino themed games. Every minigame is a different style casino lottery with multiple rewards and 3 reward tiers (standard, gold, diamond). The rewards and tiers are the same between the games. The UI contains multiple rigged mechanics that will make it impossible to not engage or skip the game despite what the  rewards might promise.

## Core Skip Mechanics
The ad was designed to resemble the annoying mobile game ads that let you play a simple mini game before skipping. This idea is enhance by making it very hard to skip but also engaging and getting your hopes up on purpose. It implements the following skip related mechanics:
- Long initial skip ad time (360s default)
- Rewards that reduce the timer are only bait (you will win -30s and -60s during a few initial spins but after that its impossible to get any more time reduction prizes)
- Ticket Drain Mode - Once the skip ad timer runs out the ticket drain mode will force you to use all remaining tickets (it will be possible but only in this mode - this is endgame) 
- Collect Prizes! - The game tells you that prizes can only be claimed once your tickets run out. You will not run out — ticket prizes (+5 and +10) are completely suppressed from the prize pool while you have more than 3 tickets, but the moment your supply drops to 3 or below they become available again and will refill you. In drain mode ticket prizes are suppressed entirely regardless of count, making it the only state where running out is actually possible.
- Idle check - The game checks if you're idling and will penalize you with more time added to skip timer
- Minigames - Some prizes (dog, awp, btc) have a special minigame you have to play before collecting them. Those minigames pause the skip ad timer for the duration
- Safety net - The game has a built in safety net so it can't take forever. If the gameplay time exceeds 10 minutes the skip ad button will become available unconditionally


## Ad Elements Breakdown
This ad interface is quite complex and consists of multiple minigames, overlays and pop-ups. The main elements are 3 minigame UIs (prize wheel, slot machine, scratch cards), widgets displayed around each minigame and special prizes that have separate minigames related to them implemented within the special pop-up window.

Below is the in-depth breakdown of UI elements implemented by this ad with examples:

### Video Ad
A short (~15s) video advertisment of the Infinite Luck Casino is played before showing a random minigame. It's deliberately cheap using AI voiceover and a lot of unneccessary effects.

### Prize Wheel
<img width="292" height="278" alt="image" src="https://github.com/user-attachments/assets/dd17dcb0-1f1f-4dc9-ab1b-bb684ac963d4" />
Spin the wheel to win a prize - it's slower then the other minigames but guarantees a reward with each spin.

### Slot Machine
<img width="292" height="293" alt="image" src="https://github.com/user-attachments/assets/c2a2dc78-a177-4e47-8caa-8b17ed07904c" />
Spin the slots using tickets to win the prizes - faster then the wheel but does not always land on the reward.

### Scratch Cards
<img width="293" height="314" alt="image" src="https://github.com/user-attachments/assets/b6525a24-876b-4de8-8601-c89023582c44" />
Scratch the card with your mouse (click + drag) or your finger (on touch screen). If the rewards underneath allign you win the prize!

### Prize List
<img width="122" height="144" alt="image" src="https://github.com/user-attachments/assets/d0adae79-40fb-46d0-aa74-4e1791e828ae" />
A simple widget that displays all the collected prizes - Collecting them will show a pop-up that explain you should keep playing at least until your tickets run out before redeeming your prizes. You are way to lucky for your tickets to run out :) 

### Ticket Counter
<img width="119" height="137" alt="image" src="https://github.com/user-attachments/assets/4e276a28-6e4e-41ac-8858-c287975403fd" />
A simple overlay showing your remaining tickets. Don't worry - If you are running low the game will make sure you get some more.

### Live Winners
<img width="119" height="86" alt="image" src="https://github.com/user-attachments/assets/7fa6abfb-49c9-4258-8c7e-fb90c65a9468" />
An overlay that aims to irritate you with the fake live winners and their comments. Some of them are randomized to seem more belivable, others are forced to use some inside jokes (~1 in 5)

### Idling Protection
<img width="266" height="258" alt="image" src="https://github.com/user-attachments/assets/97986b1c-9096-461f-a9cc-dc56f4553b1e" />
If you haven't been playing the game for more then 35 seconds (spinning the machine/wheel or scratching the cards) the game will get increasingly engry with you and will punish you with additional time added to your skip button. This pop-up has multiple tiers and the most severe punishment is +300s.

### Dog
<img width="246" height="285" alt="Dog" src="https://github.com/user-attachments/assets/d6045749-debc-40f1-8611-316d4ccd2151" />
A dog is a special friend that you can win in the lottery. It will play a simple kiss the dog minigame inspired by one of the captchas from Kits video. It has a separate timer (120s) that can be reduced by kissing the dog on the lips (-30s each time) If you fail to do that at least once, you won't be able to collect him!

### AWP Dragon Lore Skin
<img width="466" height="331" alt="image" src="https://github.com/user-attachments/assets/b0622d7a-a638-4dfb-b508-f7cef639f9c6" />
To get the skin you will have to first prove your skills in a sniper minigame. Aim and shoot 5 targets within the time limit to get the reward.

### Bitcoin
<img width="383" height="355" alt="image" src="https://github.com/user-attachments/assets/c2bb898c-ee87-4b49-bfe2-baf9c7672d5c" />
To get the Bitcoin you'll have to help the Kraken get thorugh the maze first - hold and drag him with your finger/mouse to reach the finish line


### Abandon Prizes Confirmation
When the skip timer runs out and the skip button turns gold, clicking it does not skip the ad immediately. A full-screen confirmation popup appears warning you that you will lose all your collected prizes if you leave. You must explicitly confirm before the game enters drain mode. The popup is designed to look alarming enough to make you reconsider.

Once in drain mode, clicking the skip button a second time shows a second "LAST WARNING" popup with even more dramatic styling before finally letting you proceed to drain your remaining tickets.

### Out of Tickets Popup
When drain mode reaches zero tickets the game does not end — it shows a dedicated popup with two choices:

- **3 More Tickets** — Accepts more tickets and starts the bonus round. In the bonus round tickets refill automatically whenever they reach zero, making it impossible to ever truly run out again. The skip button still requires an "ABANDON PRIZES?" confirmation.
- **Skip Ad** — Ends the game immediately and posts the success signal to the shell.

The popup is worded to make "3 More Tickets" feel like the obviously correct choice, since you have a growing list of prizes already collected and leaving would mean losing them.

## Reward Tiers

All three game modes (wheel, slots, scratch cards) share the same prize pool divided across three tiers. You advance to the next tier every 6 spins (or 6 wins for slots). Each tier upgrade triggers a full-screen animated celebration popup.

### Tier 1 — Standard 🎟️

| Prize | Icon | Notes |
|-------|------|-------|
| Golden Wheel | ⭐ | Tier upgrade to Tier 2 |
| CrowPro 1 month | 🎮 | Fake antivirus subscription |
| Free Ticket | 🎡 | Next spin is free |
| $100 Casino Credits | 💵 | |
| $200 Gift Card | 💳 | |
| Windows RG License | 🪟 | Fake OS |
| Seraph Secure 1 month | 🛡️ | Fake antivirus subscription |
| 5 Tickets | 🎟️ | +5 tickets |
| $200 Casino Credits | 💵 | |
| $100 Gift Card | 💳 | |
| **BIKE!** | 🚲 | Jackpot |
| 10 Tickets | 🎟️ | +10 tickets |
| -30s Skip Timer | ⏩ | Reduces skip timer (early spins only) |

### Tier 2 — Gold 🌟

| Prize | Icon | Notes |
|-------|------|-------|
| Diamond Wheel | 💎 | Tier upgrade to Tier 3 |
| CrowPro 6 months | 🎮 | Fake antivirus subscription |
| Free Ticket | 🎡 | Next spin is free |
| **Dog** | 🐕 | Triggers dog minigame |
| $200 Casino Credits | 💵 | |
| $500 Gift Card | 💳 | |
| CrowPro Lifetime | 👑 | Fake antivirus lifetime license |
| WWWW Gold | 🏆 | Fake browser protection package |
| 5 Gold Tickets | 🌟 | +5 tickets |
| $500 Casino Credits | 💵 | |
| **Mobility Scooter** | 🦼 | Jackpot |
| 10 Gold Tickets | 🌟 | +10 tickets |
| -60s Skip Timer | ⏩ | Reduces skip timer (early spins only) |

### Tier 3 — Diamond 💎

| Prize | Icon | Notes |
|-------|------|-------|
| **1 BITCOIN!** | ₿ | Jackpot — triggers maze minigame (~$90,000) |
| Free Ticket | 🎡 | Next spin is free |
| **Dog** | 🐕 | Triggers dog minigame |
| $500 Casino Credits | 💵 | |
| $1000 Gift Card | 💳 | |
| **AWP Dragon Lore** | 🔫 | Jackpot — triggers sniper minigame |
| ~~Skip Ad~~ | ⏭️ | Appears in the prize pool with a weight of 0 — impossible to win |
| 5 Diamond Tickets | 💎 | +5 tickets |
| $1000 Casino Credits | 💵 | |
| $500 Gift Card | 💳 | |
| **WIN A CAR!** | 🚗 | Jackpot |
| 10 Diamond Tickets | 💎 | +10 tickets |
| -60s Skip Timer | ⏩ | Reduces skip timer (early spins only) |

> **Note on ticket prizes:** Ticket prizes (+5 and +10) are removed from the prize pool entirely while you have more than 3 tickets. The moment you drop to 3 or fewer they become available and will top you back up. In drain mode they are suppressed regardless of count — making drain mode the only state where running out is genuinely possible. The skip timer reduction prizes become increasingly rare in higher tiers and are only awarded during the first few spins. The "Skip Ad" prize visible on the Diamond Wheel has a spawn weight of 0 — it was never intended to be winnable.

## Easter Eggs
The game has two hidden time control buttons that are in no way indicated by the UI:
<img width="72" height="17" alt="image" src="https://github.com/user-attachments/assets/db9a6721-b7cc-4d07-9d22-5c0a0d716de8" />
-clicking green dot next to live chat reduses the skip ad timer by 100s
<img width="90" height="26" alt="image" src="https://github.com/user-attachments/assets/2f096839-1d9a-4f3f-aeb1-21a53edeac53" />
-clicking the speaker on the volume control adds 100s to your timer
Both those time adjustments are done with minimal UI (do not display the standard time added/reduce indicators)

## Additional Arguments
The submission.html accepts additional arguments that allow you to force the game ui or add a debug menu overlay which allows you to play the reward specific minigames and displays the safety net countdown

| Parameter | Effect |
|-----------|--------|
| `?game=wheel` | Force wheel-of-fortune game mode |
| `?game=slot` | Force slot machine game mode |
| `?game=scratch` | Force scratch card game mode |
| `?game=random` | Pick a random mode (default when omitted) |
| `?debug` | Show debug buttons (🐕 dog, 🔫 sniper, ₿ maze) and safety-net countdown |


