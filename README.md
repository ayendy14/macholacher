# לוז חזרות למופע התרמה — Rehearsal Schedule for a Fundraising Show

A small, single-page website for booking rehearsal time in two dance studios ahead of a fundraising show. The interface is in Hebrew and laid out right-to-left.

## What it does

- **Weekly schedule** — shows Sunday to Friday, 08:00 to 23:00, with two columns per day: the large studio (סטודיו גדול) and the small studio (סטודיו קטן). On a phone it shows two days at a time.
- **Booking a rehearsal** — click an empty slot, enter a name, pick a studio, start time, length (30, 45 or 60 minutes) and a colour. Existing bookings can be edited or deleted by clicking them.
- **Booking rules** — a booking cannot overlap another booking or a fixed class in the same studio, cannot run past closing time, and cannot fall on Saturday. Each person is limited to one rehearsal per week.
- **Countdown** — a badge at the top counts down to the show date (default: 11 February 2027).
- **Live updates** — the page refreshes its data every 20 seconds while in use, every minute when idle, and immediately when you return to the tab.

## Admin mode

The faint lock icon in the top corner opens a password prompt. An admin can:

- Maintain the **fixed weekly timetable** (מערכת שעות קבועה) of regular classes, which block those slots every week.
- Create bookings of any length, **lock** bookings so others cannot change them, and bypass the one-per-week limit.
- View the **history** (היסטוריה) of the last 300 changes: who booked, edited or deleted what, and when.
- Change or remove the **show date** used by the countdown.
- **Reset** all rehearsals, optionally with the fixed timetable too. This cannot be undone.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole app: layout, styling and logic in one file. |
| `icon.png` | Icon for the browser tab and phone home screen. |

## Where the data lives

No data is stored in this repository. The page talks to a separate **Google Apps Script web app** backed by a Google Sheet, whose address is set in the `API_URL` line of `index.html`. That script is not part of this repository. It must answer three requests:

- `getAll` — return all bookings, classes and settings (plus history for an admin).
- `write` — save or delete a single record.
- `check` — verify the admin password.

The admin password is checked by that script and is not stored in this repository.

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
| `COLORS` | Colours offered for bookings. |
