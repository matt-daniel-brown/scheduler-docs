date
=============
@short: a set of date formatting methods
	

@type:object

@example:

@template:	api_config
@descr:
The **date** object provides the following methods:

<ul>
	<li>
    	<b>add ( date, number, unit)</b> - adds/subtracts the specified time interval to/from the date
        <ul>
          	<li><b><i>date</i></b> - (<i>Date</i>) the date object that you need to add a time to/subtract a time from </li>
            <li><b><i>number</i></b> - (<i>number</i>) the number of units to add. If this number is positive - the time will be added to the date, if negative - the time will be subtracted </li>
            <li><b><i>unit</i></b> - (<i>'minute', 'hour', 'day', 'week', 'month', 'year'</i>)  the time unit </li>
~~~js
//adds 1 year to the specified date: 29 June, 2013 -> 29 June, 2014
var newDate = scheduler.date.add(new Date(2013, 05, 29), 1, 'year');
~~~
        </ul>
    </li>
    <li>
    	<b>convert_to_utc ( date) </b> - converts local time to UTC
        <ul>
          	<li><b><i>date</i></b> - (<i>Date</i>) the date object to convert </li>
~~~js
//29 June, 2013 14:00 (local time) -> 29 June, 2013 12:00 (utc)
var time = scheduler.date.convert_to_utc(new Date(2013, 05, 29, 14, 00));
~~~
        </ul>
    </li>
    <li>
    	<b>copy ( date) </b> - makes a copy of a Date object
        <ul>
          	<li><b><i>date</i></b> - (<i>Date</i>) the date object to copy </li>
~~~js
var copy = scheduler.date.copy(new Date(2013, 05, 29));// -> 29 June, 2013
~~~
        </ul>
    </li>
    <li>
    	<b>date_part ( date) </b> - resets the time part of the provided date to 00:00:00
        <ul>
          	<li><b><i>date</i></b> - (<i>Date</i>) the date object to format </li>
~~~js
//29 June, 2013 14:30:10 -> 29 June, 2013 00:00:00
var date = scheduler.date.date_part(new Date(2013, 05, 29, 14, 30, 10));
~~~
        </ul>
    </li>
    <li>
    	<b>date_to_str ( format, utc) </b> - returns a function that converts a Date object to a string of the specified format
        <ul>
          	<li><b><i>format</i></b> - (<i>string</i>) the date format ( see settings_format.md)  </li>
          	<li><b><i>utc</i></b> - (<i>boolean</i>) specifies whether local time should be converted to UTC  </li>
~~~js
var formatFunc = scheduler.date.date_to_str("%d/%m/%Y");
var date = formatFunc(new Date(2013, 05, 29)); // -> "29/06/2013"

~~~
        </ul>
    </li>
    <li>
    	<b>day_start ( date) </b> - resets the time part of the provided date to 00:00:00. Alias of the <b>date_part</b> method. Used by the Day view to set the display date and can be redefined to provide the default behaviour
        <ul>
          	<li><b><i>date</i></b> - (<i>Date</i>) the date object to format </li>
~~~js
//29 June, 2013 14:30:10 -> 29 June, 2013 00:00:00
var date = scheduler.date.day_start(new Date(2013, 05, 29, 14, 30, 10));
~~~
        </ul>
    </li>
    <li>
    	<b>getISOWeek ( date) </b> - returns the week number of the date
        <ul>
          	<li><b><i>date</i></b> - (<i>Date</i>) the date object to format </li>
~~~js
var week = scheduler.date.getISOWeek(new Date(2013, 05, 29));// ->26
~~~
        </ul>
    </li>
    <li>
    	<b>getUTCISOWeek ( date) </b> - returns the week number of the date, but previously converts local time to UTC
        <ul>
          	<li><b><i>date</i></b> - (<i>Date</i>) the date object to format </li>
~~~js
var week = scheduler.date.getUTCISOWeek(new Date(2013, 05, 29));// ->26
~~~
        </ul>
    </li>
    <li>
    	<b>month_start ( date) </b> - returns a Date object of the first day of the month for the specified date and clears the time part to zero
        <ul>
          	<li><b><i>date</i></b> - (<i>Date</i>) the date object to format </li>
~~~js
//29 June, 2013 14:30 -> 01 June, 2013 00:00
var firstDay = scheduler.date.month_start(new Date(2013, 05, 29, 14, 30));
~~~
        </ul>
    </li>
    <li>
    	<b>str_to_date ( format, utc) </b> - returns a function that converts a string of the specified format to a Date object
        <ul>
          	<li><b><i>format</i></b> - (<i>string</i>) the date format ( see settings_format.md)  </li>
          	<li><b><i>utc</i></b> - (<i>boolean</i>) specifies whether local time should be converted to UTC  </li>
~~~js
var formatFunc = scheduler.date.str_to_date("%d/%m/%Y");
var date = formatFunc("29/06/2013"); // -> 29 June, 2013 00:00:00
~~~
        </ul>
    </li>
    <li>
    	<b>time_part ( date) </b> - returns the time of a Date object as a number of seconds counted from the midnight (00:00:00)
        <ul>
          	<li><b><i>date</i></b> - (<i>Date</i>) the date object to format </li>
~~~js
var time = scheduler.date.time_part(new Date(2013, 05, 29, 14, 30, 10));
//time -> 52210
~~~
        </ul>
    </li>
    <li>
    	<b>to_fixed ( num) </b> - adds the leading zero to numbers less than 10 and returns the result as a string. Doesn't affect numbers from 10
        <ul>
          	<li><b><i>num</i></b> - (<i>number</i>) the number to format </li>
~~~js
var num1 = scheduler.date.to_fixed(2);// ->"02"
var num2 = scheduler.date.to_fixed(10);// ->10
~~~
        </ul>
    </li>
    <li>
    	<b>week_start ( date) </b> - returns a Date object of the first day of the week for the specified date and clears the time part to zero
        <ul>
          	<li><b><i>date</i></b> - (<i>Date</i>) the date object to format </li>
~~~js
//29 June, 2013 14:30 -> 24 June, 2013 00:00
var weekStart = scheduler.date.week_start(new Date(2013, 05, 29, 14, 30));
~~~
        </ul>
    </li>
    <li>
    	<b>year_start ( date) </b> - returns a Date object of the first day of the year for the specified date and clears the time part to zero
        <ul>
          	<li><b><i>date</i></b> - (<i>Date</i>) the date object to format </li>
~~~js
//29 June, 2013 14:30 -> 01 January, 2013 00:00
var yearStart = scheduler.date.year_start(new Date(2013, 05, 29, 14, 30));
~~~
        </ul>
    </li>
</ul>



