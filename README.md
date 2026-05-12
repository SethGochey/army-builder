> All of the code and the below readme were AI generated.

Factions tab — Full unit/datasheet/profile CRUD with clone at every level (faction → unit → datasheet → profile). Faction-wide profile library with shoot/melee tabs, stamp copies into any datasheet, save profiles from datasheets back to library. Keyword/detachment tags on units with pill display.
Spreadsheet paste — "Paste from Spreadsheet" button in the unit editor accepts tab-separated DATASHEET/SHOOT/MELEE rows copied from Excel or Google Sheets. Shows a preview with per-row status and error highlighting before committing. Appends to existing datasheets rather than replacing.
Armies tab — Create named army lists tied to a faction. Per-entry: adjustable model count per datasheet, checkboxes to select which profiles are active (wargear selection), a notes field, and a points field. Running total displayed on each army card. Detachment tags on each army. Clone army support.
Print — "Print List" button on each army card renders a clean black-on-white print layout with stat lines, selected weapon profiles per datasheet, notes, keywords, and a points total. Uses @media print so the app chrome disappears.
Export tab — Per-army export button plus "Export All." Output is baImportVersion: 1 JSON — only selected profiles, user-set model counts, stable UUIDs — ready to drop into Battle Assistant's Import & Merge.
Data tab — Backup export, import & merge / replace, clear all, and the paste format reference.
