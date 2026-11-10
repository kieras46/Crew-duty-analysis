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
