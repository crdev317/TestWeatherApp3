# PRD — TestWeatherApp3

## Problem Statement

A person who wants to check the weather for the places they care about (home, work, a city they are travelling to) has to open a browser, go to a weather site full of adverts and extra content, and search for each place again every time. When their connection drops, on a train for example, they get nothing at all, not even the forecast they looked at an hour ago. Weather sites also mix up whose clock they use: checking a city in another time zone often shows hours and "today" in the wrong local time.

## Solution

A small, fast Windows desktop app that shows a clean Weather Report for one Location at a time. The user searches for a Location by name, sees its Current Conditions, Hourly Forecast and Daily Forecast, and can keep it as a Saved Location to come back to with one click. Every Weather Report shows its Last Updated time. The app keeps the latest Weather Report for each Saved Location, so it still has something useful to show when it cannot reach the Weather Provider. Forecast times and "today" always follow the Location's Local Time. The app talks to the user only inside its own window, in short, plain, friendly English.

## Requirements

### Finding a Location

1. As a user, I want to search for a Location by typing its name, so that I can see the weather for any place without knowing its coordinates.
2. As a user, I want every search result shown as name, region and country (e.g. "Springfield, Illinois, United States"), so that I can tell apart places that share a name.
3. As a user, I want to pick a search result to make it the Selected Location, so that I can see its Weather Report straight away.
4. As a user, I want to view a Location's Weather Report without having to save it, so that I can check a one-off place without cluttering my list.
5. As a user, I want a friendly message when my search finds no Locations, so that I know to try a different spelling rather than wondering whether the app is broken.
6. As a user, I want a friendly message when a search cannot reach the Weather Provider, so that I know the problem is the connection, not my search.
7. As a user, I want search to wait until I have typed at least two characters, so that I am not shown a flood of irrelevant results.

### Saved Locations

8. As a user, I want to save the Selected Location to my list of Saved Locations, so that I can return to it without searching again.
9. As a user, I want saving a Location that is already a Saved Location to do nothing, so that my list never contains duplicates.
10. As a user, I want to see my Saved Locations as a list, each shown as name, region and country, so that I can switch between them quickly.
11. As a user, I want to select a Saved Location from my list with one click, so that I can see its Weather Report straight away.
12. As a user, I want to remove a Saved Location from my list, so that I can keep my list relevant.
13. As a user, I want removing the Selected Location from my list to only un-save it, leaving its Weather Report on screen, so that tidying my list never unexpectedly clears what I am looking at.
14. As a user, I want my Saved Locations kept in the order I saved them, so that the list stays predictable.
15. As a user, I want my Saved Locations kept when I close and reopen the app, so that I only set up my list once.
16. As a user, I want to see clearly which Location in my list is the Selected Location, so that I always know whose weather I am reading.

### Selected Location and app launch

17. As a first-time user with no Saved Locations, I want the app to open on a friendly search prompt, so that I know exactly what to do first.
18. As a returning user, I want the app to reopen on the last Selected Location, so that I see the weather I care about without clicking anything.
19. As a user, I want the Selected Location's name, region and country shown prominently above its Weather Report, so that I never mistake one place's weather for another's.

### Current Conditions

20. As a user, I want to see the Selected Location's Current Conditions — temperature, feels-like temperature, Condition, wind speed and direction, and humidity — so that I know what the weather is like there.
21. As a user, I want the Condition shown as a short plain-English description (e.g. "Light rain"), so that I understand it at a glance without decoding symbols.
22. As a user, I want Current Conditions presented as of the Weather Report's Last Updated time rather than as "now" when the Weather Report is old, so that I am never misled about how current they are.

### Hourly Forecast

23. As a user, I want an Hourly Forecast for the next 24 hours showing temperature, Condition and chance of precipitation for each hour, so that I can plan the rest of my day.
24. As a user, I want Hourly Forecast times labelled in the Location's Local Time, so that hours for a place in another time zone make sense for that place.
25. As a user, I want hours already past never shown, even when the Weather Report was fetched a while ago, so that I only see forecasts that still matter.

### Daily Forecast

26. As a user, I want a Daily Forecast for today plus the next 6 days, showing high and low temperature, Condition and chance of precipitation for each day, so that I can plan my week.
27. As a user, I want "today" in the Daily Forecast to be the Location's today by its Local Time, so that a city ahead of or behind me shows the right days.
28. As a user, I want days already past never shown, so that an old Weather Report does not show yesterday as today.
29. As a user, I want each day labelled by weekday and date, with "Today" for the Location's current day, so that I can read the Daily Forecast at a glance.

### Last Updated and Refresh

