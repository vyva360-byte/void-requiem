# DeepSeek v4.1 Limits Test — "VOID REQUIEM" build prompt
Purpose: one prompt, maximum system density. A mass-appeal single-player arena survival with interlocking systems that cannot be faked — if the model stubs, skips, or hand-waves a system, it shows immediately in play. Paste everything in the block into DeepSeek.

```
Build me a complete, playable browser game in ONE single self-contained HTML file (all CSS and JavaScript inline, no external assets, no libraries, no placeholders, no TODO comments, no stub functions). If your response runs long, continue the code across messages until the file is complete — do not compress, summarize, or skip systems to save space. Every system listed below must be fully implemented and working together.

## THE GAME — "VOID REQUIEM"
A single-player arena survival game ("bullet heaven"). You are the last lantern-keeper in a drowned cathedral void. Swarms of the Hollow pour in from the dark for 20 minutes. You move; your weapons fire on their own. Survive, level up, draft a build, evolve your weapons, kill the bosses, and ring the Final Bell at 20:00 — or die and go again. Runs must feel fun AND stressful: the screen should regularly be one mistake away from disaster.

## CONTROLS & PLATFORM
- WASD / arrow keys to move; mouse optional for menus only. Full touch support: virtual joystick appears on touch devices.
- Canvas rendering, devicePixelRatio-aware, resizes to window. Must hold 60fps with 300+ enemies on screen — use object pooling for enemies, projectiles, particles, and damage numbers (no per-frame allocation in hot loops, no DOM elements for entities).

## CORE LOOP
- Enemies spawn from off-screen edges on a wave director (below) and path toward the player with separation steering so they swarm instead of stacking.
- Kills drop XP shards; walking over them collects (magnet radius stat). Filling the XP bar pauses the action and offers a LEVEL-UP DRAFT: 3 random choices (new weapon, weapon upgrade, new passive, passive upgrade, or — if nothing is available — a heal or gold). Player picks 1 (keys 1/2/3 or click). Reroll button once per draft if the "Fate" passive is owned... no — reroll charges are a meta unlock; base draft has NO reroll.
- Loadout limits: 4 weapons + 4 passives max. Drafts must respect limits and max levels (weapons/passives cap at level 5).
- Gold drops from elites/bosses, persists between runs as meta currency (localStorage).

## THE PLAYER
- Stats: HP 100, move speed 170 px/s, armor 0, magnet 60px, might +0%, cooldown −0%, area +0%, duration +0%, regen 0 HP/s, luck 0.
- I-frames 0.4s after taking a hit; hit flash + knockback on enemies. Death = run over.

## WEAPONS (auto-firing, each with level 1–5 scaling: damage, count, cooldown, special at L5)
1. **Lantern Darts** — piercing bolts at the nearest enemy. L5: +2 darts, double pierce.
2. **Orbit Blades** — blades circle the player, contact damage + knockback. L5: +2 blades, wider orbit.
3. **Bell Tolls** — periodic radial shockwave nova around the player. L5: two novas, larger radius.
4. **Seeker Moths** — homing projectiles that retarget on kill. L5: moths split into 2 on kill.
5. **Chain Penance** — lightning arcs between clustered enemies every few seconds. L5: +3 arcs, arcs can jump twice.
6. **Frost Hymn** — cone of frost in facing direction that slows (40%, 1.5s). L5: freezes solid for 0.6s.

## EVOLUTIONS (the build chase — each requires the weapon at L5 + its paired passive at any level; offered in drafts as a golden choice)
- Lantern Darts + Swift Boots → **Dawnpiercer**: darts become a fan of 5, +50% damage.
- Orbit Blades + Iron Halo → **Event Horizon**: blades are bigger, pull enemies slightly inward.
- Bell Tolls + Resonant Core → **Doomsday Bell**: nova also leaves a burning zone for 3s.
- Seeker Moths + Luck Candle → **Monarch Swarm**: moths explode on death for AoE.
- Chain Penance + Storm Sigil → **Judgment Lattice**: arcs leave static fields that zap passers-by.
- Frost Hymn + Deep Chalice → **Absolute Vigil**: cone becomes a full ring pulse, freeze 1s.

## PASSIVES (level 1–5, stat per level)
Swift Boots (+6% speed), Vital Chalice (+20 max HP, heal 20 on pickup), Iron Halo (+2 armor), Magnet Heart (+25px magnet), Might Sigil (+8% damage), Cooldown Tome (−5% cooldowns), Area Lens (+8% area), Luck Candle (+8% crit chance; crits deal 2× and show yellow numbers).

## ENEMIES (all with HP/damage/speed scaling by minute — see Difficulty)
- **Husk** — basic chaser. **Skitter** — fast, low HP, lunges. **Brute** — slow, high HP, knockback-resistant. **Spitter** — keeps distance, fires a projectile every 3s. **Splitter** — splits into 3 Minis on death. **Mini** — tiny fast weakling. **Wraith** — phases (untargetable/invisible 1s in every 4s, visible on attack).
- **ELITES** (every ~2 min, announced with a banner + roar sound cue visual): 12× HP Brute or Spitter variant, drops gold + a chest that grants a free draft choice instantly.
- **BOSSES** at 5:00, 10:00, 15:00: **The Drowned Choir** (spiral projectile patterns + summons Husks), **Mother of Static** (teleports, radial bursts, spawns Skitters), and at 20:00 **THE FINAL BELL** — killing it wins the run (victory screen). Bosses have HP bars and at least 3 attack patterns each, telegraphed (flash/wind-up) so deaths feel fair.

## WAVE DIRECTOR (minute-by-minute pressure curve — implement as a data table)
Spawn interval and batch size tighten every minute; composition unlocks: Skitters at 1:00, Spitters 3:00, Splitters 5:00, Wraiths 8:00, Brutes trickle from 2:00 and thicken. Minutes 13–19 are the "long dark": 1.6× spawn rate. Cap concurrent enemies by difficulty; spawn budget system, not fixed timers only.

## DIFFICULTY LEVELS (chosen on the menu; everything below must actually change)
- **CHILL** — enemy HP ×0.7, damage ×0.6, speed ×0.9, spawn rate ×0.75, XP ×1.25, concurrent cap 180. For learning.
- **STANDARD** — all ×1.0, cap 260. The intended game.
- **BRUTAL** — HP ×1.6, damage ×1.4, speed ×1.08, spawn ×1.35, XP ×1.1, cap 340, elites every 90s.
- **REQUIEM** — HP ×2.3, damage ×1.8, speed ×1.15, spawn ×1.7, XP ×1.15, cap 420, elites every 75s, bosses +40% HP, and the player starts at 75 HP. A skull icon marks it. Beating REQUIEM must feel like an achievement: special victory screen line + gold bonus ×2.
Per-minute enemy scaling applies ON TOP: HP ×(1 + 0.22 × minute), damage ×(1 + 0.06 × minute).

## META PROGRESSION (localStorage, survives reloads)
Spend gold between runs on permanent upgrades (5 ranks each, rising costs): +5 Max HP/rank, +2% Might/rank, +3% Speed/rank, +10px Magnet/rank, +1 Reroll Charge/rank (adds a reroll button to drafts), Greed +5% gold/rank. Plus a stats page: runs, wins per difficulty, best time survived, total kills, favorite weapon (most damage dealt). A "Reset Save" button with a confirm.

## GAME FEEL (non-negotiable — this is where the stress and fun live)
- Screen shake (trauma-based, decays), hitstop (30–60ms freeze on elite/boss kills and player hits), particle bursts on deaths (pooled), floating damage numbers (pooled, crits yellow and bigger), enemy flash-white on hit, player low-HP heartbeat vignette under 30% HP, boss warning banner, level-up time-freeze with a draft fanfare visual, kill-streak counter that appears at 25+ kills without taking damage ("UNTOUCHED ×N") and resets when hit. Muzzle/impact flashes per weapon. Muted cathedral palette: deep indigo/charcoal void, bone-white enemies with one accent each, the lantern warm gold — player and XP are the warmest things on screen.

## AUDIO
- WebAudio-synthesized SFX only (no files): hit ticks, XP pickup blips rising in pitch with combo, level-up chime, boss roar (filtered noise), player hurt thud, evolution fanfare arpeggio. A drone pad ambience that gains a layer each boss phase. Mute toggle (M) and volume persisted.

## SCREENS & FLOW
Title (game name, animated swarm background, difficulty select, Continue/Start, Meta Upgrades, Stats, How to Play) → Run (HUD: HP bar, XP bar + level, timer, kill count, gold, build icons with levels, boss HP bar when active, streak counter) → Pause (Esc/P: resume, restart, quit to title, current build list) → Level-Up Draft overlay → Game Over (time survived, level, kills, gold earned, damage by weapon table, Retry / Title) → Victory variant when the Final Bell dies (with total stats).

## ACCEPTANCE CHECKLIST (verify each is TRUE before you call it done, and tell me which file line ranges implement each)
1. A fresh player can start STANDARD, move, auto-fire, collect XP, and draft at level 2 within the first minute.
2. Two weapons can evolve in one run and visibly change behavior.
3. Bosses appear at 5:00/10:00/15:00 with HP bars, telegraphed patterns, and the 20:00 Final Bell ends the run in victory when killed.
4. All four difficulties produce measurably different pressure (state the exact numbers in code).
5. Meta upgrades persist across a page reload and reroll charges actually appear in drafts when bought.
6. The game survives 300+ concurrent enemies without the frame rate collapsing (pooling implemented; state where).
7. Touch controls work: joystick moves, everything else is automatic or tappable.
8. No console errors on load or during a full run.

Do not ask me questions. Make reasonable decisions, note them in a short "Decisions" list at the top of the file as comments, and build the whole thing.
```

## How Vic uses it
Paste into DeepSeek v4.1 chat. The tells to watch: (1) whether it returns ONE complete file or quietly amputates systems (draft limits, evolutions, and the wave table are the usual first casualties); (2) whether bosses have real patterns or are just fat Husks; (3) whether difficulty numbers exist in code or only in menu text; (4) run it and check the acceptance checklist line by line. If it truncates, the follow-up is: "Continue the file from exactly where you stopped. Do not restart or summarize."
