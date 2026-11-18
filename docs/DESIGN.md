# Weather App UI Design Templates


## Design direction

Use a calm, readable weather dashboard with a pale blue background, white cards, strong navy text, and a blue primary action. Make the selected city, temperature, conditions, and observation time easy to scan. Search comes first; saved History remains visible below or alongside the weather card.

Use one page with no sign-in, navigation drawer, or onboarding flow. Browser permission prompts are unnecessary because v1 uses city search rather than geolocation.

Implement the templates with semantic HTML5 and reusable native `<template>` elements, plain CSS3 with Grid/Flexbox, and JavaScript modules using jQuery for interactions. Render dynamic city and provider text with `.text()` or `textContent`; preserve native controls, focus behavior, and live regions when updating the DOM.

## Design tokens

| Token | Value | Use |
| --- | --- | --- |
| `--color-bg` | `#F3F7FB` | Page background |
| `--color-surface` | `#FFFFFF` | Cards and inputs |
| `--color-text` | `#102A43` | Headings and primary text |
| `--color-muted` | `#486581` | Secondary text and timestamps |
| `--color-primary` | `#175CD3` | Primary buttons and links; white button text |
| `--color-primary-hover` | `#1849A9` | Hover/active primary button |
| `--color-border` | `#CBD5E1` | Decorative card boundaries |
| `--color-control-border` | `#64748B` | Input and interactive boundaries |
| `--color-focus` | `#175CD3` | 3px focus ring with 2px offset |
| `--color-error` | `#B42318` | Errors and destructive text |
| `--color-warning` | `#854D0E` | Saved/stale or storage notices |
| Spacing | `4, 8, 12, 16, 24, 32, 48px` | Consistent layout scale |
| Card radius | `16px` | Weather and History cards |
| Control radius | `8px` | Inputs and buttons |
| Card shadow | `0 8px 24px rgb(16 42 67 / 6%)` | Subtle elevation |
| Font | System UI sans-serif | No external font request |
| Text sizes | Body 16px/1.5; secondary 14px; heading 24–32px; temperature 64px desktop, 48px mobile | Clear hierarchy |

All active controls have a minimum 44×44px target. Verify final color pairings and focus states against WCAG AA during implementation.

## Template A — First visit

```text
+--------------------------------------------------------------------+
| Weather App                                            [°C] [°F]   |
| Current conditions, wherever you are planning to go.                |
|                                                                    |
| City                                                               |
| [ Search city, e.g. London, GB                         ] [ Search ] |
| Add a country code if several cities share the same name.           |
|                                                                    |
| +-------------------------------------+ +------------------------+ |
| | Find your city's weather            | | History                | |
| | Search for a city to get started.   | | No saved weather yet.  | |
| |                                     | | Your viewed weather    | |
| |                                     | | will appear here.      | |
| +-------------------------------------+ +------------------------+ |
|                                    Weather data by OpenWeather     |
+--------------------------------------------------------------------+
```

The city field has a visible label; its placeholder is only an example. Do not make a network request on page load. Existing History is shown on return visits while the weather area keeps the search prompt until the visitor chooses a city or saved observation.

## Template B — City search results

```text
City
[ Springfield                                      ] [ Search ]

Choose a city (up to 5 results)
+--------------------------------------------------------------------+
| Springfield, Illinois, US                             [View weather] |
| Springfield, Massachusetts, US                        [View weather] |
| Springfield, Missouri, US                             [View weather] |
+--------------------------------------------------------------------+
```

Render results as a semantic list of full-width native buttons. Each button includes the complete city label in its accessible name; “View weather” can be a visual cue within the button. Avoid a custom ARIA combobox. Show coordinates as secondary text only when labels would otherwise be identical. A single result still requires selection.

After results load, announce the count in a polite live region. Keep focus on the submit button; the next Tab reaches the first result. After a result is selected, dismiss results and focus the weather heading when the result or error is ready.

## Template C — Current weather on desktop

```text
+--------------------------------------------------------------------+
| Weather App                                            [°C] [°F]   |
| City                                                               |
| [ London, GB                                          ] [ Search ] |
|                                                                    |
| +-------------------------------------+ +------------------------+ |
| | London, GB               [Refresh]  | | History    [Clear all]  | |
| | Current • Observed 14:10 UTC+00:00   | | Saved on this device   | |
| |                                     | |                        | |
| | [cloud icon]  18°C                  | | London, GB       18°C  | |
| | Broken clouds                       | | Nov 18, 14:10 UTC+00   | |
| |                                     | | [View saved] [Remove]  | |
| | Feels like     Humidity     Wind     | |                        | |
| | 17°C           72%          4.1 m/s   | | Paris, FR        15°C  | |
| |                                     | | Nov 18, 14:00 UTC+01   | |
| |                                     | | [View saved] [Remove]  | |
| +-------------------------------------+ +------------------------+ |
|                                    Weather data by OpenWeather     |
+--------------------------------------------------------------------+
```

