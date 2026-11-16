# WB Crew Duty Check

A single-page tool that reads West Midlands Bus “WB CREW DUTY SUMMARY” PDF files and checks every duty against the crew duty rules.

PDFs are read in your browser with [pdf.js](https://mozilla.github.io/pdf.js/). Nothing is uploaded anywhere.

## How to use

1. Open the page (see GitHub Pages below).
2. Click **New job: choose PDFs** and select one or more duty summary PDFs (for example Mon–Thu, Friday, Saturday and Sunday together).
3. Each upload creates a new **job**. Earlier jobs stay in the **Jobs** list, so you can go back to previous analyses. Click a job name to rename it.
4. **Add PDFs to this job** adds files to the selected job. A file with the same name replaces the old one.
5. Use **Errors only / All**, the type filter and the search box to look through duties. **Download errors (CSV)** saves the errors of the selected job for Excel.

Jobs, rules and manual type changes are saved in the browser you use (local storage). They are not shared between computers or browsers, and clearing your browser data removes them.

## Driver assignment

Open the **Driver assignment** tab to give the duties of the selected job to drivers.

1. Add drivers, or copy the rows from your Google Sheet (columns No to saturday) and use **Paste from Google Sheets**. `RD` in a day column becomes a rest day.
2. For each driver set **Rota** (WB, Soho, Bearwood, Oldbury), **Duty type** (Early, Late, Finish before, Start after, or both), **Special condition** (minimum break or specific routes) and working days.
3. The tool picks a duty for each working day so each driver's weekly pay is as close as possible to the target (default 35:30). Duties can differ from day to day, and duties with duty-check errors can be used (both can be changed in the settings). Drivers with a lower No are served first.
4. Click a duty to change it by hand. Your choice is marked **fixed** and the other drivers are re-planned around it.
5. **Export to rota** sends the result to the Rota creator: choose a rota job and a sub rota (or a new one). Each driver becomes a line with the driver's name under the line number.
6. **Copy for Google Sheets** copies No, Roll number, Name, Rota, sunday … saturday and weekly pay, ready to paste.

Default rota duty numbers (editable under **Assignment settings**):

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

- **Rota jobs**: opening a rota (.RIF) creates a new rota job, saved in the browser with its duties, week types and settings. **Copy job** duplicates the current one: rename it, then **Load duties (.DIF)** to try new duties on the same rota. **New job** starts empty; **Delete job** needs a second click.
- **Duty numbers of each sub rota** (Sub rota settings → *Duty numbers of this rota*): ranges for Mon–Thu, Fri, Sat and Sun, comma-separated. They drive the Subset filter, the list of duties on no rota, Auto-fill and Balance weeks. WB, SOHO, BW and OLD start with the defaults from Driver assignment.
- **Current rota** (top right) picks the sub rota. Each line shows duty number, type (E, M, L, S), sign on and off, and value; the yellow column is the week total, red with the difference when it is under the minimum or over the maximum week value.
- Click a cell, then click a duty in **Duties available for …** on the right to put it there. **All duties / Unassigned** and **Subset** filter the list; **Line** shows where a duty is already used.
- Rest under the minimum daily rest is highlighted on the sign-off and sign-on times, also from Saturday to the next line's Sunday when the rota moves up or down.
- Toolbar: Edit, Clear, Enter 'SP', Toggle 'RD', Insert and Remove lines, and:
  - **Swap days** opens *Rota line check and swap*: two lines side by side, click a day to select it, move the lower line with ▲ ▼ or Jump to, Switch weeks, Cross scrolling, and swap *duties on selected day*, *day and following days*, *duties all week* (RD and SP stay) or *complete weeks*.
  - **Auto-fill** puts duties that are on no rota into empty days (keeping the minimum rest, longest duties to the shortest weeks). If empty days are left with no duty, it asks whether to enter SP.
  - **Balance weeks** rearranges duties so as many weeks as possible are within min–max with the minimum rest; keeps RD and SP.
  - **Rota statistics**: total lines, shortest/longest/average week, short weeks and make up time, long weeks and excess hours, expected/worked/roster hours, rest, standby and incomplete weeks, plus a per-day count and the list of problems.
- **Week types**: under each line number choose Any, Mix, Early, Midday, L/M or Late (or set all lines at once in Sub rota settings). Any: no limit · Mix: E, M and no type · Early: E · Midday: M and no type · L/M: L and M (never splits) · Late: L. Balance weeks and Auto-fill follow the week type; a duty that does not match is marked ≠. **Week types…** (Rota lines toolbar) sets many lines at once: every few lines from a start line (e.g. 2, 7, 12, 17 … as L/M) or a list such as `2, 7, 12-15`. Week types are saved in the browser, not in the RIF file.
- **Split duties (S)**: never on L/M weeks, and at most one split in any 3 consecutive weeks (lines in rota order), set per sub rota in Sub rota settings; 0 means no limit, the default for SCH (school and split). Splits are not limited by the week type, only by this rule.
- **Duty types**: click the type letter in the duty list to set E, M, L, S or no type for one duty, or use the **Tag & type** tab: tick duties (Shift-click for a range, *Tag all shown*), choose a type and *Assign to tagged*. *Auto* goes back to the type worked out from the times (thresholds in Main rota settings).
- **Duty type rules** (Main rota settings): S = split (break of 2:00 or more), E = signs off before 15:00, L = signs on from 15:00, M = signs off from 19:30, otherwise no type. All three times can be changed.
- **Your own duty types** (Main rota settings → *Your own duty types*): add a code (1–3 letters, e.g. N), a name, what it *counts as* for week types and the split rule (E, M, L, S or no type), and optionally sign-on and sign-off time ranges. With times, matching duties get the type automatically (checked before E, M, L and S); without times you give it to duties by hand or with *Tag & type*.
- **Sub rota settings**: name, short code, start week, minimum and maximum week value, stand-by pay for SP and rest days, minimum daily rest, and whether the rota moves up, down or is static.
- **Main rota settings**: depot code, start date, rest day and stand-by codes, filename, E/M/L type times, and whether rest day pay counts in the weekly total.
- **New sub rota** builds lines from the free duties of WB, Soho, Bearwood or Oldbury.
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
- **4 Day**: by duty number. Defaults: `24xx` / `F24xx` (Mon–Fri), `54xx` (Sat), `79x` (Sun).
- **Split**: any other duty with compass over 8:30 (Mon–Sat).
- **Relief** is the longest break in the duty. With three pieces, the short break is usually just a bus changeover.

The type can be changed by hand for any duty in the table.

## Publishing with GitHub Pages

1. In the repository go to **Settings → Pages**.
2. Under **Build and deployment**, choose **Deploy from a branch**, branch `main`, folder `/ (root)`, and click **Save**.
3. After a minute the tool is available at `https://kieras46.github.io/Crew-duty-analysis/`.
