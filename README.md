# LICHBORNE — Gear Tracker

**A World of Warcraft WotLK 3.3.5a Addon for AzerothCore Private Servers that tracks gear score, iLvL, gear slots, specs, and raid composition for your entire playerbot roster.**

**Version 1.78**

---

## Screenshots

![Class Tracker](Screenshots/ClassTab.png)
![Raid Planner](Screenshots/RaidTab.png)
![Character Sheet](Screenshots/CharacterSheet.png)

---

## Recent Changes (v1.78)

- **Improved gear and spec scan sequence** — Rebuilt scan logic provides more reliable results with significantly fewer failed inspects during group scans
- **Import / Export character data** — Transfer your tracked roster between accounts. Export your characters to a text string and import them on another account or device
- **Right-click > button to kick from group** — On Class and All tabs, right-clicking the > (Invite to Group) button now kicks that bot from your party using a playerbot command
- **Delete row no longer leaves gaps** — Deleting a character from the Class tab or Raid tab compacts remaining entries up immediately
- **Fixed: gear scan row wipe on zero iLvL items** — Items with an iLvL of 0 (PvP trinkets, relics, quest items) no longer cause the entire row to retry and potentially nil out. These items are accepted on the first pass
- **Sortable column headers** — All three tabs (Class, Raid, All) now have clickable column headers for sorting. No more Sort dropdown button
  - **Class tab:** Sort by Spec, Name, iLvL, GS, or any of the 17 gear slot columns. Each class tab keeps its own independent sort state
  - **Raid tab:** Sort by Spec, Name, iLvL, GS, or Role. Role sort cycles Tank → Healer → DPS or Healer → Tank → DPS. Spec sorts class A-Z or Z-A
  - **All tab:** Sort by Spec, Name, iLvL, GS, or raid membership (+). The + header turns orange when sorting in-raid-first
  - Active sort column shows a ^ or v direction indicator. Empty rows always sink to the bottom
- **In-frame Output box** — Status messages now appear in a scrollable log at the bottom of the tracker instead of in chat. Holds 500 lines, mouse-wheel scrollable. Expand/collapse toggle for more visible lines
- **Debug Mode (DBG) button** — Toggle detailed inspect logging directly from the output box
- **Right-click + button to remove from raid** — On Class and All tabs, right-clicking the green + removes that character from the active raid roster
- **+ button color reflects raid membership** — Tinted with the active tier color when in raid; standard green otherwise
- **Log in / Log Out All Bots** and **Log Out Orphaned Bots** button label polish and reliability improvements
- **Multiple UI updates** — Tooltip wording clarified across buttons and controls; improved spacing and alignment throughout the frame
- **Bug fixes: scanning** — Resolved edge cases causing scans to stall, skip characters, or report incorrect results under certain group compositions
- **Bug fixes: sorting** — Fixed inconsistent sort behavior when rows contained empty or partially populated entries
- **Bug fixes: logic** — Corrected several state management issues affecting button enable/disable conditions and scan sequencing

(See CHANGELOG.md for full version history)

---

## Features

### Class Tabs

Each of the 10 playable classes has its own tab with up to 54 roster slots (per class) across 3 pages. Each character row tracks:

- Spec icon — auto-detected from talent inspection. Can be manually changed.
- Name — editable, colored by class
- iLvL — average equipped item level calculated via inspect
- Gear Score — actual WotLK-style GearScore calculated from inspected gear, colored by item quality
- **Need Box** — up to 2 gear slots marked as needed, shown as slot icons
- 17 gear slots — Head, Neck, Shoulders, Back, Chest, Wrists, Hands, Waist, Legs, Feet, Ring 1, Ring 2, Trinket 1, Trinket 2, Main Hand, Off Hand, Ranged
- Add to Raid (+) and Invite to Group (>) buttons per row
- Hover any gear slot to see the full item tooltip

### Bottom Controls

- **+ Add Target** — Inspects your current target and adds them
- **+ Add Group** — Bulk-adds all group/raid members
- **+ Add Target/Group Gear** — Refreshes both iLvL and GS from inspect (does not affect spec); disabled during active scan
- **+ Add Target/Group Spec** — Reads talent spec (does not affect GS); disabled during active scan
- **Stop** — Cancels a running Gear or Spec scan
- **Maintenance** — Sends maintenance to group chat
- **AutoGear** — Sends autogear to group chat
- **Log in / Log Out All Bots** — .playerbots bot add/remove *
- **Log Out Orphaned Bots** — Logs out any roster bots not currently in your active raid group, preventing ghost bots from holding slots
- **Disband Group / Raid** — Kicks all members then leaves. Requires confirmation
- **Invite Raid / Stop Invite** — Visible on all tabs; disabled while invite sequence is running
- **Import / Export** — Transfer roster data between accounts

### Output Box

- Scrollable log at the bottom of the tracker window
- All status messages route here instead of chat
- Mouse-wheel scrollable, 500-line history
- Expand (∧) / collapse (∨) toggle for more visible lines
- **DBG button** — toggles detailed inspect debug logging (green = active)

### Summary Bars

- **Avg bar** — average tracked item level per class (values in gold)
- **GS bar** — average GearScore per class (values in gold)
- **Count bar** — total characters per class

---

## Need Box

Per-character gear slot wishlist, accessible from all tabs.

