---
layout: post
title: WSAPI, nginx, and FastCGI
author_url: http://www.linkedin.com/pub/fabio-mascarenhas/a/b32/5a6
author: Fabio Mascarenhas
---

I must confess that I am not a big user of [WSAPI](http://github.com/keplerproject/wsapi)'s FastCGI
connector. I do not maintain any high-volume websites, and for testing and development it is so
much quicker and hassle-free to just start a Xavante instance; no need to mess with complex
configuration files, or log files. Even the [LuaRocks wiki](http://luarocks.org) is running on
a Xavante instance with an Apache in front, using `mod_proxy` in front. The only time Xavante
needs to be restarted is when the slice needs to go down for maintenance.

[A recent discussion](http://lua-users.org/lists/lua-l/2009-10/msg00833.html) on the Lua mailing
list prompted me to revisit the FastCGI launcher, to see if I could get it running under
[nginx](http://nginx.net) and OS X without much hassle, and to get acquainted with nginx, a web
server I only know by reputation. [Bertrand Mansion](http://www.mamasam.com/) had already pointed
the way in [a message](http://n2.nabble.com/Wsapi-and-spawn-fcgi-tp3809309p3810373.html) on the
Kepler mailing list, so it was a simple matter of finding my way around the nginx configuration,
and fixing a small bug in the snippet that Bertrand provided. This is what I ended up adding to
my `nginx.conf` file:

<script src="http://gist.github.com/217559.js"></script>

This configuration sets `PATH_INFO` and `SCRIPT_FILENAME` to what WSAPI's FastCGI launcher
and WSAPI applications expect. Notice there is no mention of `wsapi.fcgi` anywhere; that is because
nginx does not start FastCGI processes, you have to start them manually. To do that for `wspai.fcgi`
I needed to grab [spawn-fcgi](http://redmine.lighttpd.net/projects/spawn-fcgi/news), and then run
this from the command line:

<script src="http://gist.github.com/217564.js"></script>

This line spawns four worker processes (`-F 4`) using the `wsapi.fcgi` shell script that LuaRocks
installed. Now I just had to copy some WSAPI applications to nginx's docroot, and point the browser
to them, and everything just worked. I wanted to see how this setup compared with Xavante, so I ran
`ab -c 5 -n 1000` on the `hello.lua` script that does with WSAPI (in the `samples` folder) on both
nginx and plain Xavante. On my lowly Atom netbook I get ~200 req/s with Xavante and ~320 req/s with
nginx, a 50% improvement. Not bad! I have to try it on a real computer, and also benchmark nginx
proxying to a "tribe" of Xavantes instead of going directly to a single Xavante instance, but
these are for another post.

