---
layout: post
category : development
tags : [memcache, pecl, item, deletion]
---
{% include JB/setup %}

<img src="http://static.gosquared.com/images/liquidicity/09_12_06_geoffdev_790x100.jpg" alt="Dev with Geoff - Development time with our CTO Geoff Wagstaff" width="790" height="100"/>

Since <a href="http://memcached.org/">memcached</a> version 1.4.3, the item deletion function (Memcache::delete()) in the <a href="http://pecl.php.net/package/memcache">PECL memcache package</a> fails with an error similar to "CLIENT_ERROR bad command line format.  Usage: delete &lt;key&gt; [noreply]" due to a feature removal in memcache 1.4.0 and above. Details about this feature removal and the problems it caused have been <a href="http://code.google.com/p/memcached/wiki/ReleaseNotes144">published by the memcached team</a>.

The memcached team have recently released version 1.4.4, adding backwards compatibility support for clients such as the PECL memcache extension and rectifying this issue, but for those of you who would like to patch the PECL extension directly, I'm putting a <a href="http://www.gosquared.com/labs/memcache/memcache-3.0.4_patched.tgz">patched version of beta 3.0.4</a> up for download over at <a href="http://www.gosquared.com/labs">GoSquared Labs</a>.

This is a simple fix which uncomments the code in memcache_ascii_protocol.c and memcache_binary_protocol.c that pertains to the "linger time" of the deleted item. However, simply upgrading to version 1.4.4 of memcached should rectify the PECL extension problem anyway.
