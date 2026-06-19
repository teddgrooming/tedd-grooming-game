# Customer Info Collection — Design

## Purpose
Collect general customer contact info (Name, Phone, Email) from a standalone web page and store it in a Google Sheet as the durable record. Independent of all other pages on the site (no links to/from it).

## Architecture
Static HTML page + Google Apps Script Web App + Google Sheet. No backend server, consistent with the rest of the site.

```
Customer fills form → fetch POST → Apps Script Web App → appends row to Google Sheet
```

## Components

### 1. `Customer Info.html`
- Standalone page, not linked from `Links.html` or any other page.
- Fields: Name, Phone, Email (all required, basic client-side validation — non-empty name, digits-only phone, simple email regex).
- Visual style consistent with existing pages (reuse fonts/colors from `Links.html`).
- On submit: POST to Apps Script Web App URL (stored as a constant in the page's script).
- Success: show inline "Thanks, your info has been received!" message, clear form.
- Failure (network/non-2xx): show inline error message, keep entered values so the user can retry.

### 2. Google Sheet
- One row per submission.
- Columns: `Timestamp`, `Name`, `Phone`, `Email`.
- This is the customer record of truth — viewable/exportable any time via Google Sheets.

### 3. Google Apps Script Web App
- Bound to the Sheet (Extensions > Apps Script).
- `doPost(e)` handler: parses form fields from the request, appends a row with `new Date()` timestamp + Name + Phone + Email.
- Deployed as a Web App (execute as: Me, access: Anyone with the link) to obtain the POST URL used by the HTML page.
- Deployment is done manually by the user in their Google account; I provide the script code and step-by-step deploy instructions.

## Error Handling
- Client-side validation blocks empty/malformed submissions before any network call.
- Network/script errors surface as an inline message; form data is preserved for retry.
- No data is held only in memory — nothing is "lost" since the sheet write is the single source of truth once a submission succeeds.

## Out of Scope
- No authentication/login.
- No duplicate-detection or editing of existing entries.
- No linking from other pages — page is reached only via direct URL.
