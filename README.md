# LinkedIn CAPI + GTM Server-Side Lead Demo

This repository contains a single-page static website used to **demo and validate LinkedIn’s Conversions API (CAPI) Tag** in a **Google Tag Manager (GTM) Server-Side** implementation.

The page is intentionally simple and framework-free so it can be hosted on **GitHub Pages** and easily inspected using **GTM Preview mode**.

---

## What this demo shows

- A clean **lead form** that captures:
  - First name
  - Last name
  - Email
  - Company name
  - Job title
  - Country (ISO 2-letter)
- A proper `dataLayer` implementation using **Google Common Event–style `user_data`**
- Automatic capture of the LinkedIn click ID (`li_fat_id`) from the URL
- A baseline `page_view` event fired on page load
- A `lead_submission` event fired on valid form submission
- An on-page **Debug Panel** that shows the latest `dataLayer.push()` payload as formatted JSON

This setup is designed to demonstrate how:

1. A **Web GTM container** receives events from the browser
2. A **GA4 Client** in the Server container receives those events
3. The **LinkedIn CAPI Tag** consumes `user_data` from the GA4 event

---

## How to use

### 1. Add your GTM container ID
In `index.html`, replace GTM-XXXXXXX with your **Web GTM container ID** in:
- The GTM `<script>` snippet
- The `<noscript>` iframe

---

### 2. Publish to GitHub Pages
1. Push this repo to GitHub
2. Go to **Settings → Pages**
3. Select:
   - Branch: `main`
   - Folder: `/root`
4. Save and wait for the site to publish

---

### 3. Demo flow
1. Open the site with a LinkedIn click ID: https://your-site.github.io/?li_fat_id=TEST123
2. Open **GTM Preview mode** (Web + Server containers)
3. Verify:
- `page_view` fires on load
- `lead_submission` fires on form submit
4. In Server-Side GTM:
- Confirm the **GA4 Client** receives `user_data`
- Confirm the **LinkedIn CAPI Tag** maps and sends the data correctly

---

## Important notes

- No backend is required or used.
- No PII is hashed in the browser.
- This is a **demo and testing utility**, not a production lead collection system.
- Validation is client-side only.

---

## References

- LinkedIn CAPI + GTM guide  
https://learn.microsoft.com/en-us/linkedin/marketing/conversions/conversions-api-gtm-guide
- LinkedIn first-party cookie and `li_fat_id` capture  
https://learn.microsoft.com/en-us/linkedin/marketing/conversions/enabling-first-party-cookies

---

## License

MIT License. See `LICENSE` for details.

