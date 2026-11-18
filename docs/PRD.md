# Product Requirements Document: Weather App

**Status:** Ready for product review
**Version:** 1.0.0
**Date:** 2026-11-10

## Product Overview

**Product Vision:** Give people a simple way to check a city's current weather and revisit observations they have already viewed. The app runs entirely in the browser with no sign-up or application backend.

**Target Users:** People checking everyday conditions are the primary users. Travellers comparing destinations and returning users revisiting saved observations are secondary users.

**Business Objectives:** Deliver a small, useful weather application with no paid API dependency, minimal operating complexity, and an implementation suitable for this workshop.

**Success Metrics:** Release readiness is measured through acceptance testing and a small usability review, without adding analytics or telemetry.

| Metric | Release target | Measurement |
| --- | --- | --- |
| Core feature completion | All search, current weather, and history acceptance criteria pass | Automated tests and acceptance review |
| Search usability | At least 4 of 5 reviewers find and open a named city within 30 seconds | Timed manual session with responsive API fixtures |
| Local continuity | Saved snapshots survive reload; deletion persists | Browser tests |
| UI accessibility | No serious/critical automated accessibility findings in core states; all primary actions work by keyboard | Accessibility scan and manual check |
| Scope compliance | Zero backend/auth components and zero paid API endpoints | Source and network review |

## User Personas

| Persona | Need | Typical action |
| --- | --- | --- |
| Everyday weather checker | Understand current conditions before leaving home | Search a city and read temperature, conditions, and feels-like temperature |
| Traveller | Find the correct destination when cities share a name | Select a city using its state/country and inspect conditions |
| Returning visitor | Review an observation seen earlier without creating an account | Open a saved snapshot from History |

History is local to the browser and origin. It does not follow a person to another device, browser profile, or host.

## Feature Requirements



**History planning assumption:** History means up to 20 saved weather snapshots collected through this app. Users cannot request dates before their first lookup or fill gaps between observations. The [general free OpenWeather offering](https://openweathermap.org/price) includes Current Weather and Geocoding; the [Weather History API](https://openweathermap.org/api/history) depends on a different subscription. No student-only eligibility or metered One Call product is assumed.

Supporting behavior includes metric/imperial display, loading and empty states, retry controls, a ten-minute current-weather cache, and graceful storage failure. These support the three features; they do not create additional backlog tasks.

Out of scope for v1: forecast, arbitrary past-date weather, maps, geolocation, favourites, notifications, accounts, cloud sync, analytics, backend services, and payment or subscription flows.

## User Flows

1. **First visit:** Open the app → see the city search prompt and empty History → submit a city → choose a location → read current weather → the successful observation appears in History.
2. **Ambiguous location:** Submit “Springfield” → compare returned city/state/country labels → choose a result → fetch by its coordinates.
3. **Return visit:** Open the app → see locally saved History → choose “View saved” → read the original snapshot with a saved label → optionally choose “View current” to check the cache or request current data.
4. **Refresh:** Open current weather → select Refresh → reuse a fresh cache entry or fetch after ten minutes → update the card → save a new observation only when its identity is new.
5. **Failure:** Search or weather request fails → show a specific error and manual recovery action → preserve any same-city saved snapshot with its status label. Failed searches and requests add no History records.
6. **Delete:** Remove one observation immediately, or choose Clear history → confirm → remove all saved history and related local weather cache. The currently displayed card may remain in memory with saving status updated.

## Non-Functional Requirements

| Area | Requirement |
| --- | --- |
| Architecture | Static frontend only; direct browser-to-OpenWeather requests; no application server or user authentication |
| Cost | Only the two allowed free API endpoints; no paid subscription dependency |
| Responsiveness | Usable at widths from 320px upward, with desktop and mobile templates in [DESIGN](DESIGN.md) |
| Accessibility | Target WCAG 2.2 AA for implemented UI: labels, contrast, focus, keyboard access, live request status, and reduced motion |
| Performance | Initial JS target ≤200 KB gzip; acknowledge actions with loading feedback within 100ms; request timeout at 10 seconds. Provider response speed is external. |
| Reliability | No unhandled crash from malformed API data, failed requests, blocked storage, invalid persisted JSON, or missing configuration |
| Compatibility | Verify in current stable Chrome, Firefox, Safari, and Edge at release; automate Chromium E2E for the MVP |
| Privacy | No app tracking or remote history storage. OpenWeather receives the visitor's IP and requested coordinates as part of direct requests. |
| Persistence | Up to 20 snapshots; versioned local storage; session-only fallback with an explanatory notice |
| Freshness | Always show observation time and current/saved/stale state; no automatic polling |
| Offline use | Saved data can be read in an already loaded app; a cold offline launch is not guaranteed because v1 has no service worker |

## Technical Specifications

- HTML5, plain CSS3, JavaScript ES modules, jQuery for DOM/events/AJAX, and `localStorage`. See [ARCHITECTURE](ARCHITECTURE.md) for boundaries and data contracts. No application build step is required; Node.js/npm support development and testing only.
- Resolve names with `GET https://api.openweathermap.org/geo/1.0/direct`; fetch conditions with `GET https://api.openweathermap.org/data/2.5/weather` using latitude/longitude. See the [official geocoding](https://openweathermap.org/api/geocoding-api) and [current weather](https://openweathermap.org/api/current) contracts.
- A maintainer copies `assets/js/config.example.js` to ignored `assets/js/config.js` and sets `window.WEATHER_CONFIG.openWeatherApiKey`. Load this browser configuration before the application entry module. OpenWeather's API key requirement is separate from app user authentication. No visitor key-entry or login screen is required.
- Browser delivery cannot conceal the API key or enforce global account quotas. Use a key for this project, review provider usage, and keep the app within the free plan. Local caching reduces calls but is not account-level enforcement.
- Use a ten-minute cache and explicit request actions, following [OpenWeather's API care guidance](https://openweathermap.org/appid).
- Persist normalized observations without request URLs, API keys, or raw provider payloads. Convert units locally and format observation times with the returned UTC offset.
- Serve `index.html` and `assets/`, including the configured browser file and locally vendored jQuery, directly from a static HTTPS host. No bundling or compilation is required. Hosting selection and publication are future implementation decisions.

## Release Planning

Releases are documented in [RELEASE](docs/RELEASE.md)

| Stage | Deliverable | Exit criteria |
| --- | --- | --- |
| Product review | This PRD, architecture, design, and feature specifications | Scope and the local-history assumption are explicit and consistent |
| Implementation | Implement the code | All feature acceptance criteria implemented |
| Validation | Creator handoff, tester evidence, and reviewer findings | Required checks pass and no unresolved release-blocking defects |
| Release preparation | Refer [Releases](docs/RELEASE.md), Static delivery files and maintainer setup guide | HTML/CSS/JavaScript verified on a static host; provider configuration and limitations documented |


