# Feature Specification: History

Product requirement: FR-03 · Backlog: [WX-001](../BACKLOG.md) · Priority: Required for v1

## User story

As a returning visitor, I want to revisit weather observations I have viewed and remove them when I choose, without creating an account.

## Scope

History is a collection of snapshots captured through successful current-weather lookups. It is neither a city-name search log nor an archive for arbitrary past dates. The general free OpenWeather API plan is sufficient because all saved data originates from Current Weather responses.

| ID | Acceptance criterion |
| --- | --- |
| H-01 | Save a snapshot only after a valid, successful weather network response still matches the active request and city. Store the selected city, normalized readings, observation/receipt timestamps, and observation UTC offset. Failed searches, failed requests, and cancelled/late responses create no entries. |
| H-02 | Snapshot identity is city coordinate identity plus provider observation timestamp. Repeated responses for the same identity add no row and do not reorder or overwrite the saved observation. Cache-only views, unit changes, and saved-only views add no entries. A genuinely new observation for the same city may add a new row. |
| H-03 | Show at most 20 snapshots, newest saved record first. When a new unique observation exceeds capacity, remove the oldest saved record. Each row shows its city/state/country, observed date/time with city UTC offset, condition description, and temperature in the active display units. |
| H-04 | Persist a validated version 1 envelope at `weather-app:history:v1`. Reload restores the saved rows and unit preference when storage works; it does not automatically fetch weather or choose a city. History stays local to this browser and origin. |
| H-05 | “View saved” opens the exact observation and marks it Saved without a network call. Its “View current” action can reuse a fresh cache entry or fetch current conditions. Reading saved data works offline in an already loaded app. |
| H-06 | Remove deletes one row immediately and persists the change. Clear all requires confirmation; Cancel changes nothing, and confirmation removes this app's history key and weather cache while retaining units. A matching deleted cache entry is evicted. Invalidate pending weather requests for the removed city, or all cities when clearing, so an earlier request cannot recreate deleted records. An already displayed observation can remain in memory labelled Not saved. |
| H-07 | Invalid JSON, unknown schema versions, invalid records, and storage read/write failures do not crash the app. Recover valid records where possible, reset only the affected app key where necessary, and use session memory if persistence is unavailable. Show the explanatory notice defined in DESIGN.md. |
| H-08 | Empty History explains how entries are created. View/remove/clear actions work by keyboard, removal is announced, and focus is retained logically. History exposes no API keys, request URLs, raw payloads, tracking identifiers, cloud sync, or authentication flow. |

## Verification examples

- Fetch the same city/observation twice, then a newer observation; expect one row, then two.
- Save 21 unique observations; verify the oldest is evicted and the remaining order is stable after reload.
- Change units and reopen a snapshot; verify original numeric data and observation identity remain intact.
- Remove one row, reload, and confirm it stays removed. Clear all, reload, and confirm History is empty while units persist.
- Cancel Clear all; verify the rows and cache are unchanged.
- Delete or clear while weather is loading, then resolve the earlier response; it must not recreate deleted records or cache entries.
- Make storage throw, return malformed JSON, and return an unsupported version; verify graceful fallback and notices.
- Confirm opening a saved row makes zero network calls, including while the browser is offline after the app has loaded.

Related contracts: [Architecture storage rules](../ARCHITECTURE.md#cache-and-persistence), [Current weather](current-weather.md), [UI templates](../DESIGN.md).
