> All of the code and the below readme were AI generated.
# 🛡 Army Builder — 40k List Constructor

A single-file, offline-capable web app for Warhammer 40,000 players. Build faction rosters, select wargear, set points, and export army lists directly into Battle Assistant — all before you leave the house.

> **No install. No account. No server.** Open the HTML file in any modern browser and play.

---

## What is this?

Army Builder is the pre-game companion to [Battle Assistant](https://github.com/SethGochey/battle-assistant). It handles everything before dice hit the table:

| App | Phase | What it does |
|---|---|---|
| **Army Builder** | Pre-game | Faction templates, unit datasheets, wargear selection, points tracking, list export |
| **Battle Assistant** | At the table | Combat tracking, activation management, probabilistic recommendations, simulation |

The two apps communicate through a JSON export/import contract. You build your list in Army Builder, export it, and import it into Battle Assistant to begin the battle.

---

## Features

### ⚔ Factions — Unit Template Library

Define reusable unit templates organized by faction. Everything you build here flows into your army lists.

- Create any number of factions (Space Marines, Orks, Necrons, etc.)
- Each faction holds **units**, each unit holds one or more **datasheets**
- Datasheets capture the full stat line: Model Count, Wounds, Toughness, Armor Save, Invuln Save, Feel No Pain
- **Shooting profiles** per datasheet: Name, Range, Attacks, BS, Strength, AP, Damage
- **Melee profiles** per datasheet: Name, Attacks, WS, Strength, AP, Damage
- Attacks and Damage fields accept dice expressions: `D6`, `2D6`, `D3`, `2D6+1`, or plain integers
- **Keyword / Detachment tags** on each unit (e.g. INFANTRY, CORE, ADEPTUS ASTARTES) — displayed inline and included on printed lists
- **Clone at every level** — faction, unit, datasheet, and individual profile. All clones get new UUIDs at every level and are fully independent

#### Faction-wide Profile Library
Save named profiles at the faction level — useful for shared weapons like Close Combat Weapon or Bolt Pistol that appear on many different units. From any datasheet editor, stamp a fully independent snapshot copy into that datasheet in one click. Edits to the copy never affect the library original.

---

### ⊕ Paste from Spreadsheet — Data Entry Accelerator

The most time-consuming part of getting started is entering faction data. Army Builder includes a paste-from-spreadsheet feature so you can copy rows directly from Excel or Google Sheets and bulk-import entire units in seconds.

**Format:** tab-separated rows, one per line.

| Row type | Columns |
|---|---|
| `DATASHEET` | Type · Name · Models · Wounds · T · Save · Invuln · FNP |
| `SHOOT` | Type · Name · Range · Attacks · BS · S · AP · Damage |
| `MELEE` | Type · Name · Attacks · WS · S · AP · Damage |

**Rules:**
- Profiles automatically attach to the closest preceding `DATASHEET` row
- Save values accept `3`, `3+`, or `-` (no save / none)
- Attacks and Damage accept full dice notation: `D6`, `2D6+1`, `D3`, or plain integers
- A live **preview panel** shows each row parsed or rejected before you commit, with per-row error messages — valid rows are always imported even if others fail
- Imported datasheets are **appended** to any existing datasheets on the unit — nothing is overwritten

**Example paste:**
```
DATASHEET	Intercessor Sergeant	1	3	4	3	-	-
SHOOT	Bolt Rifle	24	2	3	4	-1	1
MELEE	Close Combat Weapon	3	3	4	0	1
DATASHEET	Intercessor	4	2	4	3	-	-
SHOOT	Bolt Rifle	24	2	3	4	-1	1
MELEE	Close Combat Weapon	2	3	4	0	1
```

---

### 🛡 Armies — Compose Your Lists

Build named army lists from faction unit templates. This is where wargear selection and points tracking happen.

- Select a faction, then add units to your roster
- The same unit type can be added multiple times (each is an independent entry)
- **Per-entry wargear selection** — checkboxes let you choose exactly which shooting and melee profiles are active for each datasheet in this list. Only selected profiles appear on the print and in the Battle Assistant export
- **Per-entry model count** — adjust how many models are in each datasheet, independent of the template default
- **Per-entry points field** — set the points cost for each unit entry; the army card shows a running total
- **Notes field** per entry — record warlord traits, special roles, squad composition details, etc.
- **Detachment tags** on each army list (e.g. Gladius Task Force, Anvil Siege Force)
- **Clone army** — deep-clone a complete army list with all-new UUIDs

---

### ⬡ Print — PDF-Friendly Army List

Every army card has a **Print List** button. Clicking it generates a clean, print-optimized layout and opens the browser print dialog. The app chrome disappears entirely in print view.

The printed list includes:
- Army name, faction, detachments, and total points
- Per-unit: name, points cost, and any keywords
- Per-datasheet: T, W, Armor Save, Invuln Save, Feel No Pain stat line
- Model count per datasheet
- **Only the selected profiles** — if you deselected a weapon in army composition, it won't appear on the print
- Melee and shooting profiles in formatted tables with full stat columns
- Notes per unit entry

---

### ⬇ Export — Send to Battle Assistant

The Export tab lists all saved army lists with a per-list export button, plus an **Export All** button for a combined file.

The exported JSON file exactly matches the `baImportVersion: 1` schema that Battle Assistant's importer expects. Key properties of the export:

- `baImportVersion: 1` — Battle Assistant validates this field and will reject files where it is missing or wrong
- `shootingProfiles` and `meleeProfiles` on each datasheet contain **only the profiles the player selected** for this list — not all possible options from the template
- `modelCount` reflects the count the player set for this army entry, which may differ from the faction template default
- `entryId` and `datasheetId` values are **stable** — they don't regenerate on every export, so repeated imports don't create duplicates in Battle Assistant
- Multiple army lists may be exported in a single file; Battle Assistant will import all of them

---

### ⬡ Data — Backup, Restore, Reference

- **Export Backup** — downloads all factions and army lists as a full backup JSON file
- **Import & Merge** — loads a backup file and adds any factions/armies not already present (deduplicates by ID)
- **Import & Replace** — replaces all local data with the file contents (confirmation required)
- **Clear All Data** — wipes localStorage (confirmation required)
- **Paste Format Reference** — inline documentation for the spreadsheet paste row format

---

## Getting Started — Step by Step

### 1. Build your factions

1. Go to **⚔ Factions** → click **+ Add Faction**
2. Enter a faction name and click **Save Faction**
3. Click **+ Add Unit**, name the unit (e.g. "Intercessor Squad")
4. The unit editor opens — add **Keywords** like `INFANTRY`, `CORE`, `ADEPTUS ASTARTES` using the tag input
5. Click **+ Add Datasheet** for each model type in the unit (e.g. "Intercessor Sergeant" and "Intercessors" as separate datasheets)
6. Fill in the stat line for each datasheet
7. Add shooting and/or melee profiles using the inline tables, or use **Paste from Spreadsheet** to bulk-import
8. Attacks/Damage cells accept dice notation: `D6`, `2D6+1`, `D3`, or a plain number
9. Click **Save Unit**, then repeat for all units in the faction

**Tip:** Use the **Profile Library** at the bottom of the faction editor to save shared weapons once. Then stamp them into any datasheet with **⊕ From Library**.

---

### 2. Compose your army list

1. Go to **🛡 Armies** → click **+ New Army List**
2. Name the army and select its faction
3. Add optional **Detachment** tags
4. Click **+ Add** next to each unit you want to include (the same unit can be added multiple times)
5. For each unit entry:
   - Set **model count** per datasheet if it differs from the template default
   - **Check or uncheck** profiles to select which weapons this squad is carrying in this list
   - Set the **points cost** in the field on the entry header
   - Add any **notes** (warlord traits, special rules, etc.)
6. Click **Save Army List**

---

### 3. Export to Battle Assistant

1. Go to **⬇ Export**
2. Click **⬇ Export** next to the army list you want to send, or **Export All Lists** for a combined file
3. In Battle Assistant, go to **⬡ Data** → **Import & Merge** (or Import & Replace) and select the downloaded file
4. Your army will appear in Battle Assistant's **🛡 Armies** tab, ready to use in a battle

---

### 4. Print your list

1. Go to **🛡 Armies**
2. Click **⬡ Print List** on any army card
3. The browser print dialog opens with a clean black-and-white layout
4. Save as PDF or print directly

---

### 5. Back up your data

Go to **⬡ Data** → **Export Backup** to download a backup of all factions and army lists. Import it on another device or browser with **Import & Replace**.

---

## Paste Format Reference

Full column specification for the spreadsheet paste feature:

### DATASHEET row
| Col | Field | Notes |
|---|---|---|
| 1 | `DATASHEET` | Row type identifier (case-insensitive) |
| 2 | Name | Model name, e.g. "Intercessor Sergeant" |
| 3 | Models | Integer model count, e.g. `1` or `4` |
| 4 | Wounds | Per-model wounds, e.g. `2` |
| 5 | T | Toughness, e.g. `4` |
| 6 | Save | Armor save: `3`, `3+`, or `-` (none) |
| 7 | Invuln | Invuln save: `4`, `4++`, or `-` (none) |
| 8 | FNP | Feel No Pain: `5`, `5+++`, or `-` (none) |

### SHOOT row
| Col | Field | Notes |
|---|---|---|
| 1 | `SHOOT` | Row type identifier |
| 2 | Name | Weapon name |
| 3 | Range | Range in inches, e.g. `24` |
| 4 | Attacks | Dice or integer: `2`, `D6`, `2D6+1` |
| 5 | BS | Ballistic Skill, e.g. `3` (means 3+) |
| 6 | S | Strength, e.g. `4` |
| 7 | AP | Armor Penetration, e.g. `-1` or `0` |
| 8 | Damage | Dice or integer: `1`, `D3`, `D6` |

### MELEE row
| Col | Field | Notes |
|---|---|---|
| 1 | `MELEE` | Row type identifier |
| 2 | Name | Weapon name |
| 3 | Attacks | Dice or integer |
| 4 | WS | Weapon Skill, e.g. `3` (means 3+) |
| 5 | S | Strength |
| 6 | AP | Armor Penetration |
| 7 | Damage | Dice or integer |

---

## Export Contract — Battle Assistant Interface

The exported JSON file must exactly match this schema. Battle Assistant validates `baImportVersion` and rejects files where it is missing or not equal to `1`.

```json
{
  "baImportVersion": 1,
  "armies": [
    {
      "id": "uuid",
      "name": "1st Company Strike Force",
      "faction": "Space Marines",
      "entries": [
        {
          "entryId": "uuid",
          "unitName": "Intercessor Squad",
          "datasheets": [
            {
              "datasheetId": "uuid",
              "name": "Intercessor Sergeant",
              "modelCount": 1,
              "wounds": 3,
              "toughness": 4,
              "armorSave": 3,
              "invulnSave": 7,
              "feelNoPain": 7,
              "shootingProfiles": [
                {
                  "id": "uuid",
                  "name": "Bolt Rifle",
                  "range": 24,
                  "attacks": 2,
                  "attacksDisplay": "2",
                  "ballisticSkill": 3,
                  "strength": 4,
                  "armorPenetration": -1,
                  "damage": 1,
                  "damageDisplay": "1"
                }
              ],
              "meleeProfiles": [
                {
                  "id": "uuid",
                  "name": "Close Combat Weapon",
                  "attacks": 3,
                  "attacksDisplay": "3",
                  "weaponSkill": 3,
                  "strength": 4,
                  "armorPenetration": 0,
                  "damage": 1,
                  "damageDisplay": "1"
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

**Notes:**
- `armorSave`, `invulnSave`, and `feelNoPain` use `7` to represent "none" — Battle Assistant treats values of 7 or higher as no save
- `attacks` and `damage` are expected-value floats (e.g. `D6` = `3.5`); `attacksDisplay` and `damageDisplay` are the raw strings shown in the UI
- `faction` is a plain display string — Battle Assistant does not use it relationally

---

## Data Persistence

All faction and army list data is stored in your browser's **localStorage** under two keys:

| Key | Contents |
|---|---|
| `w40k_ab_factions` | Array of Faction objects including all units, datasheets, and profile library |
| `w40k_ab_armies` | Array of ArmyList objects including all entries, wargear selections, and points |

These keys use the `ab_` prefix to avoid collision with Battle Assistant's own localStorage keys (`w40k_factions`, `w40k_armies`).

**Army Builder and Battle Assistant can run in the same browser on the same device without interfering with each other.**

Battle state in Battle Assistant is ephemeral (in-memory only). Use the **Export Backup** feature in Army Builder to back up faction and list data before clearing browser storage or switching devices.

---

## Known Limitations (v1)

These are deliberate scope decisions, not bugs:

- **Points limits are not enforced** — the app tracks and displays total points but does not prevent you from exceeding a cap. This is intentional; different game modes and house rules vary too widely.
- **Detachment rules are not validated** — keywords and detachment tags are for organizational and print purposes only. Legal detachment construction is the player's responsibility.
- **No image support** — unit art or icons are not part of the data model.
- **No cloud sync** — data lives in your browser's localStorage. Use Export Backup to move data between devices.
- **Import from Battle Assistant is not supported** — the data flow is strictly one-way (Army Builder → Battle Assistant). If you have faction data already in Battle Assistant, you'll need to re-enter it in Army Builder.

---

## Browser Support

| Browser | Support |
|---|---|
| Chrome / Edge 90+ | ✅ Full |
| Firefox 88+ | ✅ Full |
| Safari 14+ | ✅ Full |
| Mobile Chrome / Safari | ✅ Full (touch-optimised) |

---

## Using Both Apps Together

```
Army Builder                     Battle Assistant
─────────────────                ────────────────────────
⚔ Build factions        ──▶     (used as reference)
🛡 Compose army list    ──▶     ⬡ Data → Import
⬡ Print list            ──▶     (bring to table)
                                 ☩ Begin Battle
                                 ⚔ Attack recommendations
                                 ⬡ Simulate Battle
```

1. Use **Army Builder** at home to build your faction templates, compose your list, select wargear, set points, and print your army sheet
2. Export the list as JSON
3. Open **Battle Assistant** on your phone or tablet at the table
4. Import the JSON via **Data → Import & Merge**
5. Select your army in the Battle view and begin

---

## Contributing

Issues and pull requests welcome. The entire app lives in a single HTML file — no build step, no dependencies, no framework. Just open it and edit.

---

*Not affiliated with Games Workshop. Warhammer 40,000 is a trademark of Games Workshop Ltd. This tool is a fan-made utility for personal use.*
