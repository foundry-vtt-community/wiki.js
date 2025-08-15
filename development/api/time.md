---
title: Time and Calendar
description: In-world timekeeping and calendar configuration
published: true
date: 2025-08-15T22:45:41.373Z
tags: development, api, documentation
editor: markdown
dateCreated: 2025-07-22T19:33:11.382Z
---

![Up to date as of v13](https://img.shields.io/badge/FoundryVTT-v13-informational)

Foundry provides an API to manage timetracking and calendar configuration.

*Official Documentation*
- [API Docs: GameTime](https://foundryvtt.com/api/v13/classes/foundry.helpers.GameTime.html)
- [API Docs: CalendarData](https://foundryvtt.com/api/v13/classes/foundry.data.CalendarData.html)
- [API Docs: CONFIG#time](https://foundryvtt.com/api/v13/variables/CONFIG.time.html)
- [API Docs: CalendarConfig](https://foundryvtt.com/api/v13/interfaces/foundry.data.types.CalendarConfig.html)

**Legend**

```js
CalendarData.formatTimestamp // `.` indicates static method or property
CalendarData#timeToComponents // `#` indicates instance method or property
```

## Overview

Time in a Foundry world is stored as a timestamp and synchronized among users. This value is given a structure and an in-world meaning by the calendar being used, either the built-in Gregorian one or a newly configured calendar provided by the user.

The `game.time` object provides useful properties and methods to retrieve information about the current time, modify it, and interact with it.

The `CONFIG.time` object holds the time and calendar configuration used in the world, which influences how elapsed time translates into days, months, years, or even turns and rounds.

---
## Key Concepts

### Time Setup

The world time is stored in the `core.time` world [setting](/en/development/api/settings), and it represents the amount of seconds elapsed since the beginning. When the [Game](/en/development/api/game) object is set up, a new `GameTime` instance is created as `game.time` and initialized with the calendar configurations found in `CONFIG.time`.

### Time Configuration

The main configurable parts of `CONFIG.time` are the Earth calendar and the world calendar. The first one can be used for IRL timekeeping, while the second one is used to track time in-world.
Foundry provides a "Simplified Gregorian" calendar config out of the box for both, and a `CalendarData` class with helper methods.

### Initialized Structure

```text
CONFIG
├─ time
│  ├─ earthCalendarClass
│  ├─ earthCalendarConfig
│  ├─ worldCalendarClass
│  ├─ worldCalendarConfig
│  ├─ formatters
│  ├─ roundTime
│  └─ turnTime
└─ ...
```

```text
game
├─ time
│  ├─ earthCalendar   <-- instance of CONFIG.time.earthCalendarClass (using earthCalendarConfig)
│  ├─ calendar        <-- instance of CONFIG.time.worldCalendarClass (using worldCalendarConfig)
│  ├─ advance()
│  ├─ set()
│  └─ ...
└─ ...
```

---
## API Interactions

### Configuring a Custom Calendar

The most common interaction with the calendar configuration is defining a custom world calendar. There are two main steps required to do so:

1) Define the calendar.
The module or system will first need to define the calendar configuration. The data structure is described in the [API documentation](https://foundryvtt.com/api/v13/interfaces/foundry.data.types.CalendarConfig.html), but the default found in `foundry.data.SIMPLIFIED_GREGORIAN_CALENDAR_CONFIG` is a good starting point.

```js
const myCalendarConfig = {
    name: "My Custom Calendar",
    description: "A strange calendar with six 60-days months and no Mondays",
    years: {
        yearZero: 1,
        firstWeekday: 1,
    },
    months: {
        values: [
            {name: "Jabruary", abbreviation: "Jab", ordinal: 1, days: 60},
            {name: "Mapril", abbreviation: "Map", ordinal: 2, days: 60},
            {name: "Mayne", abbreviation: "May", ordinal: 3, days: 60},
            {name: "Jugust", abbreviation: "Jug", ordinal: 4, days: 60},
            {name: "Septober", abbreviation: "Sep", ordinal: 5, days: 60},
            {name: "Nocember", abbreviation: "Noc", ordinal: 6, days: 60}
        ]
    },
    days: {
        values: [
            {name: "Tuesday", abbreviation: "Tues", ordinal: 1},
            {name: "Wednesday", abbreviation: "Wed", ordinal: 2},
            {name: "Thursday", abbreviation: "Thu", ordinal: 3},
            {name: "Friday", abbreviation: "Fri", ordinal: 4},
            {name: "Saturday", abbreviation: "Sat", ordinal: 5, isRestDay: true},
            {name: "Sunday", abbreviation: "Sun", ordinal: 6, isRestDay: true}
        ],
        daysPerYear: 360,
        hoursPerDay: 24,
        minutesPerHour: 60,
        secondsPerMinute: 60
    },
    seasons: {
        values: [
          	{name: "Spring", monthStart: 2, monthEnd: 3},
            {name: "Summerfall", monthStart: 4, monthEnd: 5},
            {name: "Winter", monthStart: 6, monthEnd: 1}
        ]
    }
};
```

2) Assign it to `CONFIG.time`.
The `init` hook is an appropriate place to set the calendar configuration.

```js
Hooks.once("init", () => {
	  // ...
    CONFIG.time.worldCalendarConfig = myCalendarConfig;
    // ...
});
```

If a custom calendar class is required, the same logic applies.

```js
class MyCalendarData extends foundry.data.CalendarData {
	// ... any override here
}
```
```js
CONFIG.time.worldCalendarClass = MyCalendarData;
```

### Retrieving Time

`game.time.worldTime` returns the amount of seconds elapsed since the beginning of the world's time.
`game.time.components` returns the same information in the form of an object, describing the current hour, day, month, year, and so on.

### Setting Time

The two main methods to modify the world time are `advance()` and `set()`. They both accept time as their first parameter, either in the form of seconds or in the form of time components.

```js
// The following examples assume a default Gregorian calendar
await game.time.set(0);
console.log(game.time.worldTime); // 0
console.log(game.time.components); // {year: 0, month: 0, day: 0, hour: 0, ...}

// Advance 80 hours
await game.time.advance(80 * 60 * 60);
console.log(game.time.worldTime); // 288000
console.log(game.time.components); // {year: 0, month: 0, day: 3, hour: 8, ...}

// Set a specific date
await game.time.set({year: 2025, day: 45});
console.log(game.time.worldTime); // 63864288000
console.log(game.time.components); // {year: 2025, month: 1, day: 45, dayOfMonth: 14, ...}
```

### Formatters

`CalendarData#format` can be used to display time in a specific preferred format. The base `CalendarData` class provides two formatters: `timestamp` (the default) and `ago`:

```js
game.time.calendar.format({year: 2025, day: 45}); // "2025-02-15 00:00:00"
game.time.calendar.format({year: 2025, day: 45}, "timestamp"); // "2025-02-15 00:00:00"
game.time.calendar.format({year: 2, day: 15, hour: 3, minute: 15, second: 0}, "ago"); // "2 years, 15 days, 3 hours, and 15 minutes ago"
```

---
## Specific Use Cases

### Adding a Date Formatter

Here is a simple example of a custom date formatter:

```js
function formatSimpleDate(calendar, components, {usa=false}={}) {
  if (usa) return `${components.month}/${components.dayOfMonth}/${components.year}`;
  return `${components.dayOfMonth}/${components.month}/${components.year}`;
}
```

`CalendarData#format` looks for available formatters in two locations:
1) Formatter functions defined in `CONFIG.time.formatters`;
2) Named static functions in the CalendarData class.

A user that is already subclassing `CalendarData` will find the easiest option to be adding the above function as a static method:

```js
class MyCalendarData extends foundry.data.CalendarData {
	// ... any override here
  
  static formatSimpleDate(calendar, components, {usa=false}={}) { /* ... */ }
}
```

Alternatively, the method can be added to `CONFIG`:

```js
CONFIG.time.formatters.formatSimpleDate = function formatSimpleDate(calendar, components, {usa=false}={}) { /* ... */ }
```

In both cases, the following will be valid:

```js
const myTimeFormatted = game.time.calendar.format(myTimeComponents, "formatSimpleDate", {usa: true});
```

### Executing Code When Time Is Updated

The [`updateWorldTime`](https://foundryvtt.com/api/v13/functions/hookEvents.updateWorldTime.html) hook is called every time the world time is updated.

```js
Hooks.on("updateWorldTime", (worldTime, delta, options, userId) => {
  console.log(`User ${userId} changed time by ${delta} seconds to ${worldTime}`);
});
```

---
## Troubleshooting
> Stub
> This section is a stub, you can help by contributing to it.