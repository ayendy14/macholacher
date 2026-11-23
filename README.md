# לוז חזרות למופע התרמה — Rehearsal Schedule for a Fundraising Show

A small, single-page website for booking rehearsal time in two dance studios ahead of a fundraising show. The interface is in Hebrew and laid out right-to-left.

**Live page:** https://ayendy14.github.io/macholacher/

## What it does

- **Weekly schedule** — shows Sunday to Friday (or Sunday to Saturday, if admin turns Saturday on), 08:00 to 23:00, with two columns per day: the large studio (סטודיו גדול) and the small studio (סטודיו קטן). On a phone it shows two days at a time.
- **Booking a rehearsal** — click an empty slot (the top or bottom half of a slot picks the quarter hour), enter a name, pick a studio, a start time (hour and minutes, in 15-minute steps), a length (30, 45 or 60 minutes) and a colour. A live summary shows the resulting time range. Existing bookings can be edited or deleted by clicking them.
- **Colour ring** — colours are chosen on a ring of bright shades, so bookings stay readable; text on light colours turns dark automatically. Each device remembers the colour its user last booked with.
- **Booking rules** — a booking cannot overlap another booking or a fixed class in the same studio, cannot run past closing time, and cannot fall on Saturday unless admin has turned Saturday on. Each person is limited to one rehearsal per week.
- **Countdown** — a badge at the top counts down to the show date (default: 11 February 2027), in months and weeks, then in days during the last week, with the total number of days shown alongside.
- **Live updates** — the page refreshes its data every 20 seconds while in use, every minute when idle, and immediately when you return to the tab.
- **Safety nets** — a loading screen on first entry, and a warning before closing a booking form that has unsaved changes.

## Admin mode

The faint lock icon in the top corner opens a password prompt. An admin can:

- Maintain the **fixed weekly timetable** (מערכת שעות קבועה) of regular classes, which block those slots every week.
- Create bookings with any start and end time, in black or gray as well as the colour ring, and bypass the one-per-week limit.
- **Lock** a booking (🔒) so others cannot change it.
- **Highlight** a booking (⭐) — it glows on the schedule, useful for run-throughs and other key rehearsals.
- View the **history** (היסטוריה) of the last 300 changes: who booked, edited or deleted what, and when.
- Change or remove the **show date** used by the countdown.
- **Add or remove Saturday** from the schedule. Hiding Saturday keeps its existing bookings and classes; they reappear when it is turned back on.
- **Reset** all rehearsals, optionally with the fixed timetable too. This cannot be undone.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole app: layout, styling and logic in one file. |
| `icon.png` | Icon for the browser tab and phone home screen. |

## Where the data lives

No data is stored in this repository. The page talks to a separate **Google Apps Script web app** backed by a Google Sheet, whose address is set in the `API_URL` line of `index.html`. That script is not part of this repository. It must answer three requests:

- `getAll` — return all bookings, classes and settings (plus history for an admin).
- `write` — save or delete a single record (in `bookings`, `classes`, `history` or `settings`).
- `check` — verify the admin password.

The admin password is checked by that script and is not stored in this repository. After changing the script, publish a new version (Deploy → Manage deployments → Edit → New version) — the address stays the same.

## Running it

There is nothing to install or build. Open `index.html` in a browser, or host the two files on any static web host (for example GitHub Pages). An internet connection is needed to reach the Google Apps Script backend.

## Common adjustments

These settings are near the top of the script section in `index.html`:

| Setting | Meaning |
| --- | --- |
| `API_URL` | Address of the Google Apps Script backend. |
| `DEFAULT_SHOW` | Default show date for the countdown. |
| `SN` | Names of the two studios. |
| `S0`, `S1` | Opening and closing times, in minutes after midnight (480 = 08:00, 1380 = 23:00). |
| `DURS` | Rehearsal lengths, in minutes, offered to regular users. |
| `STEPS` | Number of colours on the colour ring (best as a multiple of 6, between 12 and 36). |
| `SAT`, `LIT` | Saturation and lightness of the ring's colours, in percent. |
| `BLK`, `GRAY` | The two extra colours available to admin only. |
