---
title: "Setting the time zone in C#.NET"
date: 2014-04-03T21:31:00
# bookComments: false
# bookSearchExclude: false
tags: [.NET, C#, Database, DateTime, Timezone, UTC]
---
DateTimes in .NET are always in one of two timezones - UTC or local time. This advice will make your life easier:

> Always store DateTimes in UTC.

Unfortunately sometimes we need to deal with timezones stored in a timezone other than UTC. Maybe we've inherited a system or we're integrating with another system which doesn't save as UTC. If your web server and database server are both in the same timezone and the DateTime is stored in the server's local timezone then everything is still OK. You can easily convert back to UTC or if the users are in the same timezone you don't need to do any conversion.

What happens when the DateTime stored in the database isn't in UTC or the server's local timezone? If you know the timezone then you can convert the DateTime to UTC using code similar to the following. Once you've converted to UTC then you can work with it as usual.

```
    /// <summary>
    /// Convert a DateTime in a specified timezone to UTC.
    /// </summary>
    /// <param name="timeZoneId">
    /// For valid IDs see: http://msdn.microsoft.com/en-us/library/cc749073.aspx
    ///</param>
    private DateTime GetUtcDateTimeForTimezone(DateTime dateTime, string timeZoneId)
    {
        TimeZoneInfo tzi = TimeZoneInfo.FindSystemTimeZoneById(timeZoneId);
        DateTimeOffset dto = new DateTimeOffset(dateTime, tzi.GetUtcOffset(dateTime));
        return new DateTime(dto.ToUniversalTime().DateTime.Ticks, DateTimeKind.Utc);
    }
```

As the comment in the above code mentions, [a list of all available time zones can be found here](http://msdn.microsoft.com/en-us/library/cc749073.aspx).