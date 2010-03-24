---
layout: post
title: Announcing WSAPI 1.3.3
author_url: http://www.linkedin.com/pub/fabio-mascarenhas/a/b32/5a6
author: Fabio Mascarenhas
---

I have finally made new releases of [WSAPI](http://keplerproject.github.com/wsapi), which the changes
that I alluded to in the [last post](/2009/11/14/whats-coming-in-wsapi-13.html), plus a few other
changes and bug fixes. This is the changelog:

* Fixed memory leak with reload option for persistent loaders that was introduced by the new
  Lua state life cycle management
* Added raw HTTP responses to wsapi.common; useful when rolling your own HTTP server/proxy
* Fixed segmentation fault when non-string is provided to lfcgi.getenv() (thanks to mkottman@github)
* Added CGILua bootstrap to wsapi.sapi, so CGILua can run without a kepler_init module present
* Added an `extra_vars` paremeter to wsapi.xavante.makeHandler and wsapi.xavante.makeGenericHandler, to
  let you pass extra variables in the WSAPI environment
* Added `overwrite` option to wsapi.request that tells the parameter parser to overwrite repeated parameters
  instead of collecting them in a list    
* Added a parameter `isolated` to the persistent generic loaders that controls whether you isolate
  each script in a Lua state or not
* Added parameters to the persistent generic loaders that let the user control the life cycle of Lua
  states: `period` sets how long WSAPI should wait between collecting stale states, and `ttl` sets the
  period after which a state becomes stale
* Fixed bug in wsapi.ringer that didn't let you use wsapi.input:read inside the response iterator
* Parameter `vars` for the WSAPI generic loaders that which variables WSAPI should check to get the physical
  path of the script, and in which order. Defaults to trying SCRIPT_FILENAME first and PATH_TRANSLATED second

I have been doing a lot of web infrastructure work for Lua these past months, and pushing this WSAPI release
lets me focus on [mk](http://github.com/keplerproject/mk) and [Orbit](http://github/keplerproject/orbit/tree/mk). There
are still some uncommited changes to WSAPI in the [mk branch](http://github.com/keplerproject/wsapi/commits/mk), and
this branch is where the development for WSAPI 1.4 is happening.

I hope I can find time to talk about what [mk](http://github.com/keplerproject/mk) is and about my plans for Orbit 3
soon. Most of what Orbit 2 is has migrated to mk, and with Orbit 3 I want to raise Orbit's level of abstraction.

Download either the [WSAPI installer](http://github.com/downloads/keplerproject/wsapi/wsapi-install-1.3.3) or the
[offline installer](http://github.com/downloads/keplerproject/wsapi/wsapi-aio-install-1.3.3.tar.gz) if you want to
install without a net connection. Or download and install [LuaRocks](http://luarocks.org) and `luarocks install`
 either `wsapi`, `wsapi-fcgi` or `wsapi-xavante`, depending on which way you want to launch WSAPI (CGI, FastCGI or Xavante).