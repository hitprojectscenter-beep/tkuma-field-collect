# יומן שטח — מנהלת תקומה (tkuma-field-collect)

Mobile GIS field data-collection PWA (Hebrew RTL). Tap a point on the map (or GPS) and record:
location name, reporter name, site type, project status, org unit, Tkuma contact, notes, up to 10 photos.
Viewing is public; adding requires SMS-verified identity (first name, last name, Israeli mobile).
Static site — no build step. Moved here from `Flood-Risk-Assesment-Police-/field-collect` (history preserved).

## Architecture
- `index.html`, `css/style.css` — MAPI customer-portal "Luxury" design (navy/purple/champagne gold, Heebo, Material Symbols)
- `js/config.js` — Supabase keys, map LAYERS, EXTRA_FIELDS, ADMIN_PHONES (CSV/GeoJSON export), CONTACT_PHONE, BRAND/logo
- `js/store.js` — data layer: Supabase (phone OTP auth, `field_points`, `field_options`, Storage `field-photos`)
  with automatic fallback to on-device demo mode (IndexedDB; OTP code shown on screen). Force demo with `?demo`.
- `js/app.js` — Leaflet map, placing, form + autocomplete, details, list/search, admin-only export
- `supabase/field_points.sql` — schema + RLS (public read, owner write) — run once in the Supabase SQL editor
- `sw.js` — offline shell + tile cache. Bump `?v=` in index.html and SHELL version in sw.js on every JS/CSS change.

## Pending external setup (owner)
SQL run in Supabase · Phone provider + test OTPs · real SMS provider (Israeli provider via Send SMS Hook, or Twilio) ·
MAPI orthophoto/buildings service URLs or GovMap token (`LAYERS` in config.js).

## ⚠️ Standing rule — QA at the end of every session
At the end of EVERY session run a comprehensive functional QA pass before finishing:
`python -m http.server 5090 & node qa/qa.mjs` (Playwright, mobile + desktop, demo mode) plus a manual
screenshot review, and report pass/fail results to the user. Extend qa.mjs whenever a feature is added.
(In a sandbox without CDN access set QA_LIBS to a folder with leaflet / markercluster / fonts.)