- 15 selectable slots: Head, Neck, Shoulders, Back, Chest, Wrists, Hands, Waist, Legs, Feet, Ring, Trinket, Main Hand, Off Hand, Ranged
- Max 2 needs per character
- Click a Need Box cell to open the popup
- Left-click a slot icon to mark as needed, right-click to remove
- At max (2), remaining slots are dimmed
- Right-click the Need Box cell itself to clear all needs for that character
- Changes sync instantly across Class, All, and Raid tabs
- Stored in LichborneTrackerDB.needs per character name

---

## Raid Tab

Up to 40 slots across two columns. Each slot shows class icon, spec icon, name, iLvL, GS, needs, role, notes, and delete button.

### Raid Controls

- **Column header sorting** — Click Spec, Name, iLvL, GS, or Role to sort. Sorts fire once per click; editing fields won't re-sort until you click the header again
- **Tier / Raid / Group dropdowns** — Tier color matches the progression tiers from the Individual Progression module for AzerothCore
- **Copy** — Copies current roster to session clipboard
- **Paste** — Prompts confirmation, pastes into destination, disappears after one use
- **Clear** — Clears roster with confirmation

### Copy / Paste

1. Navigate to source roster and click **Copy**
2. Navigate to destination and click **Paste**
3. Confirm the prompt
4. Status bar shows "Roster copied!"

Clipboard is session-only. Paste respects destination raid size.

### Invite Raid

Automatically logs out old bots, leaves party, converts to raid, and invites all roster members via .playerbots bot add. The Invite Raid button is disabled while the sequence is running.

---

## Character Sheet (All Tab)

Master view of all tracked characters across all classes — 3 columns of 20 rows (60 per page, 180 total).

- Groups A, B, C for organizing characters
- Sort by Spec, Name, iLvL, GS, or raid membership (+ header)
- Need Box column editable per row
- Add to Raid (left-click +) and Remove from Raid (right-click +) per row
- Invite to Group (left-click >) and Kick from Group (right-click >) per row
- Delete characters directly
- Count bar shows totals across all pages

---

## Tier Key

Color-coded tier reference bar at the top of the frame, aligned with the Individual Progression module for AzerothCore. Hover any swatch to see the full tier name and associated raids.

---

## Installation

### Option 1 — Git Clone (recommended, stays updated)

Navigate to your AddOns folder and run:

```
git clone https://github.com/Lichborne-AC/LichborneTracker
```

To update later just run `git pull` inside the LichborneTracker folder.

### Option 2 — Manual Install

1. Download the latest zip from the Releases page
2. Extract and drag the LichborneTracker folder into:

    World of Warcraft/Interface/AddOns/

3. Launch WoW and type `/lichborne` or click the minimap book icon

**Requirements:** WoW 3.3.5a (WotLK) | AzerothCore | Playerbot module

---

## How To Use

### First Time Setup

1. Open the tracker with `/lichborne` or click the minimap book icon
2. Target a character and click **+ Add Target**
3. Or get everyone at once: group up and click **+ Add Group**

### Tracking Gear

- **+ Add Target/Group Gear** — updates both iLvL and GS without touching spec
- Hover any gear slot to see the full item tooltip
- Gear slot colors reflect WoW item quality

### Building a Raid Roster

1. Switch to Raid tab and select tier and raid
2. Use + on any character row to add them (right-click to remove)
3. Assign roles and notes
4. Click **Invite Raid**

### Marking Needs

1. Click any Need Box cell on the Class, All, or Raid tab
2. Select up to 2 slot icons from the picker
3. Right-click a slot to remove it, or right-click the cell to clear all

### Copying a Roster

1. Navigate to source roster and click **Copy**
2. Switch to destination and click **Paste** then confirm

### Importing / Exporting Characters

Use the Import/Export button to generate a text string of your current roster. Copy it and import it on another account to transfer your tracked characters and gear data.

### Disbanding

Disband Group / Raid kicks every member via .playerbots bot remove, waits, then calls LeaveParty(). Requires confirmation.

---

## Data & Saved Variables

Stored under LichborneTrackerDB and LichborneMinimapIconDB per WoW account.

| Key | Contents |
| --- | --- |
| rows | All tracked characters, item levels, and GearScore data |
| allGroups | All tab group assignments (A/B/C) |
| raidRosters | Raid rosters keyed by raid name + group |
| needs | Gear needs per character |
| raidName | Currently selected raid |
| raidTier | Currently selected tier |
| raidGroup | Currently selected group (A/B/C) |

**Clear All Data** permanently deletes all tracked characters, gear, rosters, and needs data.

---

## Known Limitations

- Inspect requires target within ~28 yards
- GearScore depends on the inspect data returned by the server for the target's equipped items
- NotifyInspect() is rate-limited — bulk scans space out automatically
- Playerbot commands sent via SAY chat — requires bot ownership
- Roster clipboard is session-only (lost on /reload)

---

## Slash Commands

| Command | Action |
| --- | --- |
| /lichborne | Toggle the tracker window |
| /lbt | Toggle the tracker window (short alias) |

---

## Credits

Built for the Lichborne AzerothCore private server.

Special thanks to: **Dohtt**, **Scarecr0w12** — TheCGN.net, **Dreathean**, **Revision**, **crow**, and **ScoobyPwnsOnU** for feature suggestions, testing, and support.

**Questions & Support:** lichborne.wow@proton.me | Discord: jared2219

---

## Compatibility

WoW 3.3.5a (build 12340) | AzerothCore | Playerbot Module
