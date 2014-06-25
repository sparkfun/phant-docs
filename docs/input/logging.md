---
layout: docs
title: Logging Data
docs: Input
order: 1
---

To log data, you will need your list of field names, your `PUBLIC_KEY`, and your `PRIVATE_KEY`.  You can use
one of the [phant libraries](/libraries) to log data, or you can make a HTTP request using any web client.  We will
soon be releasing info about how to log data via TCP & UDP, but for now only HTTP based input is available.

## Data Types
All data is logged as a string, so anything that can be converted to a UTF-8 compatible string can be logged.  There
is no length limit to field data, but each request is currently limited to 50 kilobytes.

## HTTP Request Examples
You can make either a `HTTP GET` or `HTTP POST` request to log data, but when possible you should use `HTTP POST`.
We will be using `temp` and `humidity` as the field names in the following examples, but you should replace them
with the names of the fields you entered when creating your data stream. Replace `PUBLIC_KEY` and `DELETE_KEY`
with the keys provided to you when you created the stream.  You should make sure that you
[URL encode](http://en.wikipedia.org/wiki/Percent-encoding) your data before sending it.

<div class="url">
  <span class="method GET">GET</span>
  http://data.sparkfun.com/input/PUBLIC_KEY?private_key=PRIVATE_KEY&FIELD1=VALUE1&=FIELD2=VALUE2
</div>

{% highlight bash %}
curl -X GET 'http://data.sparkfun.com/input/PUBLIC_KEY?private_key=PRIVATE_KEY&temp=91.4&=humidity=86%25'
{% endhighlight %}

When making a `HTTP POST` request, you should send your `PRIVATE_KEY` using the `Phant-Private-Key` request header.

<div class="url">
  <span class="method POST">POST</span>
  http://data.sparkfun.com/input/PUBLIC_KEY
</div>

{% highlight bash %}
curl -X POST 'http://data.sparkfun.com/stream/PUBLIC_KEY' \
  -H 'Phant-Private-Key: PRIVATE_KEY' \
  -d 'temp=91.4&=humidity=86%25'
{% endhighlight %}

## Plain Text Response Examples

Currently, responses are sent back in plain text format by default.  The first character of the text response
will always be a `0` or a `1`, and the second character will always be a space. If the server responds with a `0`
as the first character, assume the request failed.  If the server responds with a `1` as the first character, you
can assume the post succeeded. The success or error message will start with the third character, and in some cases
error messages can span multiple lines.

**Example** Text response body from a successful post:

    1 success

**Example** Text response body from failed post:

    0 temperature is not a valid field for this stream.

     expecting: humidity, temp

If your client doesn't separate the response headers from the body, the response from the server
would look like this:

    HTTP/1.1 400 Bad Request
     content-type: text/plain
     Date: Wed, 25 Jun 2014 20:41:53 GMT
     Connection: keep-alive
     Transfer-Encoding: chunked

     0 temperature is not a valid field for this stream.

     expecting: humidity, temp

