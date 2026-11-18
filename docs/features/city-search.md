# Feature Specification: City Search

Product requirement: FR-01 · Backlog: [WX-001](../BACKLOG.md) · Priority: Required for v1

## User story

As a visitor, I want to search by city name and distinguish matching places so I can check weather for the location I intend.

## Behavior

Use an explicitly submitted city-search form, followed by a list of selectable locations. Do not fetch on each keystroke or automatically choose the first result. Use the free direct Geocoding endpoint specified in [ARCHITECTURE](../ARCHITECTURE.md#api-contract).

| ID | Acceptance criterion |
| --- | --- |
| S-01 | Trim leading/trailing whitespace. Accept 2–100 characters, including Unicode city names, spaces, punctuation, and optional comma-separated country/state hints. Invalid input shows an inline error and makes no API request. |
| S-02 | Search button or Enter submits one geocoding request with the encoded query and `limit=5`. Typing alone makes no request. Duplicate submission is disabled while that request is pending. |
| S-03 | Show at most five valid candidates as native result buttons with name, state when present, and country. Use coordinates to distinguish otherwise identical labels. Discard invalid coordinates and duplicate coordinate identities. Selecting any result, including a sole result, passes the full selected city to the weather feature. |
| S-04 | An empty valid result array displays “No cities found. Try adding a country code.” and makes no weather request. A malformed response is a request error rather than a valid empty result. |
| S-05 | Announce loading and result count. All results work with Tab and Enter/Space. Preserve typed input on failure. After selection and weather completion/error, focus the weather heading or relevant error. |
| S-06 | Editing the query invalidates old result choices; a newer submission aborts the older search. A late response for an old query must not replace current results. Intentional cancellation does not show an error. |
| S-07 | Missing configuration, 401/403, 429, network failure, 5xx, and ten-second timeout have the messages/recovery defined in [DESIGN](../DESIGN.md). No failed search adds History, and no weather is fetched for an unselected result. |

## Verification examples

- Search “London, GB”; confirm selection preserves its returned label and coordinates.
- Search “Springfield”; choose a non-first result and verify the weather request uses that result's coordinates.
- Search “São Paulo” and a city with a hyphen; ensure no ASCII-only validation blocks them.
- Submit empty/one-character/101-character text; verify zero outgoing requests.
- Resolve two search responses in reverse order; only the latest query's candidates appear.
- Return no matches, malformed JSON, 401, 429, timeout, and offline failures; verify recovery and an unchanged History.

Related contracts: [Current weather](current-weather.md), [UI templates](../DESIGN.md).
