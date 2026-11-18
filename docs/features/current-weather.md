# Feature Specification: Current Weather

Product requirement: FR-02 · Backlog: [WX-001](../BACKLOG.md) · Priority: Required for v1

## User story

As a visitor, I want to read current conditions for my selected city so I can plan my next activity.

## Behavior

Request weather by the selected geocoding latitude/longitude, with `units=metric`. Preserve the selected city's label. Normalize and validate the response before rendering or saving it; use the [domain model](../ARCHITECTURE.md#data-contracts).

| ID | Acceptance criterion |
| --- | --- |
| W-01 | Selecting a city shows loading feedback, then a card with city/state when available/country, temperature, conditions with a local icon, feels-like temperature, humidity, wind, and observation time. Missing optional readings show an em dash and missing conditions use a neutral fallback. Invalid core data produces an error and is not saved. |
| W-02 | The card distinguishes current, saved, stale, and not-saved states. “View saved” displays the original snapshot without a request. Its “View current” action uses the normal cache/fetch path. Do not replace saved values silently with newer data. |
| W-03 | Metric is the default. Switching to imperial converts Celsius to Fahrenheit and m/s to mph locally for both the weather card and History. Persist the unit preference when storage works; switching units makes no API request and does not create a History record. |
| W-04 | Convert API `dt` seconds to UTC milliseconds and show its date/time using the returned city's UTC offset, labelled `UTC±hh:mm`. Request receipt time must not replace observation time. Formatting must work when the visitor and selected city have different offsets. |
| W-05 | Selection, Refresh, and View current reuse a same-city cache entry younger than ten minutes, with an accessible “Using recent weather” update. At exactly ten minutes or when missing, make one new request. Mark stale readings visibly once this age is reached; do not poll or retry automatically. |
| W-06 | Missing configuration, 401/403, 429, 5xx, network failure, invalid payload, and ten-second timeout have explicit states. Respect the rate-limit cooldown in ARCHITECTURE.md. A failed request preserves only same-city saved data with a saved/stale label; otherwise show an error without another city's readings. |
| W-07 | A newer city selection or saved-view intent invalidates and aborts pending weather work. Late, aborted, or timed-out responses must not change the active card, weather cache, or History. Duplicate requests for the same active action are suppressed. |
| W-08 | A successful active network response enters the bounded cache and is offered to History under H-01/H-02. Display linked OpenWeather attribution. There are no weather requests on initial load, no geolocation prompt, no backend route, and no paid or historical endpoint. |

## Verification examples

- Return a fixture for London at 18°C and 4.1 m/s; imperial shows 64°F and approximately 9.2 mph using the chosen display rounding.
- Omit feels-like, humidity, or wind while retaining valid core data; render placeholders rather than zero.
- Advance a fake clock to 599,999ms and then 600,000ms after receipt; verify cache reuse then one new request.
- Complete city A after the visitor selects city B; neither the card nor History may accept A's obsolete response.
- Open saved data while a live request is pending; its late response must not replace the saved view.
- Format observations near midnight with positive/negative offsets, including a half-hour offset; show the correct city-local date.
- Test all documented failure states and verify they never append History.

Related contracts: [City search](city-search.md), [History](history.md), [UI templates](../DESIGN.md).
