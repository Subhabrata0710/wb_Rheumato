# WBSPR Website — Setup & Deployment Guide

Website for the **West Bengal Society of Pediatric Rheumatology (WBSPR)**, including a
dedicated section for the **WBSPR Annual Conference 2026** (23 October 2026, The Park
Hotel, Kolkata). Built with the same architecture as your IAP Howrah project:
static HTML/CSS/JS on GitHub Pages, with Google Sheets + Google Drive as the database
via a Google Apps Script Web App.

## 1. What's real vs. placeholder

- **Confirmed:** Society name, conference name, date (23 Oct 2026), venue (The Park
  Hotel, Kolkata).
- **Placeholder (you must replace):** committee member names/photos, logo, contact
  phone/email, registration fees, sponsors, speakers, scientific program, gallery
  photos, testimonials, domain name.
- Every placeholder is marked in the code as `Dr. Placeholder Name`, `To be announced`,
  `Coming Soon`, or an `// ADMIN:` comment — search for these to find everything that
  needs updating.

## 2. Files

```
index.html        Home page (hero, notice board, carousel, testimonials)
about.html         About WBSPR — mission, vision, history timeline
committee.html      Office Bearers, Organizing, Scientific & Advisory committees
conference.html     WBSPR Annual Conference 2026 hub page
program.html        Scientific programme (placeholder — "coming soon")
speakers.html        Faculty/speakers (placeholder — "coming soon")
gallery.html         Photo gallery with lightbox
register.html         Conference registration form + success/receipt screen
dashboard.html      Delegate dashboard (lookup, QR badge, uploads)
login.html         Delegate/abstract-submission login
venue.html         Venue details + Google Map
sponsors.html       Sponsor tiers (placeholder)
contact.html        Contact form + FAQ
nav.html / footer.html   Shared navigation & footer, loaded by script.js
style.css / portal.css   Design system + delegate portal styles
script.js         All shared front-end logic (see CONFERENCE_CONFIG at the top)
Code.gs          Google Apps Script backend (deploy this in Google Sheets)
CNAME            GitHub Pages custom domain file (placeholder: wbspr.org)
```

## 3. Google Sheets setup

1. Create a new Google Sheet. Copy its ID from the URL
   (`.../spreadsheets/d/THIS_PART/edit`).
2. Create a tab named **`Registrations`** with these headers in row 1:

   `Registration ID | Timestamp | Name | Mobile | Alternate Number | Email | Category |
   Institution | Designation | Registration Fee | Registration Period | Payment Status |
   Payment ID | Is Free | QR Code URL | Notes | Document URL | Action | (Status/QR file id)`

   (Columns T–X — ID/photo, declaration, abstract — are created automatically the first
   time the delegate portal is used.)
3. Create a second tab named **`Contacts`** with headers:

   `Timestamp | Name | Email | Mobile | Subject | Message`

## 4. Apps Script deployment

1. In your Google Sheet: **Extensions → Apps Script**.
2. Delete the default `Code.gs` content and paste in this project's `Code.gs`.
3. At the top of the file, replace:
   - `SHEET_ID` → your Sheet ID from step 3.1
   - `UPLOAD_FOLDER_ID` → a Google Drive folder ID for QR codes / uploads
   - `EMAIL_CC` and `FAILURE_EMAIL` → your real secretariat/admin email
4. **Deploy → New deployment → Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
5. Copy the deployed Web App URL.
6. In `script.js`, set `CONFIG.API_URL` to that URL.
7. If you'll take payments online, replace `CONFIG.RZP_KEY` in `script.js` with your
   real Razorpay key, and fill in `REGISTRATION_FEES` in the same file once fees are
   confirmed (remove `tba: true` for each category once its amount is set).
8. Test: submit a registration, confirm a row appears in `Registrations`, confirm the
   confirmation email + QR code arrive, then test duplicate-registration detection and
   the dashboard lookup (`dashboard.html`).

## 5. GitHub Pages deployment

1. Push all files in this folder to a GitHub repository (root, or a `/docs` folder).
2. Repo **Settings → Pages** → set the source branch/folder.
3. If using a custom domain, replace the placeholder in `CNAME` with your real domain
   and configure DNS (A/CNAME records) at your registrar.
4. Confirm HTTPS is enforced once the domain is verified.

## 6. Content still needed from you

- Real committee names, designations and photos (`committee.html`)
- Logo and hero/gallery photographs (`/images/`)
- Confirmed registration fees for Delegate / PGT / Faculty (`register.html`,
  `script.js`)
- Real phone number, email, and (if available) WhatsApp number — currently
  `+91 90000 00000` / `admin@wbspr.org` placeholders across `nav.html`, `footer.html`,
  `contact.html`, `Code.gs`
- Scientific programme once finalized (`program.html`)
- Speaker/faculty list (`speakers.html`)
- Sponsors (`sponsors.html`)
- A domain name for `CNAME`, if you want a custom URL instead of the default
  `username.github.io` address
