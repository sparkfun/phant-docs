---
layout: docs
title: Delete
docs: Management
order: 2
---

If you would like to completely remove a stream, visit `YOUR_SERVER/streams/delete` and fill in the form with
your keys.  For example, you can delete a stream on [data.sparkfun.com](http://data.sparkfun.com) by visiting
[data.sparkfun.com/streams/delete](https://data.sparkfun.com/streams/delete).

## Delete Request Examples

If you would like to make the delete request manually, you can make either a `HTTP GET` or `HTTP DELETE`
request.  Replace `PUBLIC_KEY` and `DELETE_KEY` with the keys provided to you when you created the stream.

<div class="url">
  <span class="method GET">GET</span>
  http://data.sparkfun.com/stream/PUBLIC_KEY/delete/DELETE_KEY
</div>

{% highlight bash %}
curl -X GET 'http://data.sparkfun.com/stream/PUBLIC_KEY/delete/DELETE_KEY'
{% endhighlight %}

When making a `HTTP DELETE` request, you should send your `DELETE_KEY` using the `Phant-Delete-Key` request header.

<div class="url">
  <span class="method DELETE">DELETE</span>
  http://data.sparkfun.com/stream/PUBLIC_KEY/delete
</div>

{% highlight bash %}
curl -X DELETE 'http://data.sparkfun.com/stream/PUBLIC_KEY/delete' \
  -H 'Phant-Delete-Key: DELETE_KEY'
{% endhighlight %}
