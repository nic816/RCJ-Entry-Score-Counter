# RCAP Rescue Line Entry, U12 Score Tracker

A lightweight scoring dashboard for **RCAP Rescue Line Entry, U12** training runs. Coaches can log hazards, checkpoint tiles, and missions during live sessions — all saved to a private Google Sheet in real time.

> ![Dashboard Screenshot](screenshot.png)

---

## ✨ Features

- **Live Scoring Dashboard** — 5-minute countdown timer with color warnings.
- **Hazard Counters** — Gaps (10 pts), Intersections (10 pts), Obstacles (20 pts).
- **Checkpoint Segments** — Dynamic list with tile counts and attempt multipliers.
- **U12 Mission Task** — Red line stop detection toggle (+25 pts).
- **Cloud Saving** — Runs are saved directly to your own Google Sheet and persist on refresh.
- **No Server Required** — Runs entirely in the browser using HTML/JS and Google Apps Script.

---

## 📋 Scoring Rules (RCAP Rescue Line Entry, U12)

| Element | Points |
|---|---|
| Gap navigated | +10 per gap |
| Intersection navigated | +10 per intersection |
| Obstacle avoided | +20 per obstacle |
| Checkpoint tile (1st attempt) | tiles × 5 |
| Checkpoint tile (2nd attempt) | tiles × 3 |
| Checkpoint tile (3rd attempt) | tiles × 1 |
| Red line detected successfully | +25 |

---

## 🚀 Getting Started

This is a single-file web application. To use the cloud-saving feature, you must link it to your own Google Sheet. 

### Step 1: Download the Tracker
1. Download the `index.html` file from this repository.
2. You can open this file in any web browser to use the tracker locally. However, to save data permanently, proceed to Step 2.

### Step 2: Set Up Your Google Sheet & Database
1. Create a new [Google Sheet](https://sheets.new/).
2. In the **first row**, create the following headers exactly as written: 
   `Timestamp` | `Team` | `Hazards` | `Tiles` | `RedLine` | `Total`
3. Click on **Extensions > Apps Script**.
4. Delete any code in the editor and paste the following database script:

```javascript
const SHEET_NAME = 'Sheet1'; // Change this if you rename your tab at the bottom of Google Sheets

// READ DATA (Loads previous runs on page refresh)
function doGet(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
  const data = sheet.getDataRange().getValues();
  const headers = data[0];
  const rows = data.slice(1);
  
  const formattedData = rows.map(row => {
    let obj = {};
    headers.forEach((header, i) => { obj[header] = row[i]; });
    return obj;
  });
  
  return ContentService.createTextOutput(JSON.stringify(formattedData))
    .setMimeType(ContentService.MimeType.JSON);
}

// WRITE OR DELETE DATA (Saves new runs or clears logs)
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
  
  // Handle "Clear Logs" request
  if (e.parameter.action === 'clear') {
    if (sheet.getLastRow() > 1) {
      sheet.deleteRows(2, sheet.getLastRow() - 1);
    }
    return ContentService.createTextOutput(JSON.stringify({ result: 'cleared' }))
      .setMimeType(ContentService.MimeType.JSON);
  }

  // Handle "Save Run" request
  const rowData = [
    new Date().toLocaleString(),
    e.parameter.Team || '',
    e.parameter.Hazards || 0,
    e.parameter.Tiles || 0,
    e.parameter.RedLine || '',
    e.parameter.Total || 0
  ];
  sheet.appendRow(rowData);
  
  return ContentService.createTextOutput(JSON.stringify({ result: 'success' }))
    .setMimeType(ContentService.MimeType.JSON);
}