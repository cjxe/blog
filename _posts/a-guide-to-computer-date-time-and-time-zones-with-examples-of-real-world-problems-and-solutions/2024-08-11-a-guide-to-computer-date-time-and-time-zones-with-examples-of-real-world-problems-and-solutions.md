---
title: "A guide to computer date, time and time zones with examples of real-world problems and solutions"
date: 2024-08-11 17:52:49 +02:00
tags: [tech]
description: A guide to computer date, time and time zones with examples of real-world problems and solutions.
toc: true
---
**Date** (noun): A numbered day in a month, often given with the day name, month, and year. A popular format on computers: `YYYY-MM-DD` (e.g., 2024-07-29).

**Time** (noun): The part of existence that is measured in minutes, days, years, etc., or this process considered as a whole. A popular format on computers: `HH:MM:SS` without days, months or years (e.g., 16:59:25).

# The birth of computer time

As computers evolved from human calculators to digital machines, computer developers needed an efficient way for humans (i.e., especially *programmers*) to interact with these new devices. This led to the creation of operating systems (OS).

With OS-equipped computers, programmers tackled various challenges, including the need to calculate time differences. To solve issues that required calculating time differences, computer developers developed a time-calculation system.

UNIX, an influential early OS, introduced a solution with 2 assumptions to calculate time differences:
1. **Start date**: UNIX computers established a fixed starting point called the UNIX epoch at 00:00:00 UTC on January 1, 1970. This acts as the starting date and time for most computers, similar to how "Year 0" is set in the Gregorian calendar we use today.
2. **Time representation**: UNIX time is measured as the number of seconds (or smaller units like milliseconds or nanoseconds) elapsed since the UNIX epoch. E.g., as I write this sentence within this blog post, the UNIX time is 1723306952, indicating that many seconds have passed since the start of the UNIX epoch. 

Some problems included calculating time differences that included the *current* date and time, so programmers needed to know the current time. But how did the computers know their *current* UNIX time?

**Epoch** (noun): the beginning of a period in the history of someone or something.

**UTC (Coordinated Universal Time)** (noun): a time standard that follows the Greenwich Mean Time (GMT) without Daylight Savings Time (DST) adjustments.

# How computers' know their current time

**Clock** (noun): A mechanical or electrical device for measuring time, indicating hours, minutes, and sometimes seconds by hands on a round dial or by displayed figures.

