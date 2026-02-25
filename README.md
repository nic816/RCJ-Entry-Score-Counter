# 🤖 RCJ Entry U12 Score Tracker

A professional, cloud-synced scoring dashboard designed for **RCJ (RoboCup Junior Rescue Line) Entry U12 Category**. This tool helps trainers record mock runs instantly on a tablet and sync them directly to a Google Sheet for long-term data analysis.

## 🚀 Live Demo
You can access the dashboard here: 
`https://your-username.github.io/rcap-score-tracker/` 
*(Replace with your actual GitHub Pages link)*

---

## ✨ Key Features

* **Real-Time Scoring:** Automatically calculates points for Gaps (10pts), Intersections (10pts), and Obstacles (20pts).
* **Checkpoint Tracking:** Dynamic segment creator with multipliers for 1st, 2nd, and 3rd attempts.
* **U12 Mission Task:** Dedicated toggle for the Red Line Stop detection.
* **Multi-Device Cloud Sync:** Save a run on an iPad, and it instantly appears on your Laptop via **Google Sheets**.
* **Persistent Session Log:** Data survives page refreshes using a combination of `localStorage` and Google Apps Script.
* **Dark Mode UI:** High-contrast dashboard optimized for low-light training environments or outdoor glare.

---

## 🛠️ System Architecture

1.  **Frontend:** HTML5, CSS3, and Vanilla JavaScript hosted on **GitHub Pages**.
2.  **Bridge:** **Google Apps Script** acting as a REST API to handle `POST` (saving) and `GET` (loading) requests.
3.  **Database:** **Google Sheets** serving as the master data record for all training sessions.

---

## ⚙️ Setup Instructions

### 1. The Database (Google Sheets)
* Create a new Google Sheet.
* Set the headers in Row 1: `Timestamp`, `Team`, `Hazards`, `Tiles`, `RedLine`, `Total`.

### 2. The Bridge (Google Apps Script)
* Go to **Extensions > Apps Script**.
* Paste the provided `doPost` and `doGet` functions.
* Click **Deploy > New Deployment**.
* **Select Type:** Web App.
* **Execute as:** Me.
* **Who has access:** Anyone.
* **Copy the Web App URL.**

### 3. The Frontend (VS Code)
* Open `index.html`.
* Replace `const scriptURL = '...'` with your copied URL.
* Commit and Sync to GitHub.