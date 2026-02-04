# LinkedIn CAPI + GTM Server-Side Lead Demo

This repository contains a **single-page static website** used to **demo and validate LinkedIn’s Conversions API (CAPI) Tag** in a **Google Tag Manager (GTM) Server-Side** implementation.

The page is intentionally framework-free so it can be hosted on **GitHub Pages**, easily inspected in **GTM Preview mode**, and clearly understood by advertisers and implementation partners.

---

## What this demo shows

This demo illustrates a clean, production-aligned event flow from browser → GTM Web → GTM Server → LinkedIn CAPI.

### Core capabilities

- A **modern, client-friendly lead form UI**
- A standards-based `dataLayer` implementation using **Google Common Event–style `user_data`**
- Automatic capture and persistence of the LinkedIn click ID (`li_fat_id`)
- A **sequenced `page_view` event** that fires **after GTM lifecycle events**
- A `lead_submission` event fired on valid form submission
- A live **Debug Panel** that displays the most recent `dataLayer.push()` payload

### Events emitted

#### `page_view`
- Fired **after GTM lifecycle events** (`gtm.js`, `gtm.dom`, `gtm.load`)
- Includes:
  - Page metadata
  - `user_data.user_id` populated from `li_fat_id` (URL param or cookie)

#### `lead_submission`
- Fired on valid form submission
- Includes:
  - Email
  - Name
  - Country
  - Company name
  - Job title
  - `user_data.user_id` (same resolved `li_fat_id`)

---

## LinkedIn click ID (`li_fat_id`) handling

This demo implements LinkedIn’s recommended first-party click ID capture pattern:

- Priority order:
  1. `li_fat_id` URL query parameter
  2. Existing `li_fat_id` cookie
- When present in the URL:
  - The value is stored in a first-party cookie (`li_fat_id`)
  - Cookie lifetime: **90 days**
- The resolved value is:
  - Displayed in the UI
  - Sent as `user_data.user_id` on both `page_view` and `lead_submission`

No hashing or transformation occurs in the browser.

---

## GTM event sequencing (important)

To demonstrate a **correct GTM implementation**, this page ensures that:

- GTM default lifecycle events fire first:
  - `gtm.js`
  - `gtm.dom`
  - `gtm.load`
- Only **after `gtm.load`** is observed does the page push the `page_view` event

This sequencing more closely matches real-world production behavior and avoids firing analytics events before GTM is fully initialized.

### Fallback behavior

If `gtm.load` is never observed (for example, due to ad blockers or a missing GTM snippet):

- The demo will automatically fire `page_view` after a short timeout
- This ensures the page remains usable for demos and testing

---

## How to use

### 1. Add your GTM Web container ID

In `index.html`, replace `GTM-XXXXXXX` with your **Web GTM container ID** in:

- The GTM `<script>` snippet
- The `<noscript>` iframe

---

### 2. Publish to GitHub Pages

1. Push this repository to GitHub
2. Go to **Settings → Pages**
3. Configure:
   - Branch: `main`
   - Folder: `/root`
4. Save and wait for the site to publish

---

### 3. Demo flow

1. Open the site with a LinkedIn click ID, for example:  
   `https://your-site.github.io/?li_fat_id=TEST123`
2. Open **GTM Preview mode** for:
   - Web container
   - Server container
3. Verify in GTM Preview:
   - `page_view` fires after GTM lifecycle events
   - `lead_submission` fires on form submit
4. In **Server-Side GTM**:
   - Confirm the **GA4 Client** receives `user_data`
   - Confirm the **LinkedIn CAPI Tag** maps and sends the data correctly

---

## Debug Panel

The on-page Debug Panel:

- Displays the **most recent** object pushed to `dataLayer`
- Updates automatically when:
  - `page_view` fires
  - `lead_submission` fires
- Allows you to:
  - Copy the JSON payload
  - Clear the panel (UI-only; does not modify `dataLayer` history)

This makes the demo easy to follow during live walkthroughs with advertisers.

---

## Important notes

- No backend is required or used
- No PII is hashed in the browser
- Validation is client-side only
- This is a **demo and testing utility**, not a production lead collection system

---

## References

- LinkedIn CAPI + GTM Server-Side guide  
  https://learn.microsoft.com/en-us/linkedin/marketing/conversions/conversions-api-gtm-guide

- LinkedIn first-party cookies and `li_fat_id`  
  https://learn.microsoft.com/en-us/linkedin/marketing/conversions/enabling-first-party-cookies

---

## License

MIT License. See `LICENSE` for details.
