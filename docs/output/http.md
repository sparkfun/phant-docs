---
layout: docs
title: HTTP
docs: Output
order: 1
---

To retrieve your data, you will only need your `PUBLIC_KEY`.  All data on [data.sparkfun.com](https://data.sparkfun.com)
**should be considered publicly accessible**, but you can host your own [phant server](https://github.com/sparkfun/phant)
if you would like your data to be private.

## Data Formats
Data is currently available in `CSV`, `JSON`, and `JSONP` formats, but more formats will be available soon.  If you
would like to see a specific output format supported, [submit an issue on GitHub](https://github.com/sparkfun/phant/issues),
and we will try to make it available.

## Paging
By default, [data.sparkfun.com](https://data.sparkfun.com) returns all of your logged data in the format you requested.
If there is a need, you can also request output in 50 kilobyte chunks by setting the `page` parameter in the
[query string](http://en.wikipedia.org/wiki/Query_string) portion of the URL.  This can be very helpful if you just
want to check out the latest logged data without requesting the entire data set.

## HTTP Request Examples
Data can be retrieved by using your `PUBLIC_KEY` to make a `HTTP GET` request for the `FORMAT` you would like
returned by the server.  Data will be returned in **reverse chronological order**.

<div class="url">
  <span class="method GET">GET</span>
  http://data.sparkfun.com/output/PUBLIC_KEY.FORMAT
</div>

{% highlight bash %}
curl -X GET 'http://data.sparkfun.com/output/PUBLIC_KEY.json'
{% endhighlight %}

**Example** Retrieve the first page of data in CSV format:

<div class="url">
  <span class="method GET">GET</span>
  http://data.sparkfun.com/output/PUBLIC_KEY.FORMAT?page=PAGE_NUMBER
</div>

{% highlight bash %}
curl -X GET 'http://data.sparkfun.com/output/PUBLIC_KEY.csv?page=1'
{% endhighlight %}

The JSON response would look like this:
{% highlight text %}
humidity,temp,timestamp
2,3,2014-06-25T21:35:29.827Z
86%,91.4,2014-06-25T21:10:08.112Z
86%,91.4,2014-06-25T20:40:56.425Z
{% endhighlight %}

### JSONP Output
If you are using JavaScript in a web browser to retrieve data, then you might be interested in using the
[JSONP](http://en.wikipedia.org/wiki/JSONP) format.  JSONP allows you to make to requests from a server
from a different domain, which is normally not possible because of the
[same-origin policy](http://en.wikipedia.org/wiki/Same-origin_policy).

Unlike all of the other methods, JSONP responses will always be sent with the `HTTP 200` success code.  We respond
this way for the JSONP format because browsers will not parse the response body when the server replies with a HTTP error code.

**Example** Retrieve the first page of data in JSONP format with [jQuery](http://jquery.com):

{% highlight js %}
var public_key = 'YOUR_PUBLIC_KEY';
 $.ajax({
   url: 'http://data.sparkfun.com/output/' + public_key + '.json',
   jsonp: 'callback',
   cache: true,
   dataType: 'jsonp',
   data: {
     page: 1
   },
   success: function(response) {
     // response will be a javascript
     // array of objects
     console.log(response);
   }
 });
{% endhighlight %}

The JSONP response to this request would look like this:
{% highlight js %}
typeof handler === 'function' && handler([{"humidity":"2","temp":"3","timestamp":"2014-06-25T21:35:29.827Z"},{"humidity":"86%","temp":"91.4","timestamp":"2014-06-25T21:10:08.112Z"},{"humidity":"86%","temp":"91.4","timestamp":"2014-06-25T20:40:56.425Z"}]);
{% endhighlight %}

