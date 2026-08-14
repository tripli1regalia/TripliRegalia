# Tripli Regalia Mayur Palace — Sales Portfolio Website

Luxury online sales portfolio for **Tripli Regalia Mayur Palace**, Amba Mata Road, Udaipur.
Single-page marketing site targeting MICE groups, destination weddings and milestone celebrations.

---

## Project Structure

```
Tripli-Regalia/
├── index.html           # Entire site (HTML + CSS + JS in one file)
├── images/              # All local media assets
│   ├── *.jpg            # Property, restaurant, café, room and exterior photos
│   └── regalia video 1.mp4  # Hero/showcase video (~4.9 MB)
├── server.js            # Simple Node.js static file server (port 8000)
├── start-server.bat     # Launches server.js (also auto-runs at Windows login via Startup folder)
└── README.md
```

The site is a single self-contained `index.html` — styles and scripts are inline. No build step, no framework, no dependencies.

---

## Page Sections

| Section | Notes |
| --- | --- |
| Offer banner | Sticky "Limited-Time Live Offer" bar with CTA that pre-selects the special offer tag in the enquiry form |
| Header / Nav | Logo links to top (`href="#"`), smooth scroll; mobile fullscreen menu |
| Hero | Static dark background image (`images/Exterior5.jpg`) with gradient overlay for legibility |
| Video showcase | `images/regalia video 1.mp4`, autoplay/loop/muted + controls, `preload="metadata"`, responsive (max-width 900px) |
| Stats strip | Animated counters (rooms / capacities) |
| The Property | About + room/amenity highlights |
| Venue capacity cards | Rooftop Pool & Bar (120), Banquet (80), Fine Dine (100), Poolside/Café |
| Gallery | 26 local images, lazy-loaded, with keyboard-enabled lightbox |
| Enquiry form | Posts to Google Apps Script webhook (lead tracker), then shows on-page thank-you + "Chat on WhatsApp" button |
| Floating WhatsApp | Links to wa.me/919261202317 |
| Google Analytics | GA4 tag `G-SL0F031N0S` in `<head>` |

---

## Lead Tracker — Google Apps Script Integration

The enquiry form (`#inquiryForm`) sends leads to a Google Sheets tracker via a Google Apps Script Web App.

- **Webhook URL** (in `index.html`, JS constant `SHEET_URL`):

  `https://script.google.com/macros/s/AKfycbwCNFaMtLRji4OSbeKafiUZWD4udfVxvM26PI0zLQHxPv-3Kp-D_VAmyG6_H5aZ344Jbg/exec`

- **Method**: `POST` with `mode: 'no-cors'` (fires-and-forgets; the browser cannot read the response by design).
- **Payload**: `FormData` built from the form. Field `name` attributes are the exact keys the script should expect:

  `name`, `phone`, `email`, `eventType`, `eventDate`, `guestCount`, `notes`

- On submit the page never redirects — it prevents the default action, posts the data, then shows an on-page thank-you message and a "Chat on WhatsApp" button (backup contact channel to +919261202317).

> To change the lead destination, update `SHEET_URL` in the JS. If the Apps Script expects different column keys, keep them in sync with the form `name` attributes.

---

## Deployment

### Local preview (dev)
```powershell
# Option 1 — Node server (mirrors the production file structure)
node server.js        # → http://localhost:8000

# Option 2 — just open the file
start index.html      # works too; the Google Sheet POST uses no-cors
```

### GitHub Pages (production)
1. Repository: `github.com/tripli1regalia/TripliRegalia` (branch `main`).
2. GitHub → repo **Settings → Pages** → Source: **Deploy from a branch** → Branch: `main` / root.
3. Live URL: `https://tripli1regalia.github.io/TripliRegalia/`

Changes go live automatically after `git push` once Pages is enabled.

### Deploy updates
```powershell
git add -A
git commit -m "describe change"
git push
```

---

## Handover Notes for the Digital Team

- **Editing the site**: all content/markup/styles/scripts live in `index.html`. Search for section comments like `<!-- ================= HERO ================= -->` to navigate.
- **Images**: keep them under `images/`; the gallery reads from a hardcoded `IMAGES` array in the JS — add new files there.
- **Video**: `regalia video 1.mp4` is ~4.9 MB. For faster mobile loads, re-encode/compress before replacing it.
- **WhatsApp number** `+91 92612 02317` appears in: floating chat button, thank-you button, and the footer contact card.
- **Contacts**: Amit Bhardwaj — `amitbhardwaj@triplihotels.com` · Reservations — `reservation@triplihotels.com`.
- **Analytics**: GA4 measurement ID `G-SL0F031N0S` (Google Analytics 4), configured in `<head>`.
- **Autostart server (local)**: `start-server.bat` is placed in the Windows Startup folder so `node server.js` runs at login — optional, not needed for GitHub Pages.
