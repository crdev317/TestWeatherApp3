# TestWeatherApp3

A small Windows desktop app that shows the weather for places the user searches for and keeps.

## Language

### Places

**Location**:
A named place with coordinates, found by searching, and always shown as name, region and country (e.g. "Springfield, Illinois, United States").
_Avoid_: City, place, town

**Saved Location**:
A Location the user has kept in their list so they can return to it without searching again.
_Avoid_: Favourite, bookmark

**Selected Location**:
The one Location whose Weather Report is currently shown, whether or not it is a Saved Location.
_Avoid_: Current location, active location

**Local Time**:
The clock time at a Location, in that Location's own time zone — not the user's.
_Avoid_: Location time, destination time

### Weather

**Weather Report**:
The Current Conditions, Hourly Forecast and Daily Forecast for one Location, fetched together at one moment.
_Avoid_: Weather data, forecast (for the whole bundle)

**Last Updated**:
The moment a Weather Report was fetched from the Weather Provider, in the user's own clock — the only signal of its age.
_Avoid_: Stale, expired, fetched-at

**Refresh**:
Fetching a new Weather Report for the Selected Location, replacing its previous one — only ever for the Selected Location.
_Avoid_: Reload, sync, update

**Current Conditions**:
The observed weather at a Location as of its Weather Report's Last Updated time — temperature, feels-like temperature, Condition, wind and humidity — not necessarily the weather right now.
_Avoid_: Now, current weather

**Forecast**:
Predicted weather for a Location at future times, as either an Hourly Forecast or a Daily Forecast.
_Avoid_: Prediction, outlook

**Hourly Forecast**:
The Forecast for each hour of the 24 hours after its Weather Report's Last Updated time, labelled in the Location's Local Time; hours already past are never shown.

**Daily Forecast**:
The Forecast for each of 7 days — today in the Location's Local Time plus the next 6 — with a high and low temperature, a Condition and a chance of precipitation; days already past are never shown.
_Avoid_: Weekly forecast, extended forecast

**Condition**:
A short plain-English description of the weather at a given time, such as "Partly cloudy" or "Light rain".
_Avoid_: Weather code, summary, icon

**Weather Provider**:
The external service that supplies Weather Reports and Location search results — Open-Meteo.
_Avoid_: API, backend, data source

### Preferences

**Unit System**:
The user's single app-wide choice of Metric (°C, km/h, mm) or Imperial (°F, mph, in), Metric by default.
_Avoid_: Units setting, measurement preference

## Relationships

- A **Saved Location** is exactly one **Location**; a **Location** is saved at most once.
- There is at most one **Selected Location** at a time — none only before the user first picks a **Location** — and it may or may not be a **Saved Location**.
- Removing a **Saved Location** only un-saves it; if it is the **Selected Location**, it stays selected.
- A **Weather Report** belongs to exactly one **Location** and carries exactly one **Last Updated** time.
- Each **Saved Location** and the **Selected Location** has at most one latest **Weather Report**, which is kept when the app closes and reopens.
- A **Refresh** applies only to the **Selected Location**; other **Saved Locations** keep their latest **Weather Report** until they are selected.
- A **Weather Report** contains one **Current Conditions**, one **Hourly Forecast** and one **Daily Forecast**.
- Every **Current Conditions**, hour of an **Hourly Forecast** and day of a **Daily Forecast** has one **Condition**.
- Every **Location** has one **Local Time**; the **Hourly Forecast** and **Daily Forecast** follow it, while **Last Updated** follows the user's clock.
- The **Unit System** applies to every value in every **Weather Report**.

## Example dialogue

> **Dev:** "The user searched 'Springfield' and picked the Illinois one — is that a **Saved Location** now?"
> **Domain expert:** "No — it's the **Selected Location**. It only becomes a **Saved Location** if they choose to keep it, and saving it again does nothing."
> **Dev:** "And if the **Weather Provider** can't be reached when they reopen it?"
> **Domain expert:** "Show the **Weather Report** we have, with its **Last Updated** time. There's no separate 'stale' state."

## Flagged ambiguities

- "Current location" could mean a device-detected position — resolved: there is no auto-detection; the term is **Selected Location**, and every **Location** comes from a search.
- "Forecast" was used for both the whole bundle and the predicted part — resolved: the bundle is a **Weather Report**; a **Forecast** is only the predicted part.
- "Today" could mean the user's day or the Location's — resolved: it is the Location's day, by its **Local Time**.
- "Current Conditions" suggested "right now" even for an old **Weather Report** — resolved: they are the conditions as of **Last Updated**, and are presented that way.
- Two places can share a name — resolved: a **Location** is always identified by name, region and country, and two Locations are the same when the **Weather Provider** says they are.
- Severe-weather alerts, maps or radar, historical weather, and air quality, pollen or UV were considered — resolved: all are out of scope and have no terms here; none is part of a **Weather Report**.
