---
layout: post
title: Kepler Refresh
author_url: http://www.linkedin.com/pub/fabio-mascarenhas/a/b32/5a6
author: Fabio Mascarenhas
---

As some of you have already noticed, [LuaRocks 2](http://luarocks.org) broke the current Kepler
installer. [André](http://www.linkedin.com/in/andrecarregal) and I decided that instead of making
a new Kepler installer it would be a better idea to make an installer for
[WSAPI](http://keplerproject.github.com/wsapi) itself, to bury the idea of "Kepler" as a web
framework and leave it as just the name for the project that built WSAPI and other modules.
LuaRocks 2.0.1 has just been released, so I took the lid off the new versions of the modules
that I [released last week](/2009/10/22/stealth-updates.html) and released a new version of
WSAPI itself, version 1.2. This is the changelog:

* Adds time-based collection of Lua states to FCGI and Xavante launchers
* Adds `wsapi` launcher script, to start a Xavante WSAPI server
* Fixed `undefined media type` error
* Added `is_empty` utility function to check if a string is `nil` or `''`
* Fixed bug with empty bodies in `wsapi.xavante`, and added full http status codes to responses
* Changing order of evaluating `PATH_TRANSLATED` and `SCRIPT_FILENAME`, to make non-wrapped
  launchers work in Apache OSX
* Reload support for `load_isolated_launcher`

I have also created a new rock for WSAPI, *wsapi-xavante*. The `wsapi.xavante` connector now
belongs to this rock, and, most importantly, it has a new `wsapi` script that runs a Xavante
instance configured for WSAPI applications and static files. For example, the following line
starts a Xavante instance on port 80 and using `/var/www` as document root:

<script src="http://gist.github.com/220083.js"></script>

See `wsapi --help` for configuration options, including logging and reloading of Lua states
for each request.

The *wsapi* rock still has the base WSAPI modules, as well as the CGI launcher, `wsapi.cgi`, and
the *wsapi-fcgi* rock has the `wsapi.fastcgi` module and the `wsapi.fcgi` launcher.

There are also a installer that replaces the old `kepler-install`. It is `wsapi-install-1.2`,
available standalone [here](http://cloud.github.com/downloads/keplerproject/wsapi/wsapi-install-1.2)
and as an all-in-one tarball (for offline installation) 
[here](http://cloud.github.com/downloads/keplerproject/wsapi/wsapi-aio-install-1.2.tar.gz). This
installer works like the old kepler-install, but installs Lua 5.1.4, LuaRocks 2.0.1, and the
*wsapi-xavante* rock and its dependencies. The old *kepler-\** rocks won't be updated for LuaRocks 2.

I am preparing a release of Orbit that includes the Orbit Pages launcher and examples that used to
be part of the Kepler rocks. The same should happen with the CGILua launchers that were included as partof Kepler.
