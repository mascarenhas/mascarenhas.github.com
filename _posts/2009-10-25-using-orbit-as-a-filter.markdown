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
<noscript>
{% highlight lua %}
require "sputnik.wsapi_app"
require "orbit"
require "wsapi.util"

local filter = orbit.new()

local sputnik = sputnik.wsapi_app.new{
   VERSIUM_PARAMS = { '/Users/mascarenhas/sputnik/wiki-data/' },
   BASE_URL       = '/sputnik.ws',
   PASSWORD_SALT  = '6QQQuj9A2aUqwKq5YQsCCG8hEZZX1Lf2praCyM1I',
   TOKEN_SALT     = 'WyYwaUJ5XyVJFrjPf0iD02Ux3qMibp1Oek2fEvGm',
}

function filter.sputnik(wsapi_env, node)
  if wsapi.util.isempty(wsapi_env.QUERY_STRING) then
    wsapi_env.QUERY_STRING = "p=" .. wsapi.util.url_encode(node)
  else
    wsapi_env.QUERY_STRING = "p=" .. wsapi.util.url_encode(node) .. "&"
     .. wsapi_env.QUERY_STRING
  end
  wsapi_env.PATH_INFO = "/"
  return sputnik(wsapi_env)
end

filter:dispatch_wsapi(sputnik, "/")
filter:dispatch_wsapi(filter.sputnik, "/wiki/?(.*)")

return filter
{% endhighlight %}
</noscript>

Just set `NICE_URL` in your Sputnik configuration to `BASE_URL .. '/wiki/'` and you are set. Of course
a trivial example like this could easily be done as a straight WSAPI application, but if you have
more rewrite rules then using Orbit's dispatcher will make your filter cleaner.
