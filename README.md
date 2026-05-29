# Hindbio (GitHub Pages + Google Sheets)

This site is static and can be hosted on **GitHub Pages** (no server).

The enquiry form saves submissions into **Google Sheets** via a **Google Apps Script Web App**.

## 1) Create the Google Sheet

1. Create a new Google Sheet (name it anything, e.g. `Hindbio Enquiries`).
2. Optional: create a tab named `Responses` (the script will create it if missing).

## 2) Create the Apps Script (Form → Sheet)

1. In the Google Sheet: **Extensions → Apps Script**
2. Paste this code:

```javascript
function doPost(e) {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const sheet = ss.getSheetByName('Responses') || ss.insertSheet('Responses');

  const headers = ['submittedAt','name','mobile','industry','fuel','message','page'];
  if (sheet.getLastRow() === 0) sheet.appendRow(headers);

  const body = JSON.parse((e && e.postData && e.postData.contents) || '{}');
  sheet.appendRow([
    body.submittedAt || new Date().toISOString(),
    body.name || '',
    body.mobile || '',
    body.industry || '',
    body.fuel || '',
    body.message || '',
    body.page || ''
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({ ok: true }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

## 3) Deploy the Apps Script as a Web App

1. Click **Deploy → New deployment**
2. Select **Web app**
3. **Execute as**: Me
4. **Who has access**: Anyone
5. Click **Deploy**
6. Copy the **Web app URL**

## 4) Paste the Web App URL into the website

Open `index (1).html` and find:

`const SHEETS_WEB_APP_URL='PASTE_GOOGLE_APPS_SCRIPT_WEB_APP_URL_HERE';`

Paste your Web App URL between the quotes.

## 5) Host on GitHub Pages (no server)

### Easiest: GitHub Desktop (recommended)
1. Install GitHub Desktop
2. **File → Add local repository** (choose this folder)
3. **Publish repository** to GitHub
4. In GitHub (repo page) → **Settings → Pages**
5. **Build and deployment** → Source: **Deploy from a branch**
6. Branch: **main** / folder: **/(root)**
7. Save → wait 1–2 minutes

Your site will be available at the GitHub Pages URL.

## Notes
- `index.html` is a small redirect file so GitHub Pages loads your existing `index (1).html`.
- Form submissions appear in your Google Sheet under the `Responses` tab.

