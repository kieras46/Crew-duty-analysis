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
5. **Copy for Google Sheets** copies No, Roll number, Name, Rota, sunday … saturday and weekly pay, ready to paste.

Default rota duty numbers (editable under **Assignment settings**):

| Rota | Mon–Thu | Fri | Sat | Sun |
|---|---|---|---|---|
| WB | 001–099 | F001–F099 | 401–499 | 821–899 |
| Soho | 201–399 | F201–F399 | 501–599 | 701–799 |
| Bearwood | 3001–3099 | F3001–F3099 | 6001–6099 | 7601–7699 |
| Oldbury | 7001–7199 | same as Mon–Thu | 7501–7599 | 9501–9599 |

Oldbury runs the Mon–Thu duties on Friday too, so Oldbury Fridays are planned from the Mon–Thu PDF.

Early means book off by 14:30 and Late means book on from 15:00 (both editable). Routes come from the running board numbers: 12/xx covers routes 12 and 13, 87/xx covers 82 and 87, and any other board runs its own number (74/10 is route 74). A specific route request means every piece of the duty runs on one of the requested routes. Jobs uploaded before this version have no sign-on/off times stored, so upload their PDFs again to use time and route requests.

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
