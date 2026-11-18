# Backlog

This backlog intentionally contains one task. The checklists below are acceptance criteria within that task.

## WX-001 — Create the web app with all features

| Field | Value |
| --- | --- |
| Status | To do |
| Priority | P0 — MVP |
| Release | v1.0 |
| Implementation role | `creator` |
| Validation roles | `tester`, `reviewer` |
| Dependencies | Maintainer-provided free OpenWeather API key for the live smoke test |

**User story:** As a visitor, I want to find a city, check its current weather, and revisit weather snapshots saved on my device so I can get useful weather information without an account.

## Feature Requirements

| ID | Feature | Priority | Specification | Status |
| --- | --- | --- | --- | --- |
| FR-01 | Search by city name and choose among matching locations | Required for v1 | [City search](features/city-search.md) | Review |
| FR-02 | Show current weather for the selected city, with units and observation time | Required for v1 | [Current weather](features/current-weather.md) |Review |
| FR-03 | Save successful observations locally, reopen them, and delete them | Required for v1 | [History](features/history.md) |Review |

**Acceptance criteria:**

- [ ] City search meets S-01 through S-07 in [city-search.md](features/city-search.md).
- [ ] Current weather meets W-01 through W-08 in [current-weather.md](features/current-weather.md).
- [ ] Local History meets H-01 through H-08 in [history.md](features/history.md).
- [ ] Desktop/mobile layouts, keyboard access, focus handling, and all loading/empty/error states match DESIGN.md.
- [ ] Requests use only the free Geocoding and Current Weather endpoints, directly from the browser; attribution links to OpenWeather.
- [ ] Missing configuration, invalid credentials, rate limits, offline requests, request races, and storage failures have usable recovery behavior.
- [ ] Only static frontend assets are produced. No backend, proxy, serverless function, hosted database, auth, analytics, or paid API dependency is added.
- [ ] `package.json` implements the development/test command contract in AGENTS.md; the lockfile, placeholder `assets/js/config.example.js`, pinned full jQuery distribution/license, and reproducible setup instructions are included.
- [ ] JavaScript lint, QUnit unit/DOM tests, and mocked browser tests pass. The static `index.html` and `assets/` run without compilation. Evidence covers cache expiry, city disambiguation, saved history after reload, deletion, and late-response protection.
- [ ] Manual checks cover responsive layouts, keyboard use, stable target browsers, and one live free-API smoke test when a key is available. Unperformed checks remain documented and the task stays open.
- [ ] Tester and reviewer findings are addressed; the implementation handoff lists actual checks and any remaining limitations.

**Completion evidence:** Not yet available.
