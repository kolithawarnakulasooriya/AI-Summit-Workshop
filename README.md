# Weather App

A planned browser-only weather app using OpenWeather's free APIs. Users can search for a city, see current conditions, and revisit weather snapshots saved on their device. No application backend or user login.

**Status:** Planning complete. The application has not been implemented.   
**Implementation stack:** HTML5, CSS3, JavaScript ES modules, and jQuery.

## Project documents

| Document | Purpose |
| --- | --- |
| [AGENTS.md](AGENTS.md) | Project instructions, conventions, commands, and completion criteria |
| [PRD](docs/PRD.md) | Product vision, users, requirements, and release plan |
| [Architecture](docs/ARCHITECTURE.md) | Browser architecture, OpenWeather integration, and local data |
| [Design](docs/DESIGN.md) | Desktop/mobile UI templates, components, and states |
| [Backlog](docs/BACKLOG.md) | One open task: build the web app with all three features |
| [City search specification](docs/features/city-search.md) | Search and location selection acceptance criteria |
| [Current weather specification](docs/features/current-weather.md) | Readings, units, refresh, and error handling |
| [History specification](docs/features/history.md) | Saved snapshots, persistence, and deletion |

## API and History scope

The plan uses the [Current Weather API](https://openweathermap.org/api/current) and [Geocoding API](https://openweathermap.org/api/geocoding-api), which OpenWeather lists in its [free-for-everyone offering](https://openweathermap.org/price). History is locally saved observations from successful lookups. Arbitrary past-date weather retrieval is outside this scope because the general free offering does not include the [Weather History API](https://openweathermap.org/api/history).
