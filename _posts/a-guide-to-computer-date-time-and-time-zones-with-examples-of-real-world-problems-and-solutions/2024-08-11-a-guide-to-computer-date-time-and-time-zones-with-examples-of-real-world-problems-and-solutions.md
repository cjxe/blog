---
title: "Understanding computer date, time and time zones"
date: 2024-08-11 17:52:49 +02:00
modified: 2025-03-09 08:11:45 +00:00
tags: [technology]
description: A practical guide to computer date, time and time zones with examples of real-world problems and solutions.
toc: true
---

<h1 style="display: none;"></h1>

The nature of time is complex for many reasons, one of which is different parts of the world experience it differently. This complexity creates challenges in technology by preventing us from connecting easily across the globe. So, computers must track time precisely; but varying time zones, daylight savings, and shifts in time calculations complicate the issue.

In this post, you'll learn *why* computers track date<small id="reference-date"><sup>[[1]](#definition-date)</sup></small> and time<small id="reference-time"><sup>[[2]](#definition-time)</sup></small>, *how* they do it, alongside real-world examples; so, you can start building time-based applications without the headache, or easily solve your existing time-related issues.

---

<small id="definition-date"><sup>[[1]](#reference-date)</sup>**Date** (noun): A numbered day in a month, often given with the day name, month, and year. A popular format on computers: `YYYY-MM-DD` (e.g., 2024-07-29).</small>

<small id="definition-time"><sup>[[2]](#reference-time)</sup>**Time** (noun): The part of existence that is measured in minutes, days, years, etc., or this process considered as a whole. A popular format on computers: `HH:MM:SS` without days, months or years (e.g., 16:59:25).</small>

# Birth of computer time

As computers evolved from human calculators to digital machines, their creators required practical methods to improve computers' user experience. So, they developed operating systems (OS).

With OS-equipped computers, computer users tackled various challenges, including the need to calculate date and time differences. So, OS engineers developed a time-calculation system.

UNIX (an influential early OS) introduced a solution with 2 assumptions to calculate date and time differences:
1. **Start date**: UNIX computers established a fixed starting point which is set to January 1 1970, 00:00:00 UTC (i.e., UNIX epoch<small id="reference-epoch"><sup>[[3]](#definition-epoch)</sup></small>). This acts as the starting date and time for most computers, similar to how "Year 0" is set in the Gregorian calendar we use today.
2. **Time representation**: UNIX time is measured as the number of seconds (or milliseconds) elapsed since the UNIX epoch. E.g., as I write this sentence within this blog post, the UNIX time is 1723306952 seconds, indicating that many seconds have passed since the start of the UNIX epoch. 

Some problems included calculating date and time differences that included the *current* date and time, so computer users needed to know the current time. 

But how did the computers know their *current* time?

---

<small id="definition-epoch"><sup>[[3]](#reference-epoch)</sup>**Epoch** (noun): The beginning of a period in the history of someone or something.</small>

# How computers track time

There are 2 clocks<small id="reference-clock"><sup>[[4]](#definition-clock)</sup></small> in a computer to track the current time:
1. **Hardware clock** (a.k.a. Real-time clock): The machine's hardware tracks a clock using a battery and circuits. It is recommended to set this clock to UTC<small id="reference-UTC"><sup>[[5]](#definition-UTC)</sup></small>, but you can also configure an additional "time zone" property to align this clock with your local time. E.g., Windows sets the hardware's clock to the user's local time; whereas MacOS and Linux set their hardware clock to UTC.
2. **System clock**: The system clock is dependent on the hardware clock, and managed by the OS's kernel. The OS reads the hardware clock, but also is in sync with the other clocks on the internet. When an app wants to access your machine's clock, it accesses this clock.

---

<small id="definition-clock"><sup>[[4]](#reference-clock)</sup>**Clock** (noun): A mechanical or electrical device for measuring time, indicating hours, minutes, and sometimes seconds by hands on a round dial or by displayed figures.</small>

<small id="definition-UTC"><sup>[[5]](#reference-UTC)</sup>**Coordinated Universal Time (UTC)** (noun): A time standard that follows the Greenwich Mean Time (GMT) without Daylight Savings Time (DST) adjustments.</small>

# Accessing your computer's time values

## With the terminal

### System clock

If you are on MacOS or Linux, you may access the system clock by running the `date` command in your bash shell:

```bash
> date
Thu 28 Mar 2024 18:10:10 CET # This command applies the time zone offset by default.

> date -u 
Thu 28 Mar 2024 17:10:10 UTC

> date +"%s" 
1711645810 # The number of seconds that passed since the UNIX epoch. The `-u` flag is applied by default. Think of this number when you think about the _system clock_.
```

### Currently selected time zone

"Internally, your Mac is using UTC. Your Mac is not "in CET". It is "in UTC with a default `TZ` setting of `CET`". `TZ` environment variable controls how a local time is derived from the underlying UTC when a local timestamp or timezone offset is required. If you change `TZ`, then local timestamps will be shown in the new timezone, but the computer's internal timekeeping in UTC is not affected." <a href="https://stackoverflow.com/a/54524475/12959962" target="_blank">source</a>

So, when you run the `date` command, then the command-line utility reads the system clock and the `TZ` environment variable stored somewhere in the operating system, and returns the current date including the local time zone's offset. If you would like to get the current time zone of your computer, you can run the following command in UNIX-like operating systems:
```bash
> sudo systemsetup -gettimezone 
Time Zone: Europe/Berlin
```

Further reading: <a href="https://sdimitro.github.io/post/keeping-track-time" target="_blank">how do computers store and maintain all the different time zone information to keep up with each country’s ever-changing time zone rule?</a>

## With a programming language

Your favourite browser (and many other apps) reads the system clock and the `TZ` environment variable. Open up your browser's DevTools and navigate to the console. Then, run the following code:

```js
> const currentDate = new Date();
> console.log(currentDate)
Thu Mar 28 2024 18:10:10 GMT+0100 (Central European Standard Time) // Similar to running `date` in a bash shell.

> console.log(currentDate.valueOf()) 
1711645810000 // Similar to running `date +"%s"` in a bash shell.

> const tz = Intl.DateTimeFormat().resolvedOptions().timeZone;
> console.log(tz);
Europe/Berlin // Similar to running `sudo systemsetup -gettimezone` in a bash shell.
```

# Troubleshooting time-related issues in real-world applications

**Disclaimer**: I obfuscated sensitive information (such as the name of the market/exchange) to prevent any sensitive information from being leaked.

## Problem #1: Fiji market's analytics are 1 hour behind Fiji's current time

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

## Problem #2: Incorrect relative time display (e.g., 9 hours ago)

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

<a href="https://stackoverflow.com/a/15171030/12959962" target="_blank">Further explanation.</a>

**How to prevent this issue from happening again**

The database should store the time in UTC (instead of local time).

## Problem #3: Discrepancy between Data Manipulation Service and the UI

**Background**

1. The UI has a calendar component that allows the user to select a single day, month and year.
2. The data and network flow throughout the app: UI <--> internal API <-->  Data Manipulation Service <--> Database

**Actual/current behaviour**

The table in the UI doesn't match the table in Data Manipulation Service .

**Expected behaviour**

The table in the UI should match the table in Data Manipulation Service.

**Investigation result**

Given the user is in Asia/Tokyo time zone,

when the user loads the report,

and set the end date to "10/01/2024",

then the UI sends "10/01/2024 14:59:59 UTC" to the internal API,

then Data Manipulation Service makes a search query that is 9 hours shorter than expected

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

# Best practices

These are the best practices that I found to be the most valuable.

1. Use UTC for everything excluding when displaying the time to the user.
2. When displaying the time to the user, indicate the time zone of the displayed time.
3. When displaying the time to the user, display it in their locale format.
4. When manipulating time (e.g., to display in a different format, or to calculate a new time), use libraries.
5. When using libraries, keep them up-to-date (preferably automatically).
6. When storing time in UTC, also store the time zone information (e.g., of the market, of the user, of the request).
7. When storing time in UTC, use ISO 8601 (`YYYY-MM-DDTHH:mm:ss.sssZ`) format for consistency and quick manipulation in many programming languages.
8. When writing tests that include the "time" parameter, test multiple time zones and edge cases (e.g., UTC+14 and UTC-12, DST transition dates).
  - We can mock browser's time zone (e.g., see how on <a href="https://developer.chrome.com/docs/devtools/sensors" target="_blank">Chromium browsers</a>).
	- We can see a list of time zones and make calculations using <a href="https://www.worldtimebuddy.com" target="_blank">world time buddy</a>.