There are 2 clocks in a computer to track the current time:
1. **Hardware clock** (a.k.a. Real-time clock): The machine's hardware tracks a clock using a battery and circuits. You are encouraged to match this clock to UTC (the reason is explained in the [Best practices when working with date, time and local time in an international app](#best-practices-when-working-with-date-time-and-local-time-in-an-international-app) chapter), but you can set the time zone to your local time (which is the current time in your current time zone). E.g., Windows sets the hardware's clock to the user's local time; whereas MacOS and Linux set their hardware clock to UTC.
2. **System clock**: The system clock is dependent on the hardware clock, and managed by the OS's kernel. The OS reads the hardware clock, but also is in sync with the other clocks on the internet. When an app wants to access your machine's clock, it accesses the system clock (and the `TZ` variable which I will explain later in the [Accessing local time and time zone](#accessing-local-time-and-time-zone) chapter).

# Accessing the system clock in an OS (e.g., MacOS and Linux)

If you are on MacOS or Linux, you may access the system clock by running the `date` command in your bash shell:

```bash
> date
Thu 28 Mar 2024 18:10:10 CET

> date -u 
Thu 28 Mar 2024 17:10:10 UTC

> date +"%s" 
1711645810 # The number of seconds that passed since the UNIX epoch. The `-u` flag is applied by default. Think of this number when you think about the _system clock_.
```

# Accessing local time and time zone

"Internally, your Mac is using UTC. Your Mac is **not** "in CET". It is "in UTC with a default `TZ` setting of `CET`". `TZ` environment variable controls how a local time is derived from the underlying UTC when a local timestamp or timezone offset is required. If you change `TZ`, then local timestamps will be shown in the new timezone, but the computer's internal timekeeping in UTC is not affected[.](https://stackoverflow.com/a/54524475/12959962)"

So, when you run `date`, then the command-line utility reads the system clock and the `TZ` environment variable, and prints the current date including the time zone. If you would like to get the current time zone of your computer, you can run the following command in Linux systems:
```bash
> sudo systemsetup -gettimezone 
Time Zone: Europe/Berlin
```

Further reading: [how do computers store and *update/maintain all the different time zone information to keep up with each country’s ever-changing time zone rule*?](https://sdimitro.github.io/post/keeping-track-time/)

# Accessing the system clock and time zone using a programming language (e.g., JavaScript)

Your favourite browser (and many other apps) reads the system clock and the `TZ` environment variable. Open up your browser's DevTools and navigate to the console. Then, paste the following lines:

```js
> const currentDate = new Date();
> console.log(currentDate)
Thu Mar 28 2024 18:10:10 GMT+0100 (Central European Standard Time) // Similar to running `date` in a bash shell.

> console.log(tempDate.valueOf()) 
1711645810000 // Similar to running `date +"%s"` in a bash shell.

> const tz = Intl.DateTimeFormat().resolvedOptions().timeZone;
> console.log(tz);
Europe/Berlin // Similar to running `sudo systemsetup -gettimezone` in a bash shell.
```

# Some problems and solutions that I came across

**Disclaimer**: I obfuscated sensitive information (such as the name of the market/exchange) to prevent any sensitive information from being leaked.

## Problem #1: Analytics for the Fiji market are 1 hour behind Fiji's current time

**Background**

The user interface (UI) visualises analytics (e.g., using tables, graphs, charts etc.) for a list of markets that the company provides support to. When the UI makes a request to the internal API, then the API responds with the following:
- data related to the query
- metadata
	- current date and time in UTC
	- the time zone of the current market

**Actual/current behaviour**

The analytics for the Fiji market are 1 hour behind Fiji's current time. 

**Expected behaviour**

The analytics for the Fiji market should be up-to-date with Fiji's current time.

**Investigation result**

We use an external library to keep up-to-date with every time zone information in the world. Fiji changed their DST in 2022, but we forgot to install the latest version of the library (which included the latest changes).

**Solution**

We updated our external library that provides time zone information for all countries.

**How to prevent this issue from happening again**

Automatically update the external library that provides time zone information for all countries.

## Problem #2: Relative time difference (e.g., 9 hours ago) is showing the wrong information

**Background**

The UI has a component that displays relative time difference (e.g., 9 hours ago). When the UI needs to display the relative time info, the UI makes a request to the internal API to get the last updated time. Then, the UI finds out the current time, and subtracts the time that is received from the API.

**Actual/current behaviour**

The relative time difference component is showing "-3 hours ago".

**Expected behaviour**

The relative time difference component should show "4 hours ago".

**Investigation result**

The API sends the local time of the market; whereas the the UI expects the time to be sent in UTC time. 

**Solution**

Before calculating the relative time difference, add the time zone offset to the local time.

**Whiteboard solution**

<figure>
	<img src="/a-guide-to-computer-date-time-and-time-zones-with-examples-of-real-world-problems-and-solutions/problem-2-whiteboard-solution.png" alt="Problem 2's whiteboard solution">
</figure>

**Code solution**

From:
```ts
const currentDate = new Date();
```

To:
```ts
const currentDate = new Date(new Date().toLocaleString('en-us', { timeZone: currentMarketsTimeZone }));
```


[Further explanation](https://stackoverflow.com/a/15171030/12959962).

**How to prevent this issue from happening again**

The database should store the time in UTC (instead of local time).

## Problem #3: Discrepancy between Data Manipulation Service Inc. and UI

**Background**

1. The UI has a calendar component that allows the user to select a single day, month and year.
2. The data and network flow throughout the app: UI <--> internal API <-->  Data Manipulation Service Inc. <--> Database

**Actual/current behaviour**

The table in the UI doesn't match the table in Data Manipulation Service Inc..

**Expected behaviour**

The table in the UI should match the table in Data Manipulation Service Inc..

**Investigation result**

Given the user is in Asia/Tokyo time zone,

when the user loads the report,

and set the end date to "10/01/2024",

then the UI sends "10/01/2024 14:59:59 UTC" to the internal API,

then Data Manipulation Service Inc. makes a search query that is 9 hours shorter than expected

**Solution**

When the user selects an end date on the calendar, then the UI should send the end-of-day time in UTC (i.e., 23:59:59 UTC) regardless of the user's time zone (e.g., 23:59:59 UTC when the user in London, also 23:59:59 UTC when the user in Asia/Tokyo) to the internal API.

**Whiteboard solution**

<figure>
	<img src="/a-guide-to-computer-date-time-and-time-zones-with-examples-of-real-world-problems-and-solutions/problem-3-whiteboard-solution.png" alt="Problem 3's whiteboard solution">
</figure>

**Code solution**

From:
```ts
const endOfDayDateTime = endOfDay(dateRange.to);
```
To:
```ts
import { zonedTimeToUtc } from 'date-fns-ts';

const endOfDayDateTime = zonedTimeToUtc(endOfDay(dateRange.to), 'UTC');
```

**How to prevent this issue from happening again**

The UI should send time in UTC to the API (instead of local time).

# Best practices when working with date, time and local time in an international app

These are the best practices that I found to be the most useful/valuable.

1. Use UTC for everything excluding when displaying the time to the user.
2. When displaying the time to the user, indicate the time zone of the displayed time.
3. When displaying the time to the user, display it in their locale format.
4. When manipulating time (e.g., to display in a different format, or to calculate a new time), use libraries.
5. When using libraries, keep them up-to-date (preferably automatically).
6. When storing time in UTC, also store the time zone information (e.g., of the market, of the user, of the request).
7. When storing time in UTC, use ISO 8601 (`YYYY-MM-DDTHH:mm:ss.sssZ`) format for consistency and quick manipulation in many programming languages.
8. When writing tests that include the "time" parameter, test multiple time zones and edge cases (e.g., UTC+14 and UTC-12, DST transition dates).
  - We can mock browser's time zone (e.g., see how on [Chromium browsers](https://developer.chrome.com/docs/devtools/sensors)).
	- We can see a list of time zones and make calculations using [world time buddy](https://www.worldtimebuddy.com/).
