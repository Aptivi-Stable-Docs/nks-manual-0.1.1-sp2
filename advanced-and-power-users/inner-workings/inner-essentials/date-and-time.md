---
description: Date and time tools
icon: clock-three
---

# Date and Time

Nitrocid KS provides an API that allows you to perform date and time operations, like rendering them to a string instance, managing time zones, and starting alarms. This feature is found under the `Nitrocid.Kernel.Time` namespace, as this feature is crucial for the kernel to be able to tell the current date (today) and the current time (now).

The tools let you perform from the simplest operations, such as getting the current date and time, to the most complex operations, such as converting date and time to other formats, such as the Unix time.

## Time tools

The base time/date tools allow you to perform general time and date operations, such as getting remaining time from either the current moment or from the selected moment. These functions allow you to perform this operation easily:

* `GetRemainingTimeFromNow()`: Returns a `TimeSpan` instance that indicates the remaining time from the current moment to the specified amount of milliseconds.
* `GetRemainingTimeFrom()`: Returns a `TimeSpan` instance that indicates the remaining time from the specified moment to the specified amount of milliseconds.

Not only these tools are readily available for you to use, but you can also use this class to get the current date and time in both the local time (`KernelDateTime`) and the UTC time zone (`KernelDateTimeUtc`).

### Time zones

<figure><img src="../../../.gitbook/assets/144-wcsaver.png" alt=""><figcaption></figcaption></figure>

Nitrocid KS provides a built-in time zone API that allows you to get information about a time zone and convert the time to the equivalent time using the specific time zone. This API is found in the `TimeZones` class.

{% hint style="info" %}
If the kernel-wide time zone is enabled, the current kernel time and date changes, depending on your selected kernel-wide time zone. Otherwise, the kernel uses the operating system time and date.
{% endhint %}

One of the functions that the above class implements is `GetZoneTimeString()` and its sibling functions, `GetZoneTimeTimeString()` and `GetZoneTimeDateString()`.

* The first function returns the full date and time using the specified time zone.
* The second function returns the time using the specified time zone.
* The third function returns the date using the specified time zone.

{% hint style="info" %}
For Linux systems, a check for `/usr/share/zoneinfo` is performed, and if it doesn't exist, you'll be prompted to install `tzdata` from your Linux distribution's repository. See the [dependency page](../../../installation-and-maintenance/dependency-information.md) for more information about how to install this package to your system.
{% endhint %}

### Calendar management API

<figure><img src="../../../.gitbook/assets/025-calendar.png" alt=""><figcaption></figcaption></figure>

In addition to providing the calendar command in the addon, the base Nitrocid API provides some of the calendar management APIs to make access to them easier than before. You can now get the calendar straight from the calendar type or its name using the `GetCalendar()` function from the `CalendarTools` class.

{% hint style="info" %}
Additionally, you can use the variant calendar, which uses your culture to define a calendar culture. This can be accessed by calling `GetCalendar()` with `CalendarTypes.Variant` as a type.
{% endhint %}

Additionally, the time and date renderers can be used with the base calendar class instance obtained from the above function. The following calendars can be used:

* `Chinese`: `zh-CN`
* `Gregorian`: `en-US`
* `Hijri`: `ar`
* `Japanese`: `ja-JP`
* `Persian`: `fa`
* `SaudiHijri`: `ar-SA`
* `Taiwanese`: `zh-TW`
* `ThaiBuddhist`: `th-TH`
* `Variant`: Your culture as specified by your kernel configuration

{% hint style="warning" %}
It's important to take note of these:

* While the events can be configured to be a yearly event, such as the Nitrocid KS release anniversary, such events have to be created by hand until the event commands are updated.
* Although it looks like that you can make your own custom calendar, there are no available methods to register your own calendar. We'll work on it in the next Nitrocid release. For now, use your newly-created instance of your calendar.
{% endhint %}

### Date conversion API

Nitrocid API provides date conversion tools that allow you to change how the date and the time are being represented. For instance, you can translate the date and the time to UNIX time and vice versa.

As for the calendars, you can use the `GetDateFromCalendar()` function, provided that you have a calendar instance from the `GetCalendar()` function. Also, you can use the `GetDateFromCalendarNoCulture()` function for calendars with overridden `Calendar` property.

## Miscellaneous tools

In addition to the above time tools, there are some of the features that use the date and time feature as an essential recipe for their implementation.

### Alarms

<figure><img src="../../../.gitbook/assets/024-caffeine.png" alt=""><figcaption></figcaption></figure>

When it comes to alarms, they are useful to alarm you for something at a specified time. This service and its tools can be accessed via the `AlarmTools` class.

```csharp
public static class AlarmTools
```

You can start an alarm using the `StartAlarm()` function that lets you specify your alarm ID, your alarm name, and your alarm interval in seconds.

```csharp
public static void StartAlarm(string alarmId, string alarmName, int alarmValue)
```

Additionally, you can stop an alarm before it's up using a function defined below:

```csharp
public static void StopAlarm(int alarmIdx)
public static void StopAlarm(string alarmId)
```

### Renderers

The renderers are essential if you want to get the text form of either the current date and time or the selected date and time. This is to simplify the functions that are used to do the same thing, but in the more coherent form to be able to use it like never before. The following classes are available for you to use in your mods:

* `TimeDateRenderers`: This class provides general time and date renderers that use your local time zone to return a text indicating either the current time and date or the selected time and date. You can optionally specify a target date, a target culture/calendar, and a target format type.
* `TimeDateRenderersUtc`: This class provides general time and date renderers that use UTC to return a text indicating either the current time and date or the selected time and date. You can optionally specify a target date, a target culture/calendar, and a target format type.
* `TimeDateMiscRenderers`: This class provides miscellaneous time and date renderers, including getting a string instance of the remaining time and printing the current date and time to the console.

{% hint style="info" %}
You can also use one of the constants specified in the `TimeDateRenderConstants` class as a custom format.

In some renderers, you can use the custom date and time formats, as long as they specify a valid syntax to print the date and the time. You can refer to [this documentation](https://learn.microsoft.com/en-us/dotnet/standard/base-types/standard-date-and-time-format-strings) for more information about how to write a valid date and time format.
{% endhint %}

<figure><img src="../../../.gitbook/assets/145-corner.png" alt=""><figcaption></figcaption></figure>

Nitrocid KS provides an option to show the date and time in the upper right corner. If you've enabled this option, you'll be able to see the current date and time there. To enable or to disable this feature, you must open the kernel settings application using the `settings` command, then `Miscellaneous settings` > `Show Time/Date on Upper Right Corner`. Once enabled, you should be able to see the current date and time in the corner as shown in the above picture.

{% hint style="info" %}
In case the current date and time doesn't show up in the corner, reboot the kernel, and it should show up.
{% endhint %}
