---
layout: post
title: Using Orbit as a WSAPI Filter
author_url: http://www.linkedin.com/pub/fabio-mascarenhas/a/b32/5a6
author: Fabio Mascarenhas
---

A nice trick that you can do with [Orbit](http://github.com/keplerproject/orbit)'s dispatcher is to
write [WSAPI](http://github.com/keplerproject/wsapi) filters, using the `dispatch_wsapi` method. This
method takes one function and one or more patterns, just like the other Orbit dispatch methods. If
the `PATH_INFO` matches any of the patterns Orbit's dispatcher passes the unmodified WSAPI environment
to the function, and just returns its results.

You can use this feature to easily add URL rewriting to any WSAPI application that does not depend on
any feature of your web server, like "nice urls" for [Sputnik](http://sputnik.freewisdom.org). Th
is a `sputnik.ws` script that uses Orbit to rewrite URLs such as
`http://server/sputnik.ws/wiki/node/name` to `http://server/sputnik.ws/?p=node/name`, the format
Sputnik expects:

<script src="http://gist.github.com/218345.js"></script>

Just set `NICE_URL` in your Sputnik configuration to `BASE_URL .. '/wiki/'` and you are set. Of course
a trivial example like this could easily be done as a straight WSAPI application, but if you have
more rewrite rules then using Orbit's dispatcher will make your filter cleaner.
