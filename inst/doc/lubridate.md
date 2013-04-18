<!--
%\VignetteEngine{knitr}
%\VignetteIndexEntry{Lubridate}
-->

# Do more with dates and times in R with lubridate 1.3.0

_note: This vignette is an updated version of the blog post first published at [r-statistics](http://www.r-statistics.com/2012/03/do-more-with-dates-and-times-in-r-with-lubridate-1-1-0/)_

Lubridate is an R package that makes it easier to work with dates and times. Below is a concise tour of some of the things lubridate can do for you. 

Lubridate was created by Garrett Grolemund and Hadley Wickham.

## Parsing dates and times

Getting R to agree that your data contains the dates and times you think it does can be tricky. Lubridate simplifies that. Identify the order in which the year, month, and day appears in your dates. Now arrange “y”, “m”, and “d” in the same order. This is the name of the function in lubridate that will parse your dates. For example,


```r
library(lubridate)
```

```
## Error: there is no package called 'lubridate'
```

```r
ymd("20110604")
```

```
## Error: could not find function "ymd"
```

```r
mdy("06-04-2011")
```

```
## Error: could not find function "mdy"
```

```r
dmy("04/06/2011")
```

```
## Error: could not find function "dmy"
```


Lubridate's parse functions handle a wide variety of formats and separators, which simplifies the parsing process.

If your date includes time information, add h, m, and/or s to the name of the function. `ymd_hms` is probably the most common date time format. To read the dates in with a certain time zone, supply the official name of that time zone in the `tz` argument.


```r
arrive <- ymd_hms("2011-06-04 12:00:00", tz = "Pacific/Auckland")
```

```
## Error: could not find function "ymd_hms"
```

```r
arrive
```

```
## Error: object 'arrive' not found
```

```r
leave <- ymd_hms("2011-08-10 14:00:00", tz = "Pacific/Auckland")
```

```
## Error: could not find function "ymd_hms"
```

```r
leave
```

```
## Error: object 'leave' not found
```


## Setting and Extracting information

Extract information from date times with the functions `second`, `minute`, `hour`, `day`, `wday`, `yday`, `week`, `month`, `year`, and `tz`. You can also use each of these to set (i.e, change) the given information. Notice that this will alter the date time. `wday` and `month` have an optional `label` argument, which replaces their numeric output with the name of the weekday or month.


```r
second(arrive)
```

```
## Error: could not find function "second"
```

```r
second(arrive) <- 25
```

```
## Error: object 'arrive' not found
```

```r
arrive
```

```
## Error: object 'arrive' not found
```

```r
second(arrive) <- 0
```

```
## Error: object 'arrive' not found
```

```r

wday(arrive)
```

```
## Error: could not find function "wday"
```

```r
wday(arrive, label = TRUE)
```

```
## Error: could not find function "wday"
```


## Time Zones

There are two very useful things to do with dates and time zones. First, display the same moment in a different time zone. Second, create a new moment by combining an existing clock time with a new time zone. These are accomplished by `with_tz` and `force_tz`.

For example, a while ago I was in Auckland, New Zealand. I arranged to meet the co-author of lubridate, Hadley, over skype at 9:00 in the morning Auckland time. What time was that for Hadley who was back in Houston, TX?


```r
meeting <- ymd_hms("2011-07-01 09:00:00", tz = "Pacific/Auckland")
```

```
## Error: could not find function "ymd_hms"
```

```r
with_tz(meeting, "America/Chicago")
```

```
## Error: could not find function "with_tz"
```


So the meetings occurred at 4:00 Hadley’s time (and the day before no less). Of course, this was the same actual moment of time as 9:00 in New Zealand. It just appears to be a different day due to the curvature of the Earth.

What if Hadley made a mistake and signed on at 9:00 his time? What time would it then be my time?


```r
mistake <- force_tz(meeting, "America/Chicago")
```

```
## Error: could not find function "force_tz"
```

```r
with_tz(mistake, "Pacific/Auckland")
```

```
## Error: could not find function "with_tz"
```


His call would arrive at 2:00 am my time! Luckily he never did that.

## Time Intervals

You can save an interval of time as an Interval class object with lubridate. This is quite useful! For example, my stay in Auckland lasted from June 4, 2011 to August 10, 2011 (which we’ve already saved as arrive and leave). We can create this interval in one of two ways:


```r
auckland <- interval(arrive, leave)
```

```
## Error: could not find function "interval"
```

```r
auckland
```

```
## Error: object 'auckland' not found
```

```r
auckland <- arrive %--% leave
```

```
## Error: could not find function "%--%"
```

```r
auckland
```

```
## Error: object 'auckland' not found
```


My mentor at the University of Auckland, Chris, traveled to various conferences that year including the Joint Statistical Meetings (JSM). This took him out of the country from July 20 until the end of August.


```r
jsm <- interval(ymd(20110720, tz = "Pacific/Auckland"), ymd(20110831, tz = "Pacific/Auckland"))
```

```
## Error: could not find function "interval"
```

```r
jsm
```

```
## Error: object 'jsm' not found
```


Will my visit overlap with and his travels? Yes.

```{r]
int_overlaps(jsm, auckland)
```

Then I better make hay while the sun shines! For what part of my visit will Chris be there?


```r
setdiff(auckland, jsm)
```

```
## Error: object 'auckland' not found
```


Other functions that work with intervals include `int_start`, `int_end`, `int_flip`, `int_shift`, `int_aligns`, `union`, `intersect`, `setdiff`, and `%within%`.

## Arithmetic with date times

Intervals are specific time spans (because they are tied to specific dates), but lubridate also supplies two general time span classes: Durations and Periods. Helper functions for creating periods are named after the units of time (plural). Helper functions for creating durations follow the same format but begin with a “d” (for duration) or, if you prefer, and “e” (for exact).


```r
minutes(2)  ## period
```

```
## Error: could not find function "minutes"
```

```r
dminutes(2)  ## duration
```

```
## Error: could not find function "dminutes"
```


Why two classes? Because the timeline is not as reliable as the number line. The Duration class will always supply mathematically precise results. A duration year will always equal 365 days. Periods, on the other hand, fluctuate the same way the timeline does to give intuitive results. This makes them useful for modeling clock times. For example, durations will be honest in the face of a leap year, but periods may return what you want:


```r
leap_year(2011)  ## regular year
```

```
## Error: could not find function "leap_year"
```

```r
ymd(20110101) + dyears(1)
```

```
## Error: could not find function "ymd"
```

```r
ymd(20110101) + years(1)
```

```
## Error: could not find function "ymd"
```

```r

leap_year(2012)  ## leap year
```

```
## Error: could not find function "leap_year"
```

```r
ymd(20120101) + dyears(1)
```

```
## Error: could not find function "ymd"
```

```r
ymd(20120101) + years(1)
```

```
## Error: could not find function "ymd"
```


You can use periods and durations to do basic arithmetic with date times. For example, if I wanted to set up a reoccuring weekly skype meeting with Hadley, it would occur on:


```r
meetings <- meeting + weeks(0:5)
```

```
## Error: object 'meeting' not found
```


Hadley travelled to conferences at the same time as Chris. Which of these meetings would be affected? The last two.


```r
meetings %within% jsm
```

```
## Error: could not find function "%within%"
```


How long was my stay in Auckland?


```r
auckland/edays(1)
```

```
## Error: object 'auckland' not found
```

```r
auckland/edays(2)
```

```
## Error: object 'auckland' not found
```

```r
auckland/eminutes(1)
```

```
## Error: object 'auckland' not found
```


And so on. Alternatively, we can do modulo and integer division. Sometimes this is more sensible than division - it is not obvious how to express a remainder as a fraction of a month because the length of a month constantly changes.


```r
auckland%/%months(1)
```

```
## Error: object 'auckland' not found
```

```r
auckland%%months(1)
```

```
## Error: object 'auckland' not found
```


Modulo with an timespan returns the remainder as a new (smaller) interval. You can turn this or any interval into a generalized time span with as.period().


```r
as.period(auckland%%months(1))
```

```
## Error: could not find function "as.period"
```

```r
as.period(auckland)
```

```
## Error: could not find function "as.period"
```


### If anyone drove a time machine, they would crash

The length of months and years change so often that doing arithmetic with them can be unintuitive. Consider a simple operation, `January 31st + one month`. Should the answer be 

1. `February 31st` (which doesn't exist)
2. `March 4th` (31 days after January 31), or
3. `February 28th` (assuming its not a leap year)

A basic property of arithmetic is that `a + b - b = a`. Only solution 1 obeys this property, but it is an invalid date. I've tried to make lubridate as consistent as possible by invoking the following rule *if adding or subtracting a month or a year creates an invalid date, lubridate will return an NA*. This is new with version 1.3.0, so if you're an old hand with lubridate be sure to remember this!

If you thought solution 2 or 3 was more useful, no problem. You can still get those results with clever arithmetic, or by using the special `%m+%` and `%m-%` operators. `%m+%` and `%m-%` automatically roll dates back to the last day of the month, should that be necessary.


```r
jan31 <- ymd("2013-01-31")
```

```
## Error: could not find function "ymd"
```

```r
jan31 + months(0:11)
```

```
## Error: object 'jan31' not found
```

```r
floor_date(jan31, "month") + months(0:11) + days(31)
```

```
## Error: could not find function "floor_date"
```

```r
jan31 %m+% months(0:11)
```

```
## Error: could not find function "%m+%"
```


Notice that this will only affect arithmetic with months (and arithmetic with years if your start date it Feb 29).

## Vectorization

The code in lubridate is vectorized and ready to be used in both interactive settings and within functions. As an example, I offer a function for advancing a date to the last day of the month


```r
last_day <- function(date) {
    ceiling_date(date, "month") - days(1)
}
```



## Further Resources

To learn more about lubridate, including the specifics of periods and durations, please read the [original lubridate paper](http://www.jstatsoft.org/v40/i03/). Questions about lubridate can be addressed to the lubridate google group. Bugs and feature requests should be submitted to the [lubridate development page](http://github.com/hadley/lubridate) on github.