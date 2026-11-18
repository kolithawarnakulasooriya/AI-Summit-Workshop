# AGENTS.md

## 1. Project Overview

Weather App is a browser-only app for finding a city, viewing its current weather, and revisiting locally saved weather snapshots. Use OpenWeather's free Current Weather and Geocoding APIs. 

### Read first

- [PRD](docs/PRD.md) product requrements are listed here.
- [DESIGN](docs/DESIGN.md) ui design instructions are listed here
- [ARCHITECTURE](docs/ARCHITECTURE.md) architectural design is written here
- [SOURCES](docs/SOURCES.md) sources are written here

[BACKLOG](docs/BACKLOG.md) shows the application implementation information

## 2. Tech Stack

Planned stack; scaffold it when implementing the backlog task:

| Concern | Choice |
| --- | --- |
| UI | HTML5, JavaScript ES modules, and jQuery |
| Development | Static file server |
| Styling | Plain CSS3, Grid/Flexbox, and shared CSS custom properties |
| Requests | jQuery `$.ajax()` with JSON responses, jqXHR cancellation, and request IDs |
| State | Plain JavaScript state objects and focused controllers |
| Persistence | Versioned `localStorage`, with an in-memory fallback |
| Unit/DOM tests | QUnit browser tests with stubbed AJAX, automated through Playwright |
| Browser tests | Playwright, with intercepted API responses |
| Quality | ESLint for JavaScript; HTML/CSS and accessibility review |
| Hosting | `index.html` and `assets/` served directly over HTTPS; no build step |

## 3. Essential Commands

Git Difference check for PR creation

```sh
git status --short
git diff --check
```

## 4. Project Structure

Planned application structure is defined in [ARCHITECTURE](docs/ARCHITECTURE.md#planned-source-layout). 
Do not describe planned source files as implemented.

## 5. Environment

Copy the placeholder-only `assets/js/config.example.js` to ignored `assets/js/config.js` during implementation and set `window.WEATHER_CONFIG.openWeatherApiKey`. Load it before the application entry module. This is browser-readable configuration, not a secret store. No compilation or build command is required.

## 6. Coding Conventions & Patterns

- Use semantic HTML, plain CSS, and small JavaScript ES modules. Use camelCase for functions/variables and kebab-case for filenames and CSS classes. Document object shapes with JSDoc where useful.
- Keep API access in `assets/js/services/`, persistence in `assets/js/storage/`, and request lifecycle logic in controllers. Render validated plain objects rather than raw API payloads.
- Use jQuery for events, DOM updates, and AJAX. Bind handlers once; use delegated `.on()` handlers for dynamic lists. Insert city names and provider text with `.text()` or `textContent`, never as interpolated HTML.
- Validate external JSON and persisted data at the boundary. Render absent optional readings as an em dash, never a fabricated zero.
- Store temperatures in Celsius and wind in metres per second. Convert display units locally; changing units must not fetch or rewrite observations.
- Use the selected geocoding coordinates as city identity. Preserve the selected city's name, state when present, and country in weather and history views.
- Abort superseded requests and reject late results using request IDs. A cancelled or stale response must not update the UI or history.
- Use semantic HTML, explicit labels, keyboard-operable controls, visible focus, and scoped styles using the design tokens.
- Keep timestamps in UTC milliseconds internally. Use the observation's OpenWeather UTC offset when formatting the city's observation time.

## 7. Testing Expectations

- Test observable behavior against the acceptance criteria in `docs/features/`. Mock OpenWeather responses; automated tests must not consume live API quota or require a real API key.
- Cover ambiguous and empty search results, input validation, successful weather, optional fields, request races, timeout, 401/403, 429, network errors, and 5xx responses.
- Verify the ten-minute weather cache, unit conversions, city-local timestamps, history deduplication and capacity, reload persistence, deletion, and malformed or unavailable storage.
- Exercise the complete search → select → weather → history → reload flow in Playwright. Check keyboard navigation and desktop/mobile layouts.
- For documentation-only changes, check links, consistency, formatting, and TOML syntax. Do not invent app test results before the app exists.

## 8. Guardrails (Do's and Don'ts)

- Do use only the two allowed HTTPS endpoints in [ARCHITECTURE](docs/ARCHITECTURE.md#api-contract). Display a linked OpenWeather attribution.
- Do make requests directly from the browser. A development server and static host serve files; they must not proxy weather requests.
- Do handle missing API configuration without a crash. OpenWeather requires a maintainer-provided API key; app users do not create accounts or log in.
- Do keep the real `assets/js/config.js` and API keys out of Git, logs, fixtures, documentation, and screenshots. Commit only the placeholder config example. Browser delivery still makes the configured key publicly inspectable.
- Do retain the same-city saved snapshot with an explicit saved/stale label if a refresh fails. Never show another city's readings under a newly selected city.
- Do keep history on the device and allow deletion. No analytics, tracking, or cloud sync is required.
- Do not add backend routes, serverless functions, proxies, hosted databases, accounts, or auth flows.
- Do not use One Call, Time Machine, paid historical endpoints, or free trials that can incur usage charges. No automatic polling or background retries.
- Do not present saved observations as a continuous historical weather record, forecast, or live data.
- Do not deploy, publish, commit, or push unless requested. Preserve unrelated user changes.

## 9. Git & Definition of Done

Use focused changes and descriptive commits such as `docs: define weather app requirements` or `feat: implement city weather search` when committing is requested. Include the task ID `WX-001` in implementation PRs.

**For documentation:** all requested documents and three persona definitions exist, internal links resolve, the backlog has exactly one task, and planned behavior is consistent across files.

**For implementation: **
- all `WX-001` acceptance criteria pass
- JavaScript lint, QUnit unit/DOM tests, and browser tests succeed
- desktop/mobile and keyboard checks pass; setup instructions and `assets/js/config.example.js` are present
- `index.html` and `assets/` work on a static host without compilation
- Report actual commands, outcomes, and remaining limitations
- Update the backlog to Done only when those requirements are met.
  
## Important
- Backend: Noneno
- User authentication: None
- Use paid endpoints: None