30. As a user, I want every Weather Report to show its Last Updated time in my own clock, so that I can judge how fresh it is.
31. As a user, I want a Refresh to happen automatically when I select a Location, so that I see up-to-date weather whenever I switch places.
32. As a user, I want the Selected Location to be refreshed automatically every 30 minutes while the app is open, so that the Weather Report stays current without my doing anything.
33. As a user, I want to ask for a Refresh myself at any time, so that I can get the latest weather when I need it.
34. As a user, I want to see that a Refresh is in progress, so that I know the app is working and do not click again.
35. As a user, I want a Refresh to apply only to the Selected Location, so that the app does not make network requests for places I am not looking at.
36. As a user, I want a Saved Location I have not selected recently to keep its latest Weather Report until I select it, so that I can still see something for it offline.

### Offline and failure behaviour

37. As a user, I want the latest Weather Report for each Saved Location and the Selected Location kept when I close and reopen the app, so that I have weather to look at even when I start the app offline.
38. As a user, I want the app to show the kept Weather Report, with its Last Updated time and a friendly "can't reach the weather service" message, when a Refresh fails, so that I still have useful information instead of an empty screen.
39. As a user, I want the app to try the Refresh again on the next 30-minute cycle, or whenever I ask, after a failure, so that it recovers by itself when my connection returns.
40. As a user, I want a friendly message and a search prompt, not an error, when I select a Location that has never had a Weather Report and the Weather Provider cannot be reached, so that I understand why there is nothing to show.
41. As a user, I want every error message to say what happened and what I can do next, in plain English with no codes or technical detail, so that I am never left confused.

### Unit System

42. As a user, I want to switch the app between Metric (°C, km/h, mm) and Imperial (°F, mph, in), so that values are shown in the units I think in.
43. As a user, I want Metric to be the default Unit System, so that the app is ready to use without setup.
44. As a user, I want my Unit System choice to apply to every value in every Weather Report at once, so that I never see mixed units.
45. As a user, I want changing the Unit System to update the displayed Weather Report immediately without a Refresh, so that the switch feels instant and works offline.
46. As a user, I want my Unit System choice kept when I close and reopen the app, so that I only choose it once.

### Desktop experience

47. As a Windows user, I want to install the app with a standard Windows installer, so that it behaves like any other desktop app.
48. As a user, I want the app to remember its window size and position, so that it reopens where I left it.
49. As a user, I want the app to start and show my last Weather Report within a couple of seconds, so that checking the weather is quicker than opening a browser.
50. As a user, I want the app usable with keyboard alone (search, select, save, remove, Refresh), so that I can use it without a mouse.
51. As a user relying on a screen reader, I want every Weather Report value and message to have an accessible label, so that the app is usable without sight.

### Privacy and trust

52. As a user, I want the app not to send any usage data, analytics or crash reports anywhere, so that my use of the app stays private.
53. As a user, I want the places I search for and save never written to the app's logs in precise form, so that my locations are not exposed if the logs are shared.
54. As a user, I want the app to need no account, sign-in or API key, so that I can start using it immediately.

## Implementation Decisions

- **The Weather Provider is Open-Meteo, for both Location search (its geocoding service) and Weather Reports (its forecast service).** Its free tier needs no API key, so no secret is handled at all. The free tier is licensed for non-commercial use only; a commercial release needs Open-Meteo's paid, keyed plan, and the key would then be stored in the Windows credential store (Technical-Context, Principle 2).
- **One Weather Report is fetched in a single request per Refresh, covering Current Conditions, 24 hours of hourly data and 7 days of daily data, with timestamps in the Location's own time zone.** A single request keeps a Weather Report internally consistent: all three parts share one Last Updated time.
- **Weather Reports are always fetched and kept in Metric, and converted to the Unit System only for display.** This is what lets a Unit System change take effect instantly and offline, without a Refresh.
- **The Location's time zone is taken from the Weather Provider's search result and forecast response and kept with the Location.** Local Time labels are computed from it, so a kept Weather Report still labels hours and "today" correctly after a restart. Last Updated is displayed in the user's system clock.
- **Past hours and days are filtered out at display time, never at fetch time.** The kept Weather Report is stored whole; what counts as "past" depends on the moment it is displayed, so filtering happens against the current time on every render.
- **Two Locations are the same if the Weather Provider identifies them by the same ID.** This is what "a Location is saved at most once" is checked against; a matching display name alone is not enough, because different places share names.
- **Conditions are derived from the Weather Provider's standard weather codes (WMO weather interpretation codes) through one fixed mapping to plain-English text.** An unrecognised code maps to a neutral fallback ("Unknown conditions") rather than an error, so a new code from the Weather Provider never breaks the display.
- **Every Weather Provider response is validated against a schema before it becomes a Location or a Weather Report.** A response that fails validation is treated exactly like an unreachable Weather Provider: the kept Weather Report stays on screen with the friendly message, and the failure is logged.
- **All Weather Provider calls are made from the main process; the UI receives Locations and Weather Reports over a typed IPC contract and never contacts the network itself** (Technical-Context, Principles 1 and 3).
- **Saved Locations, the Selected Location, the Unit System, the kept Weather Reports and the window position are persisted as local files in the user's app-data folder.** They are small, single-user and local-only; no database is warranted.
- **Kept Weather Reports are limited to the Saved Locations plus the Selected Location.** When an unsaved Location stops being selected, its Weather Report is discarded, so storage never grows without bound.
- **The automatic Refresh interval is 30 minutes, timed from the last successful or attempted Refresh of the Selected Location, and the timer restarts whenever a different Location is selected.** This avoids two Refreshes close together after a manual Refresh or a selection.
- **Search requests are made only after at least two characters and a short pause in typing (about 300 ms).** This keeps the app polite to a free service and avoids flicker.
- **Only one Refresh for the Selected Location is in flight at a time; a Refresh requested while one is running joins it rather than starting another.** If the user selects a different Location mid-Refresh, the stale result is discarded when it arrives.
- **Logging follows Technical-Context: provider calls are logged with endpoint, status and latency, but never with coordinates or search text.**

