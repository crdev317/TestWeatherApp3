# Domain Glossary

## Places

### Location
A named place on Earth with coordinates, such as a town or city, that the user can see weather for. A Location is found by searching its name. There is no automatically detected "my location": every Location comes from a search.
Not the same as a Saved Location. Any Location can be viewed without being saved.

### Saved Location
A Location the user has chosen to keep in their list of places, so they can come back to it without searching again.
Not the Selected Location, although a Saved Location can also be the Selected Location.

### Selected Location
The one Location whose Weather Report is currently shown. It may or may not be a Saved Location. The app reopens on the last Selected Location.

## Weather

### Weather Report
Everything the app shows about the weather at one Location, fetched together at one moment: the Current Conditions, the Hourly Forecast and the Daily Forecast. Every Weather Report carries its Last Updated time.
Not a Forecast on its own: a Forecast is one part of a Weather Report.

### Last Updated
The moment a Weather Report was fetched from the Weather Provider. It is always shown alongside the Weather Report, so the user can judge how fresh the Weather Report is. The glossary has no separate "stale" state; the Last Updated time is the only signal of age.

### Current Conditions
The observed weather at a Location right now: temperature, feels-like temperature, Condition, wind and humidity.
Not a Forecast, which predicts weather at future times.

### Forecast
Predicted weather for a Location at future times. It comes in two forms: the Hourly Forecast and the Daily Forecast.

### Hourly Forecast
The Forecast for each hour of the next 24 hours at a Location.

### Daily Forecast
The Forecast for each of 7 days at a Location: today plus the next 6 days. Each day has a high and low temperature, a Condition and a chance of precipitation.

### Condition
A short, plain-English description of the weather at a given time, for example "Partly cloudy" or "Light rain". It appears in the Current Conditions and in every hour of the Hourly Forecast and every day of the Daily Forecast.

### Weather Provider
The external service the app gets Weather Reports and Location search results from. The Weather Provider is Open-Meteo.

## Preferences

### Unit System
The user's single, app-wide choice between Metric (°C, km/h, mm) and Imperial (°F, mph, in). It applies to every value the app displays. The default is Metric. There are no separate per-quantity unit choices.
