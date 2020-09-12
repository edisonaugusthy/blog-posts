**See some of the single line Javascript snippets for dealing with date** ✨♻️

#### Query

###### Is Before

Check if a date is before another date.

```js
// '2010-10-20' is before 2010-10-21
new Date(2010, 10, 20) < new Date(2010, 10, 21);
// => true
```

###### Is Same

Check if a date is the same as another date.

```js
// '2010-10-20' is same of '2010-10-21'

new Date(2010, 9, 20).valueOf() === new Date(2010, 9, 21).valueOf();
// => false
new Date(2010, 9, 20).valueOf() === new Date(2010, 9, 20).valueOf();
// => true
new Date(2010, 9, 20).getTime() === new Date(2010, 9, 20).getTime();
// => true
new Date(2010, 9, 20).valueOf() === new Date(2010, 9, 20).getTime();
// => true
new Date(2010, 9, 20).toDateString().substring(4, 7) ===
  new Date(2010, 9, 21).toDateString().substring(4, 7);
// => true
```

###### Is After

Check if a date is after another date

```js
new Date(2010, 9, 20) > new Date(2010, 9, 19);
// => true
```

###### Is Leap Year

Check if a year is a leap year

```js
new Date(2000, 1, 29).getDate() === 29;
// => true
```

###### Is a Date

Check if a variable is a native js Date object.

```js

new Date() instanceof Date;
// => true
```
--------------------------------

#### GET Or SET
 Get the Millisecond/Second/Minute/Hour of the given date

```js
new Date().getSeconds();
// => 49
new Date().getHours();
// => 19
```

 Set the Millisecond/Second/Minute/Hour of the given date.

```js
new Date(new Date().setSeconds(30));
// => "2018-09-09T09:12:30.695Z"
new Date(new Date().setHours(13));
// => "2018-09-09T03:12:49.695Z"
```

###### Date of Month

Gets the day of the month.

```js

new Date().getDate();
// => 9

```

Sets the day of the month

```js
new Date().setDate(4);
// => "2018-09-04T09:12:49.695Z"
```

###### Day of Week

Gets the day of the week.

```js

new Date().getDay();
// => 0 (Sunday)
```

Sets the day of the week

```js

new Date().setDate(new Date().getDate() - 14);
// => "2018-08-26T09:12:49.695Z"
```

###### Day of Year

Gets or Sets the day of the year.

```js

Math.floor(
  (new Date() - new Date(new Date().getFullYear(), 0, 0)) / 1000 / 60 / 60 / 24
);
// => 252
```

###### Week of Year

Gets the week of the year


```js
const day = new Date();
const MILLISECONDS_IN_WEEK = 604800000;
const firstDayOfWeek = 1; // monday as the first day (0 = sunday)
const startOfYear = new Date(day.getFullYear(), 0, 1);
startOfYear.setDate(
  startOfYear.getDate() + (firstDayOfWeek - (startOfYear.getDay() % 7))
);
const dayWeek = Math.round((day - startOfYear) / MILLISECONDS_IN_WEEK) + 1;
// => 37
```

Sets the week of the year

```js

const day = new Date();
const week = 24;
const MILLISECONDS_IN_WEEK = 604800000;
const firstDayOfWeek = 1; // monday as the first day (0 = sunday)
const startOfYear = new Date(day.getFullYear(), 0, 1);
startOfYear.setDate(
  startOfYear.getDate() + (firstDayOfWeek - (startOfYear.getDay() % 7))
);
const dayWeek = Math.round((day - startOfYear) / MILLISECONDS_IN_WEEK) + 1;
day.setDate(day.getDate() - (dayWeek - week) * 7);
day.toISOString();
// => "2018-06-10T09:12:49.794Z
```

###### Days in Month

Get the number of days in the current month

```js
new Date(2012, 02, 0).getDate();
// => 29
```


-----------------------------

#### Parse

 Return the date parsed from date string using the given format string

```js
const datePattern = /^(\d{2})-(\d{2})-(\d{4})$/;
const [, month, day, year] = datePattern.exec('12-25-1995');
new Date(`${month}, ${day} ${year}`);
// => "1995-12-24T13:00:00.000Z"
```

 Return the date parsed from time string using the given format string

```js
const datePattern = /^(\d{4})-(\d{2})-(\d{2})\s(\d{1,2}):(\d{2})$/;
const [, year, month, day, rawHour, min] = datePattern.exec('2010-10-20 4:30');
new Date(`${year}-${month}-${day}T${('0' + rawHour).slice(-2)}:${min}:00`);
// => "2010-10-19T17:30:00.000Z"
```

----------------------------

#### Manipulate


###### Add

Add the specified number of days to the given date.

```js
// add 7 days
const now = new Date();
now.setDate(now.getDate() + 7);
// => "Sun Sep 16 2018 09:12:49"
```

###### Subtract

Subtract the specified number of days from the given date.

```js
// subsctact 7 days
new Date(new Date().getTime() - 1000 * 60 * 60 * 24 * 7);
// => Sun Sep 09 2018 09:12:49
```

###### End of Time

Return the end of a unit of time for the given date.

```js
const end = new Date();
end.setHours(23, 59, 59, 999);
end.toISOString();
// => "2018-09-09T16:59:59.999Z"
```

--------------------

#### Display

###### Time from now

Return time from now.

```js

new Intl.RelativeTimeFormat().format(-4, 'day');
// => "4 days ago"
```

###### Difference

Get the unit of time between the given dates

```js

// difference betwwen '2007-01-27' to '2007-01-29'
new Date(2007, 0, 27) - new Date(2007, 0, 29);
// => -172800000
Math.ceil(
  (new Date(2007, 0, 27) - new Date(2007, 0, 29)) / 1000 / 60 / 60 / 24
);
// => -2
```

----------------

