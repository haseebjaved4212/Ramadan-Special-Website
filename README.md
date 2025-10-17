# Prayer Times — Ramadan Website

Lightweight static web app that shows the current time and date, lets users pick a country and city, and displays prayer times using external APIs.

## Repository structure

- [index.html](index.html) — main HTML layout and UI.
- [style.css](style.css) — all styling and responsive rules.
- [script.js](script.js) — application logic (clock, UI interactions, API calls).
- [Images/](Images/) — background and logo images used by the UI.

## Features

- Analog and electronic clocks (live update).
  - See [`getHourElec`](script.js) for the electronic clock rendering and [`hr` / `mn` / `sc` rotation logic](script.js).
- Country & city picker populated from a public countries API.
  - Populates the country list from `https://countriesnow.space/api/v0.1/countries` (implemented in [script.js](script.js)).
  - UI functions: [`getData`](script.js) and [`getTime`](script.js).
- Prayer times lookup via the Aladhan API.
  - Calendar by coordinates or by city/country: implemented in [`getPostsAdhan`](script.js).
- Search/filter for country and city lists: [`searchCountry`](script.js) and [`searchCity`](script.js).
- Mobile responsive navigation and sections (home / place / prayer-times / contact).

## How to run (development)

1. Clone or copy the project files to a local folder.
2. Open [index.html](index.html) in a browser (no build tools required).
   - The app is static; modern browsers are sufficient.
   - If testing geolocation features, allow location access in the browser.

## Usage

- Home: shows current time (analog + electronic) and date.
  - Electronic time uses [`getHourElec`](script.js).
- Country & City:
  - Click the "country" nav item or open the "country" section to show the country list populated by [script.js](script.js).
  - Click a country to populate the city list (function [`getData`](script.js)).
  - Click a city to fetch prayer times (function [`getTime`](script.js) → [`getPostsAdhan`](script.js)).
- Search:
  - Use the "Find your country" field to filter the country list (`searchCountry`).
  - Use the "Find your city" field to filter the city list (`searchCity`).

## APIs used

- Countries list: `https://countriesnow.space/api/v0.1/countries`
  - Populates countries and their cities.
- Prayer times: `https://api.aladhan.com/v1/`
  - Calls include calendar by coordinates and calendarByCity (see [`getPostsAdhan`](script.js) for implementation details).

## Notes & Known issues (observed in workspace)

The following are current implementation notes and bugs found in [script.js](script.js):

- Date indexing for Aladhan calendar:
  - The code uses array indexing like `prayer.data[date.getMonth() + 1][date.getDay() + 1]` which is likely incorrect: JavaScript `Date.getDay()` returns day-of-week (0–6). Use `date.getDate()` (day of month) and verify how the API returns calendar data (indexing by month/day may need adjustment).
  - See [`getPostsAdhan`](script.js).
- `getHourElec` uses `setInterval` without a delay argument; it should include an interval (e.g., 1000 ms) to update once per second.
  - See [`getHourElec`](script.js).
- Logic bug when checking for empty children:
  - The expression `if (!city.children.length === 0)` is wrong and always true/false unexpectedly. Use `if (city.children.length === 0)` or `if (!city.children.length)`.
  - Search/visibility toggles rely on DOM class toggling in multiple places; consider centralizing state to avoid inconsistent UI states.
- Country/city string handling:
  - The code splits/join manipulations when building city lists are a bit complex and may introduce stray characters. Consider directly using the `post.cities` array rather than converting via join/split/map.
  - See population code in [script.js](script.js).

## Suggested improvements / TODO

- Fix the date indexing and validate returned calendar structure from Aladhan.
- Add error handling and user-facing messages for failed fetches (network or API errors).
- Improve accessibility:
  - Add semantic labels, ARIA attributes, and keyboard navigation for the country/city selections.
- Add unit / integration tests (basic DOM tests) if project grows.
- Cache country list locally (localStorage) to reduce repeated API calls.
- Improve time formatting (pad minutes/seconds to two digits).

## Contribution

- To contribute, open an issue describing the bug or enhancement.
- Fork the repo, create a branch, and submit a pull request.
- Keep changes focused (one issue per PR) and include a short description of the fix.

## License

- Add a license file if you plan to publish. Currently no license file is included.

---

Quick links:
- [index.html](index.html)
- [style.css](style.css)
- [script.js](script.js)
- [Images/](Images/)

Key functions in code:
- [`getPostsAdhan`](script.js)
- [`getTime`](script.js)
- [`getData`](script.js)
- [`getHourElec`](script.js)
- [`clickChoice`](script.js)
- [`searchCountry`](script.js)
- [`searchCity`](script.js)

---