## Testing Decisions

- **A good test checks external behaviour only:** what a module returns, what it persists, or what the user sees — never its internal state or how it is built. Tests are named in the glossary's language (Weather Report, Selected Location, Refresh, Local Time).
- **Weather Provider adapter — Tier 1.** Tested against recorded Open-Meteo search and forecast responses replayed through `msw`. Covers: responses become correctly shaped Locations and Weather Reports; name/region/country formatting; the time zone carried through; schema-invalid, empty and error responses all surface as a "provider unavailable" outcome, never an exception.
- **Weather Report presenter — Tier 1, pure functions.** Given a Weather Report, the current time and a Unit System, it is tested for: past hours and days dropped; Hourly and Daily labels in the Location's Local Time (including a Location ahead of and behind the user, and across a date line); "Today" resolved by Local Time; Metric↔Imperial conversion of every value; every known weather code mapped to a Condition and an unknown code mapped to the fallback.
- **Location & preference store — Tier 1, real local file I/O in a throwaway temp directory.** Covers: Saved Locations, the Selected Location, the Unit System and kept Weather Reports survive a simulated restart; saving a duplicate Location does nothing; removing the Selected Location only un-saves it; an unsaved Location's Weather Report is discarded when it stops being selected; a corrupt or missing file falls back to the first-launch state.
- **Refresh coordinator — Tier 1, with a fake clock and a fake provider at the seam.** Covers: Refresh on selection, on the 30-minute timer and on request; only the Selected Location is refreshed; concurrent requests join one Refresh; a result for a no-longer-selected Location is discarded; on failure the kept Weather Report stays and the next cycle retries.
- **UI — component tests with Testing Library.** Covers the visible states: first-launch search prompt, search results, no-results and provider-unavailable messages, Weather Report view with Last Updated, Refresh in progress, offline message over a kept Weather Report, Unit System switch, Saved Locations list with the Selected Location marked.
- **IPC bridge and whole app — Tier 2, Playwright driving the built Electron app against live Open-Meteo with a throwaway user-data directory.** One scenario: search → select → save → Unit System switch → restart → the Saved Location, Unit System and kept Weather Report are all still there. This is the real-I/O test on the provider seam and on the main↔UI seam.
- **Prior art:** none yet — this is a new codebase. The first Feature establishes the test harness and fixture recording pattern the others follow.
- **Platform matrix:** Windows only, per Technical-Context.

## Out of Scope

- Automatically detecting the user's position; every Location comes from a search.
- Severe-weather alerts, warnings or notifications of any kind, including Windows notifications.
- Maps, radar and satellite imagery.
- Historical weather beyond the latest Weather Report.
- Air quality, pollen and UV index.
- A system-tray icon or background running when the window is closed.
- Per-quantity unit choices (only one app-wide Unit System).
- Renaming Saved Locations or giving them custom labels.
- Refreshing Saved Locations other than the Selected Location in the background.
- macOS and Linux.
- Accounts, sign-in, sync between devices, and any remote telemetry or crash reporting.
- Commercial distribution (which would require Open-Meteo's paid plan).

## Further Notes

- The vocabulary in this PRD is defined in `Context.MD`; the engineering constraints (stack, security baseline, pinning, logging, testing tiers) are in `Technical-Context.MD`. Where this PRD and either document disagree, those documents win.
- No distribution or release channel exists yet; the installer is built locally until one is chosen.
- This is a multi-feature product; the next step is `/factory-roadmap` to break these Requirements into sequenced Features on the tracker.