Use a max-width 1120px content area with 32px desktop padding. At widths ≥960px, use a flexible weather column and a 320px History column with a 24px gap. History rows show the condition description, observation date/time in the saved city's UTC offset, temperature in the selected display units, and an explicit “View saved” action. Add the description on its own line within each row. The wireframe shortens some offset labels; implementation uses `UTC±hh:mm` consistently.

“Current” means the most recent successfully requested or fresh cached observation. The timestamp remains visible, and the card changes to a stale label once the ten-minute cache age is reached. Refresh respects the cache; announce “Using recent weather” when it makes no new request.

## Template D — Mobile

```text
+----------------------------------+
| Weather App           [°C] [°F]   |
|                                  |
| City                             |
| [ London, GB                   ] |
| [            Search            ] |
|                                  |
| +------------------------------+ |
| | London, GB         [Refresh] | |
| | Current • Observed 14:10     | |
| | UTC+00:00                   | |
| |                              | |
| | [cloud] 18°C                 | |
| | Broken clouds                | |
| | Feels like  17°C             | |
| | Humidity    72%              | |
| | Wind        4.1 m/s          | |
| +------------------------------+ |
|                                  |
| History             [Clear all]  |
| Saved on this device             |
| +------------------------------+ |
| | London, GB             18°C  | |
| | Nov 18, 14:10 UTC+00:00      | |
| | [View saved]       [Remove]  | |
| +------------------------------+ |
| Weather data by OpenWeather      |
+----------------------------------+
```

Below 960px, stack weather above History. Below 640px, use 16px page padding, a full-width Search button, and stacked readings. At 640–959px the search input and button can share a row. Support 320px width without horizontal scrolling. Allow long city labels and status timestamps to wrap; do not truncate essential location information.

## Template E — Saved snapshot and deletion

```text
London, GB                                      [View current]
Saved observation • Nov 18, 2026, 14:10 UTC+00:00
18°C • Broken clouds

This is weather you viewed earlier.

Clear history?
Remove all saved weather from this browser. This cannot be undone.
[Cancel]                                      [Clear history]
```

“View saved” opens the original snapshot without a network call and focuses its heading. “View current” explicitly checks the cache or fetches conditions. Keep saved mode visible even when the snapshot is recent. If a displayed observation is deleted, label it “Not saved” while retaining the card in memory.

Removing one row is immediate and announces its removal; move focus to the next row's View saved button or the History heading if none remains. Clear all opens a labelled modal dialog, initially focuses Cancel, traps focus, supports Escape, and returns focus to the trigger or empty History heading after closing.

## Component and state contract

| Component | Required states and behavior |
| --- | --- |
| CitySearch | Idle, invalid input, searching, results, no matches, request error; Enter submits; duplicate submission disabled while pending |
| WeatherCard | Prompt, skeleton/loading, current, saved, stale, not saved, failed; never mix selected city and another city's data |
| UnitToggle | Two labelled native buttons with `aria-pressed`; metric default; convert temperatures/wind in both card and History without a request |
| HistoryList | Empty, saved rows, session-only persistence, removed record; show at most 20 records |
| StatusMessage | Polite loading/success updates; new actionable errors announced with `role="alert"`; text accompanies all icons/colors |
| ConfirmClearDialog | Native dialog or equivalent accessible modal; Cancel and destructive confirmation; focus restored after close |
| Attribution | Persistent linked “Weather data by OpenWeather” footer |

| Situation | User-facing copy | Recovery |
| --- | --- | --- |
| Invalid input | “Enter a city name between 2 and 100 characters.” | Return focus to the city field |
| No results | “No cities found. Try adding a country code.” | Edit query and search again |
| Loading | “Finding cities…” / “Loading weather for London…” | Keep layout stable; no fake readings |
| No configuration | “Live weather is not configured. Please contact the app maintainer.” | Saved History stays usable; setup details belong in README |
| Provider 401/403 | “Weather access is unavailable. Please contact the app maintainer.” | Search/saved history remain available as applicable; no login prompt |
| Rate limit | “Too many weather requests. Try again after [time].” | Live request buttons unavailable during cooldown; manual retry afterwards |
| Network/timeout/5xx | “Couldn't load weather. Try again.” | Retry; same-city saved data stays clearly labelled |
| Invalid provider data | “Weather data is unavailable for this location right now.” | Retry manually or choose another city |
| Blocked storage | “History will only be kept for this session.” | Weather and session History remain usable |
| Corrupt saved data | “Some saved weather couldn't be restored.” | Continue with valid records or empty History |

Disable a pending action's duplicate button, but allow the visitor to change the query or choose another intent. Preserve typed search text through errors. Do not use full-screen spinners or dismissible toasts as the only source of critical information.

## UI review checklist

- Verify first visit, ambiguous results, current, saved, stale, loading, empty, error, and storage-warning states.
- Test at 320px, 768px, and 1280px; include a long city name and 200% browser zoom.
- Use keyboard-only search, selection, refresh, unit conversion, saved view, removal, and clear confirmation.
- Ensure readable contrast, visible focus, meaningful button names, and no live-region announcement on every keystroke.
- Respect `prefers-reduced-motion`; any skeleton or transition must remain understandable without animation.
- Treat all wireframe measurements as UI targets; the feature specifications define acceptance behavior.
