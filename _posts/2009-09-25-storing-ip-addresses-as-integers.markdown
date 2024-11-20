---
title: Storing IP addresses as integers
date: 2009-09-25
layout: single
---

Tying long sessions to an IP address is a good way to ensure some security for your users. But inevitably, you'll want to store that IP. While you could, of course, store it as a string, such as `"123.125.126.127"` (called a "dotted quad"), some developers might prefer saving some space and storing it as a compact integer. There's a right way and a wrong way to do this. The wrong way is as follows:

<pre>'12.34.56.78'.sub('.', '').to_i</pre>

This is a bad idea because two IP addresses can result in the same integer: 12.34.56.78 and 123.4.56.78 are both valid IPs. The correct way is as follows:

<pre>'12.34.56.78'.split('.').collect(&:to_i).
              pack('C*').unpack('N').first</pre>

To understand this seemingly obfuscated train wreck, let's look at the data in this chain after each method call:

<pre>'12.34.56.78'.split('.'). #=> ['12', '34', '56', 78']
  collect(&:to_i).        #=> [12, 34, 56, 78]
  pack('C*').             #=> "\f\"8N"
  unpack('N').            #=> [203569230]
  first                   #=> 203569230
</pre>

I would guess that most Ruby developers are unfamiliar with pack and unpack. These allow you to create and extract data into and out of binary-packed strings. In the third method call, you'll see the result is `"\f\"8N"`. This strange-looking string is really just 4 bytes of data, the numbers 12, 34, 56, and 78 in binary form, put into a string. We then unpack that 4-byte string into a 4-byte integer (with network byte order).

This is also the same problem solved by the C library method `inet_aton`, which is implemented as part of [Ruby's `IPAddr` class](http://www.ruby-doc.org/stdlib/libdoc/ipaddr/rdoc/classes/IPAddr.html). Thus, there is a much simpler alternative:

<pre>require 'ipaddr'
IPAddr.new('12.34.56.78').to_i   #=> 203569230</pre>

But what fun is that!

PS. Yes, you can have multi-line method chains simply by leaving the dot at the end of the line. Ruby then knows to look for a method call on the next line. Both snippets are valid Ruby! You should, of course, indent appropriately.
