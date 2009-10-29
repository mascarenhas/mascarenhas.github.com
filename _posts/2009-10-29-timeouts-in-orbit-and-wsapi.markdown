---
layout: post
title: Timeouts in Orbit and WSAPI
author_url: http://www.linkedin.com/pub/fabio-mascarenhas/a/b32/5a6
author: Fabio Mascarenhas
---

A [Xavante](http://keplerproject.github.com/xavante) or FastCGI instance running an
[Orbit](http://keplerproject.github.com](Orbit) application can be very long-lived. MySQL
has an annoying "feature" that makes it close a connection with a client in case the
connection has been idle for a long time, and `libmysqlclient` has an even more annoying
bug where it won't reopen a dropped connection automatically. Even worse, the error
code for a closed connection varies depending on the specific implementation of the
server. This used to be a problem for long-lived Orbit applications that kept connections
open so they wouldn't have to be reopened on every request.

Orbit 2.1.0  has a solution to this problem in the form of connections that have timeouts.
To create a timed connection you use the `orbit.recycle` function, passing a connection
maker and a timeout in seconds. The function returns a connection proxy that automatically
reopens the connection when it becomes stale. For example, if you want a connection that
reopens every hour you can do the following, where `todo` is your Orbit application:

<script src="http://gist.github.com/221829.js"></script>

An orthogonal solution to the same problem is also in the new launchers for
[WSAPI](http://keplerproject.github.com/wsapi) 1.2. If you open `wsapi.fcgi` this is
what you see now (comments were removed for brevity):

<script src="http://gist.github.com/221848.js"></script>

The `period` and `ttl` properties control the automatic recycling of Lua states by the
launcher. After `period` section the launcher goes through all the states it has allocated,
and throws out any state that is older than `ttl` seconds. So setting `ttl` to less than
the MySQL timeout will guarantee that all your requests will have a connection that has
not timed out, as the connection gets reopened when the launcher initializes a new Lua
state for your application.

I am open to any suggestions on how to deal with these reliability problems, as I want
WSAPI (and, by extension, Orbit) to be reliable without having to sacrifice persistence
(and thus performance) too much.
