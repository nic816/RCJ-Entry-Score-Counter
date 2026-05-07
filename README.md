# RCAP Rescue Line Entry, U12 Score Tracker

A lightweight scoring dashboard for **RCAP Rescue Line Entry, U12** training runs. Coaches can log hazards, checkpoint tiles, and missions during live sessions — all saved to a private Google Sheet in real time.

![RCAP Rescue Line Entry, U12](https://img.shields.io/badge/RoboCup_Junior-Rescue_Line_U12-00e5ff?style=for-the-badge)

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

### Step 2: Set Up Your Google Sheet
1. Create a new [Google Sheet](https://sheets.new/).
2. In the first row, create the following headers exactly as written: 
   `Timestamp` | `Team` | `Hazards` | `Tiles` | `RedLine` | `Total`
3. Click on **Extensions > Apps Script**.
4. Delete any code in the editor and paste in your Apps Script code (you will need a basic script to accept POST requests and append rows to your sheet).
5. Click **Deploy > New Deployment**.
6. Select **Web App** as the type.
7. Set "Who has access" to **Anyone**.
8. Click Deploy and **copy the Web App URL**.

### Step 3: Link the Tracker to Your Sheet
1. Open your downloaded `index.html` file in any text editor (like Notepad or VS Code).
2. Scroll down to approximately line 351 to find the configuration variable.
3. Replace the placeholder with your copied Google Web App URL:
   ```javascript
   const scriptURL = 'YOUR_GOOGLE_APPS_SCRIPT_URL_HERE';