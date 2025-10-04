<div align="center">

<h1>PlaceMate Schedule</h1>

<p>Turn placement emails into calendar events — automatically.</p>

<p>
  <img alt="Status" src="https://img.shields.io/badge/status-active-brightgreen" />
  <img alt="Platform" src="https://img.shields.io/badge/platform-Google%20Apps%20Script-blue" />
  <img alt="License" src="https://img.shields.io/badge/license-MIT-lightgrey" />
</p>

</div>

---

## 🌟 Overview
Placement teams often send schedules as emails with Excel attachments. Manually finding your name, extracting venue/date/time, and creating a calendar event is tedious.

PlaceMate automates this using Google Apps Script:
- **Reads unread** placement emails
- **Converts Excel** attachments to Google Sheets
- **Searches for your name/reg number** in the sheet
- **Extracts date, time, and venue** from subject/body/lists
- **Creates a Google Calendar event** with reminders

---

## ✨ Features
- **Zero maintenance**: runs on a time-driven trigger
- **Robust parsing**: supports multiple date formats and times like “5 pm” or “5:30 pm”
- **Smart venue detection** from dedicated venue lists or general lists
- **Duplicate protection** using in-memory event keys
- **Automatic cleanup** of temporary Drive files

---

## 🧩 How it works
1. Search unread emails from/to your placement cell within the last 30 minutes
2. For each matching email, convert Excel attachments to Google Sheets
3. Scan rows to see if your `my_name` or `my_reg_number` appear
4. Extract date/time from subject/body; detect venue from sheets or fall back to “TBD”
5. Create a Calendar event with reminders (60 min and 15 min before)

---

## 🚀 Quick Start (Google Apps Script)
1. Open `https://script.google.com` → New project
2. Paste `server.js` code into the default script file
3. Replace placeholders in code:
   - `const my_name = "Your_Name"`
   - `const my_reg_number = "Your_Registration_Number"`
   - In the Gmail query, replace `placement_cell_email` with the real address
4. Enable Advanced Drive Service:
   - Sidebar → Services (+) → add “Drive API”
   - In Project Settings → GCP Project → ensure “Google Drive API” is enabled
5. Run → select `myFunction` → grant permissions (Gmail, Drive, Calendar)
6. Triggers → Add Trigger → Function: `myFunction` → Time-driven → Every 15–30 minutes

---

## 🛠️ Optional: Local workflow with clasp
If you prefer editing locally and pushing to Apps Script:

```bash
npm i -g @google/clasp
clasp login
clasp create --type standalone --title "PlaceMate Schedule"
# Replace generated code with your local server.js (rename to Code.gs if needed)
clasp push
clasp open
```
Then enable the Drive service, authorize, and add the trigger as above.

---

## ⚙️ Configuration
In `server.js`:
- `my_name` and `my_reg_number`: your details used to match rows
- Gmail query: set the correct `placement_cell_email` in
  `(from:"placement_cell_email" OR to:"placement_cell_email") is:unread`

Tips:
- Keep target emails unread until processed
- Attachments should be `.xlsx`/`.xls` or Google Sheets
- Subject should include keywords like `talk`, `test`, `process`, `online`, `assessment`, or `exam`

---

## 🔐 Permissions
The script will request:
- Gmail read access (search emails, read attachments)
- Drive access (convert Excel → Google Sheets, temporary files)
- Calendar access (create events)

These are required for full automation.

---

## 🧪 Testing
1. Send yourself a test email with an Excel sheet containing your name/reg number
2. Include a subject like: `Technical Test on 08/09/2025 at 5:00 pm`
3. Click Run on `myFunction` and watch the logs (View → Logs)
4. Check your Google Calendar for the new event

---

## ❓ Troubleshooting / FAQ
- “The script can’t open the sheet” → Ensure Drive API (Advanced Service) is enabled and files are converted using `Drive.Files.insert(..., { convert: true })`.
- “No date detected” → The script defaults to tomorrow 10:00 AM if it can’t parse a date/time.
- “Times like ‘5 pm’ aren’t picked” → Supported. Regex accepts both with and without minutes.
- “Duplicate events” → The script guards against duplicates by combining subject + start time.
- “Wrong placement email” → Update the Gmail query to the correct address in `server.js`.

---

## 📸 Screenshots (optional)
- Apps Script Services setup (Drive API)
- Triggers configuration
- Sample Calendar event created

---

## 📄 License
MIT — feel free to use and adapt.
