# The Stand 🍋🧋

A week-1 onboarding game for brand-new BD reps. Run a lemonade stand for a week and
*feel* unit economics — pricing, margin, spoilage, fixed costs, the discount hook, and
the lifetime-value payoff — then discover you've been learning the exact math a merchant runs.

**Play it:** open `index.html` (it's a single self-contained file — no install, works offline),
or visit the GitHub Pages link.

- Guided mode teaches one lever per day on a **scripted weather curriculum** (heatwave on sign
  day, a busy day 6, a viral finale) so every lesson reliably lands; Sandbox unlocks everything
  and keeps the weather seeded-random for replays.
- Predict the crowd, then get told **what actually moved it** (the event, your price, returning
  regulars, the deal) so forecasting becomes a skill, not a guess.
- A closing **"this was your job all along"** panel translates each lemonade fundamental into the
  exact merchant conversation a new BD rep will have.
- Share a **challenge code** so a friend plays your exact week.

`server.js` is only a tiny local static server for development — not needed to play.

## Shared (cohort) leaderboard — optional

Out of the box, records are stored per-browser. To run a **shared board** across an onboarding
cohort, point the game at a tiny Google Apps Script web app:

1. In Google Drive: **New → More → Google Apps Script**. Paste:

   ```js
   const SHEET = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Sheet1");
   function doPost(e){
     const d = JSON.parse(e.postData.contents);
     SHEET.appendRow([new Date(), d.name, d.profit, d.mode, d.code, d.when]);
     return ContentService.createTextOutput("ok");
   }
   function doGet(){
     const rows = SHEET.getDataRange().getValues().slice(1);
     const recs = rows.map(r => ({name:r[1], profit:Number(r[2]), mode:r[3], code:r[4], when:r[5]}));
     return ContentService.createTextOutput(JSON.stringify(recs)).setMimeType(ContentService.MimeType.JSON);
   }
   ```
   (Bind it to a Sheet via **Resources → … / container-bound script**, or create the script *from*
   a Google Sheet so `getActiveSpreadsheet()` resolves.)

2. **Deploy → New deployment → Web app**, execute as *Me*, access *Anyone*. Copy the `/exec` URL.
3. In `index.html`, set `CONFIG.leaderboardUrl` to that URL. Done — the board becomes 🌍 Global,
   and it falls back to the device-local board if the network is unavailable.
