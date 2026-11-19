# WB Crew Duty Check

A single-page tool that reads West Midlands Bus “WB CREW DUTY SUMMARY” PDF files and checks every duty against the crew duty rules.

PDFs are read in your browser with [pdf.js](https://mozilla.github.io/pdf.js/). Nothing is uploaded anywhere.

## How to use

1. Open the page (see GitHub Pages below).
2. Click **New job: choose PDFs** and select one or more duty summary PDFs (for example Mon–Thu, Friday, Saturday and Sunday together).
3. Each upload creates a new **job**. Earlier jobs stay in the **Jobs** list, so you can go back to previous analyses. Click a job name to rename it.
4. **Add PDFs to this job** adds files to the selected job. A file with the same name replaces the old one.
   You can also choose the `.DCL` duty files (e.g. `WBMHH55.DCL`, `WBFOH55.DCL`, `WBSAH55.DCL`, `WBSUH55.DCL`) instead of PDFs: every piece is read with sign-on, board, times and sign-off, and compass, relief, time on bus and pay are worked out exactly as in the duty summary PDF. Where a duty has two versions, the school holidays version (1) is left out.
5. Use **Errors only / All**, the type filter and the search box to look through duties. **Download errors (CSV)** saves the errors of the selected job for Excel.

## Garages

Use **Garage** (top right, next to Undo) to switch between depots, e.g. West Bromwich and Walsall. **Garages…** adds, renames or deletes a garage. Each garage keeps its own:

- duty rules and 4 Day duty numbers,
- driver assignment settings, including its rotas and their duty numbers,
- driver list,
- sub rota duty numbers in the Rota creator.

Jobs and rota jobs are listed under the garage they were made in. A new garage can start from the default rules or copy the rules and settings of another garage (without its rotas, drivers or 4 Day numbers). Everything made before garages existed belongs to West Bromwich.

## 4 Day duties

The **4 Day duties** panel in Duty check holds the 4 Day duty numbers of the selected garage, separately for Mon–Thu, Friday, Saturday and Sunday PDFs. Write numbers or ranges, comma-separated: `2401-2499, 2510`. Keep the letter for lettered duties: `F2401-F2499`. Under each field you see how many duties of the selected job match. The numbers are used by Duty check, Driver assignment and the Rota creator (Sunday 4 Day premium, applied when a rota is opened).

Jobs, rules and manual type changes are saved in the browser you use (local storage). They are not shared between computers or browsers, and clearing your browser data removes them.

## Driver assignment

Open the **Driver assignment** tab to give duties to drivers. **Duties from job** (Assignment section) chooses which Duty check job is used; by default it is the job open in Duty check.

1. Add drivers, or copy the rows from your Google Sheet (columns No to saturday) and use **Paste from Google Sheets**. `RD` in a day column becomes a rest day.
2. For each driver set **Rota** (WB, Soho, Bearwood, Oldbury), **Duty type** (Early, Late, Finish before, Start after, or both), **Special condition** (minimum break or specific routes) and working days.
3. The tool picks a duty for each working day so each driver's weekly pay is as close as possible to the target (default 35:30). Duties can differ from day to day, and duties with duty-check errors can be used (both can be changed in the settings). Drivers with a lower No are served first.
4. Click a duty to change it by hand. Your choice is marked **fixed** and the other drivers are re-planned around it.
5. **Export to rota** sends the result to the Rota creator: choose a rota job and a sub rota (or a new one). Each driver becomes a line with the driver's name under the line number.
6. **Export to Google Sheets**: one row per driver with number, roll number, name, rota, the requests (duty type, special condition, working days, notes — optional) and the duty given on each day (RD on rest days), plus weekly pay, difference to the target and the reason for any day without a duty. Optionally each duty shows sign-on, sign-off and pay. *Copy table* and paste into cell A1, or *Download CSV* and use File → Import in Google Sheets. Duty numbers keep their leading zeros (012).

Rotas and their duty numbers are set under **Assignment settings**: rename a rota by editing its name, **Add rota** for a new one, × to remove one. West Bromwich defaults:

| Rota | Mon–Thu | Fri | Sat | Sun |
|---|---|---|---|---|
| WB | 001–099 | F001–F099 | 401–499 | 821–899 |
| Soho | 201–399 | F201–F399 | 501–599 | 701–799 |
| Bearwood | 3001–3099 | F3001–F3099 | 6001–6099 | 7601–7699 |
| Oldbury | 7001–7199 | same as Mon–Thu | 7501–7599 | 9501–9599 |

Oldbury runs the Mon–Thu duties on Friday too, so Oldbury Fridays are planned from the Mon–Thu PDF.

Every driver gets at least 11:00 rest between finishing one duty and starting the next day's duty (editable). With **Finish before**, duties that finish closest to the requested time are preferred.

Early means book off by 14:30 and Late means book on from 15:00 (both editable). Routes come from the running board numbers: 12/xx covers routes 12 and 13, 87/xx covers 82 and 87, and any other board runs its own number (74/10 is route 74). A specific route request means every piece of the duty runs on one of the requested routes. Jobs uploaded before this version have no sign-on/off times stored, so upload their PDFs again to use time and route requests.

## Rota creator

Works like OmniROTA. Export the rota (`.RIF`) and its duties (`.DIF`) from OmniROTA and open them in the **Rota creator** tab. The `.DIF` file gives the exact sign-on/off times, values and pay codes; without it the duties of the selected PDF job are used.

- **Open OmniROTA folder** looks for the rota files (`.ORD`) and the duty files (`.DCL`) in the folder. If there is more than one rota (e.g. the original and a `ROCR` copy) or more than one set of duty files, it asks which to open. The duty pieces come from the `.DCL` files, or from the rota's own `.ORL` file when there are none; they are used for the duty rule checks in Global statistics. Choose the OmniROTA folder with the rota's `.ORD` file (or open the `.ORD` with *Open rota file*). It holds everything: all sub rotas and lines, every duty with sign-on/off, value, type (E, M, L, S as set in OmniROTA), pay grade and days, plus each sub rota's minimum/maximum week, stand-by pay, minimum rest and Up/Static/Down, the start date and the RD/SP codes. No separate RIF or DIF export is needed. Where a duty has school-day and school-holiday versions, the version used by the rota lines is kept.
- **Sunday 4 Day pay**: OmniROTA stores only the duty value (time worked) for Sunday 4 Day duties in the `.ORD`. The duty files (`.DCL`, or the rota's `.ORL`) have the value as paid, with the 16% premium, and that value is used. Only without them is the pay value worked out as the duty value plus the 16% premium (× 1.16, editable in Main rota settings), marked `*` in the duty list. Which duties are 4 Day comes from the Sunday field of the 4 Day duties panel in Duty check (790-799 for West Bromwich). **Save for OmniROTA** writes the pay value into the duty table and the week totals; a file saved here (… ROCR) is not given the premium again when reopened.
- **Global statistics** (Tools) starts with checks, each ✓ or ✗ with the list behind it: all duties allocated (every duty in the duty list is on a rota line), no duty used twice, **RTA: 11:00 rest between days** on every line of every sub rota (also Saturday → next line's Sunday, whatever the sub rota's own setting), duty rules from the Duty check tab (compass, relief, time on bus — worked out from the duty pieces in the `.ORL` or `.DIF` file), all duty numbers known, and no empty days. Below it: one table with every sub rota of the job — lines, average week, short weeks, make up and the lowest make up possible, long weeks and excess hours, expected/worked/roster hours, RD, SP, incomplete weeks and lines with problems — with a total row. Click a sub rota to open it.
- **Lines on screen** (View): Normal, or 10, 15, 20, 25 or 30 weeks at once; cells get smaller (two rows, then one row) to fit. On a wide screen the Rota creator fills one screen, with the toolbar always visible.
- **Rota jobs**: opening a rota (.RIF) creates a new rota job, saved in the browser with its duties, week types and settings. **Copy job** duplicates the current one: rename it, then **Load duties (.DIF)** to try new duties on the same rota. **New job** starts empty; **Delete job** needs a second click.
- **Duty numbers of each sub rota** (Sub rota settings → *Duty numbers of this rota*): ranges for Mon–Thu, Fri, Sat and Sun, comma-separated. They drive the Subset filter, the list of duties on no rota, Auto-fill and Balance weeks. WB, SOHO, BW and OLD start with the defaults from Driver assignment.
- **Current rota** (top right) picks the sub rota. Each line shows duty number, type (E, M, L, S), sign on and off, and value; the yellow column is the week total, red with the difference when it is under the minimum or over the maximum week value.
- Click a cell, then click a duty in **Duties available for …** on the right to put it there. **All duties / Unassigned** and **Subset** filter the list; **Line** shows where a duty is already used.
- Rest under the minimum daily rest is highlighted on the sign-off and sign-on times, also from Saturday to the next line's Sunday when the rota moves up or down.
- Toolbar: Edit, Clear, Enter 'SP', Toggle 'RD', Insert and Remove lines, and:
  - **Swap days** opens *Rota line check and swap*: two lines side by side, click a day to select it, move the lower line with ▲ ▼ or Jump to, Switch weeks, Cross scrolling, and swap *duties on selected day*, *day and following days*, *duties all week* (RD and SP stay) or *complete weeks*.
  - **Auto-fill** puts duties that are on no rota into empty days (keeping the minimum rest, longest duties to the shortest weeks); if no empty day fits, the duty replaces an SP day, except in full SP weeks. If empty days are left with no duty, it asks whether to enter SP.
  - **Balance weeks** rearranges the rota's own duties for the lowest make up time: no week above the minimum while another is below it, no week over the maximum, minimum rest kept. The message shows make up before and after, and the lowest possible with the hours on the rota. Empty days stay empty. RD stays; weeks that are SP on every working day are never changed, other SP days can move. Option **West Brom Special**: where possible, an early-finish duty before rest days and a later-start duty after rest days.
  - **BHM (Bank Holiday Monday)** makes a copy of the rota job in which Monday runs the Sunday duties; the normal rota job is not changed. In every sub rota the Monday duties come off; Monday RD and SP stay. A week with a Sunday duty works the same Sunday duty on Monday, or the most similar free one (closest sign-on) when the minimum rest to Tuesday does not allow it. **Walsall rule**: a week with Sunday RD gets SP on Monday. **West Bromwich rule**: the Sunday duties left over go to weeks with Sunday RD that work Tuesday (closest to their own Monday sign-on, first those of the same sub rota, then other Sunday duties within its subset, the Sunday duty numbers set in Sub rota settings: Soho duties on SOHO, Oldbury duties on OLD), and a week with Tuesday RD, or when the duties run out, gets SP. Each Sunday duty is used once. The message lists Sunday duties not worked on the Monday. The copy can be saved for OmniROTA like any rota.
  - **Rota statistics**: total lines, shortest/longest/average week, short weeks and make up time, long weeks and excess hours, expected/worked/roster hours, rest, standby and incomplete weeks, plus a per-day count and the list of problems.
- **Week types**: under each line number choose Any, Mix, Early, Midday, L/M or Late (or set all lines at once in Sub rota settings). Any: no limit · Mix: E, M and no type · Early: E · Midday: M and no type · L/M: L and M · Late: L. Splits (S) only in Any, Mix, Early and Midday weeks. **Sunday:** only L duties follow the week type (Late, L/M or Any weeks); every other Sunday duty may go in any week, and only the 11:00 rest is checked. Balance weeks and Auto-fill go by the duty type given to each duty (from OmniROTA, *Tag & type* or your own types), never by its times, and never put a duty into a week of the wrong type: if there are not enough duties of a type, the day stays empty and the duties left over are listed. A duty that does not match is marked ≠. **Week types…** also shows the duties of the rota counted by type for each day and suggests how many Early, Midday and Late weeks they can fill: a week can only be Early if every day it works has an E duty for it (so Sundays with few E duties limit the Early weeks to lines with Sunday off). The lines are chosen so that a Late week is never followed by an Early week that works Sunday (no 11:00 rest from Saturday night to Sunday morning), and Early after Midday is avoided where possible. *Use these numbers* sets those lines, the rest Any. **Week types…** (Rota lines toolbar) sets many lines at once: every few lines from a start line (e.g. 2, 7, 12, 17 … as L/M) or a list such as `2, 7, 12-15`. Week types are saved in the browser, not in the RIF file.
- **Split duties (S)**: only in Any, Mix, Early and Midday weeks (never Late or L/M), and at most one split in any 3 consecutive weeks (lines in rota order), set per sub rota in Sub rota settings; 0 means no limit, the default for SCH (school and split). 
- **Duty types**: click the type letter in the duty list to set E, M, L, S or no type for one duty, or use the **Tag & type** tab: tick duties (Shift-click for a range, *Tag all shown*), choose a type and *Assign to tagged*. *Auto* goes back to the type worked out from the times (thresholds in Main rota settings).
- **Duty type rules** (Main rota settings): S = split (break of 2:00 or more), E = signs off before 15:00, L = signs off from 22:01 (or after midnight), M = signs off from 19:30, otherwise no type. All three times can be changed.
- **Your own duty types** (Main rota settings → *Your own duty types*): add a code (1–3 letters, e.g. N), a name, what it *counts as* for week types and the split rule (E, M, L, S or no type), and optionally sign-on and sign-off time ranges. With times, matching duties get the type automatically (checked before E, M, L and S); without times you give it to duties by hand or with *Tag & type*.
- **Sub rota settings**: name, short code, start week, minimum and maximum week value, stand-by pay for SP and rest days, minimum daily rest, and whether the rota moves up, down or is static.
- **Main rota settings**: depot code, start date, rest day and stand-by codes, filename, E/M/L type times, and whether rest day pay counts in the weekly total.
- **New sub rota** builds lines from the free duties of WB, Soho, Bearwood or Oldbury.
- **Save DIF** saves a copy of the loaded duty file (.DIF) with the day flags (`,,,NO,YES,…`) taken off every D line. Sunday, Mon–Thu, Friday and Saturday duties all stay; a duty that is identical on Mon–Thu and Friday (e.g. Oldbury 70xx) is written once.
- **Save for OmniROTA (.ORD)** writes the whole rota back as an OmniROTA data file named after the original with ` ROCR` added (e.g. `WB GARAGE 131226 ROCR.ORD`): all sub rotas and lines, their settings, and the duty table from the original file with any duty types you changed. It is available for rotas opened from an OmniROTA folder. Open it in OmniROTA on a copy first.
- In **Balance weeks**, *Only these lines* (e.g. `1-10, 15, 22`) balances just those weeks among themselves and leaves the rest untouched. *Don’t balance these lines* (e.g. `3, 7, 40-45`) keeps those weeks exactly as they are and balances all the others; both can be used together. **Keep** (tick box under the week type of a line in the grid) tags a week so Balance weeks never changes it; the tag is saved with the rota job.
- **Save RIF** writes the file in the same format for importing back into OmniROTA. Keys: arrows, Enter to edit, R, S, Delete, Ctrl+Z.

## Rules checked (defaults, editable under **Rules**)

| Duty type | Days | Compass | Relief |
|---|---|---|---|
| Standard | Mon–Sat | max 8:30 | min 0:38 |
| Split | Mon–Sat | 8:31 – 11:30 | min 2:00 |
| 4 Day | Mon–Sat | min 9:00 | min 0:38 |
| Standard | Sunday | max 7:00 | min 0:38 |
| 4 Day | Sunday | min 8:00 | min 0:38 |

- No splits on Sunday.
- Time on bus: straight-through duties (no relief) max 5:20; all other duties max 5:00 per piece.

How the duty type is identified:

- **Straight through**: no relief.
- **4 Day**: by duty number, set in the **4 Day duties** panel for each garage. West Bromwich: `2401-2499` (Mon–Thu), `F2401-F2499` (Fri), `5401-5499` (Sat), `790-799` (Sun).
- **Split**: any other duty with compass over 8:30 (Mon–Sat).
- **Relief** is the longest break in the duty. With three pieces, the short break is usually just a bus changeover.

The type can be changed by hand for any duty in the table.

## Publishing with GitHub Pages

1. In the repository go to **Settings → Pages**.
2. Under **Build and deployment**, choose **Deploy from a branch**, branch `main`, folder `/ (root)`, and click **Save**.
3. After a minute the tool is available at `https://kieras46.github.io/Crew-duty-analysis/`.
