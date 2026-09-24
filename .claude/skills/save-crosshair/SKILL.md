---
name: save-crosshair
description: Save the crosshair currently active in CS2 as a new preset (custom_chN.cfg) and add it to the F8 crosshair menu. Use when the user wants to save/copy their current or a just-imported crosshair as a preset.
disable-model-invocation: true
---

Create a new crosshair preset from the crosshair CS2 currently has saved.

## 1. Read the current crosshair
File: `C:\Program Files (x86)\Steam\userdata\*\730\local\cfg\cs2_user_convars_0_slot0.vcfg` (glob the steam id; if several exist, use the most recently modified).
Note: CS2 only writes this file when settings change/on exit, so if the user says the crosshair was just changed and values look stale, tell them to open Settings or leave the map once, then retry.

Take these cvars from it (values are quoted; `true`/`false` must become `1`/`0`):
```
cl_crosshair_dynamic_maxdist_splitratio, cl_crosshair_dynamic_splitalpha_innermod,
cl_crosshair_dynamic_splitalpha_outermod, cl_crosshair_dynamic_splitdist,
cl_crosshair_dynamic_spread_limit, cl_crosshair_friendly_warning, cl_crosshair_drawoutline,
cl_crosshair_sniper_width, cl_crosshaircolor_a/_r/_g/_b, cl_crosshair_t, cl_crosshairdot,
cl_crosshair_gap, cl_crosshair_length, cl_crosshair_thickness, cl_crosshairstyle,
cl_crosshair_recoil, cl_grenadecrosshairdelay_smoke/_decoy/_explosive/_flash/_fire
```
Ignore legacy cvars (cl_crosshairsize, cl_crosshairthickness, cl_crosshairgap, cl_crosshairalpha, cl_crosshaircolor, cl_fixedcrosshairgap, ...) and `cl_crosshair_screen_height`. If a cvar is missing from the file, copy the value from the newest existing preset.

## 2. Create the preset
Pick N = highest existing `crosshair/custom_chN.cfg` + 1. Write `crosshair/custom_chN.cfg` in the same format as the other presets (cvar lines, blank line, the two `bind "9"`/`bind "0"` lines, blank line, echo line). Use ascii, one `cvar value` per line.

## 3. Wire it into the menu chain
Presets form a ring: bind 9 = previous, bind 0 = next. Let LAST = the current highest preset (before N).
- `custom_chN.cfg`: `bind "9"` -> LAST, `bind "0"` -> whichever preset LAST's `bind "0"` pointed to (the ring start, currently CH1).
- `custom_chLAST.cfg`: change `bind "0"` and its echo text to point to N.
- The ring start (CH1): its `bind "9"` (and echo) currently points to LAST; change it to N.
- `crosshair/menu.cfg`: update the entry lines `bind "9"`/`bind "0"` and the echo `[9] Preset ... [0] Preset ...` only if needed so the entry presets stay valid (currently 9 = CH1, 0 = CH7 = last). Set `[0]` to the new last preset N.

## 4. Report
Tell the user the preset number, the values saved (style, length/gap/thickness, color, outline), and that F11 in-game (on a non-Workshop map) reloads the menu. Do not commit unless asked.
