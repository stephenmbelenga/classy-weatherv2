# Classy Weather (React - Class Components)

Lightweight weather demo built with React class components. This project shows practical examples of:

- React class components and state
- Data fetching (geocoding + forecast)
- Parent → child and child → parent communication
- Component lifecycle methods
- Basic error handling and loading UI

## Quick Start

Install dependencies and run locally:

```bash
npm install
npm start
```

Open http://localhost:3000 to view the app.

## Key Files

- `src/App.js` — main app using class components, includes `Input`, `Weather`, and `Day` classes.
- `src/Appv2.js` — alternative implementation showing constructor/binding and a manual fetch button.
- `src/Counter.js` — example class component demonstrating state updates with functional `setState`.
- `src/starter.js` — helper snippets and commented utilities.

## React Classes

The app is implemented with class components to demonstrate classic React patterns:

- `App` (`src/App.js`): holds top-level state (`location`, `isLoading`, `displayLocation`, `weather`) and orchestrates data fetching.
- `Input`: child component (declared inside `App.js`) that receives `location` and an `onChange` handler from `App` to update the parent state.
- `Weather` and `Day`: presentational children that receive weather data as props and render the forecast.
- `Counter` (`src/Counter.js`): standalone class example showing constructor binding and functional state updates.

## Data fetching

Data is fetched from the Open-Meteo APIs in `App.fetchWeather()`:

1. Geocoding: `https://geocoding-api.open-meteo.com/v1/search?name=...` to resolve the typed location to latitude/longitude.
2. Forecast: `https://api.open-meteo.com/v1/forecast?...&daily=weathercode,temperature_2m_max,temperature_2m_min` to retrieve daily data.

Implementation notes:

- Fetch calls run inside an `async` method on the class (`fetchWeather`).
- `isLoading` state toggles a loading indicator in the UI.
- The code checks for `geoData.results` and throws an `Error("Location not found")` when appropriate.

## Parent–Child Communication

- Child to parent: the `Input` component calls the `onChange` handler passed from `App` to update `App.state.location` (this is how typed input flows up).
- Parent to child: `App` passes `weather` data and `displayLocation` to `Weather`, which in turn passes individual day props to `Day`.
- Data flows down via props; events flow up via callbacks (classic unidirectional React data flow).

## Component Lifecycle

- `componentDidMount()` in `App` reads a saved `location` from `localStorage` and sets it into state.
- `componentDidUpdate(prevProps, prevState)` watches `location` changes to call `fetchWeather()` and persist the new location to `localStorage`.
- `componentWillUnmount()` is hinted (commented) in the `Weather` class as a place to clean up if needed.

These lifecycle methods mirror common `useEffect` patterns but use the class-method form.

## Error Handling

- Fetch operations are wrapped in `try { ... } catch (err) { ... } finally { ... }` blocks. Errors are logged to the console (`console.error`).
- The code throws an explicit error when geocoding returns no results.
- Current UX shows a `Loading...` indicator via `isLoading`, but does not yet display user-facing error messages—this is a recommended improvement.

Suggested error/robustness improvements:

- Show user-friendly error messages in the UI instead of only `console.error`.
- Use `AbortController` to cancel stale requests when `location` changes rapidly.
- Validate and sanitize user input before sending requests.
- Add PropTypes or TypeScript for clearer prop contracts.

## Notes & Next Steps

- `Appv2.js` demonstrates an alternate pattern with a `constructor`, explicit `bind`, and a manual fetch button instead of automatic fetching on update.
- `Counter.js` is a small example of state updates using the functional form of `setState` to avoid stale state.

If you'd like, I can:

- Add UI error messages and an error banner.
- Convert these class components to functional components using hooks.
- Add `prop-types` and simple unit tests for components.
