---
layout: post
title: What's Coming in WSAPI 1.3
author_url: http://www.linkedin.com/pub/fabio-mascarenhas/a/b32/5a6
author: Fabio Mascarenhas
---

The [current HEAD of WSAPI](http://github.com/keplerproject/wsapi) has some nifty new features:

1. WSAPI has always encouraged the use of isolation between applications, with the
   Xavante and FastCGI launchers using [Rings](http://keplerproject.github.com/rings) to put each
   WSAPI application/script in its own separate Lua state. This is great for avoiding applications
   stepping on each others toes with misuse of globals, but can be a performance hit if your
   applications are doing unbuffered responses.
  
   WSAPI 1.3 will include the *isolated* option to these launchers, set to `true` by default, to
   control whether you want isolation or not. Just edit your `wsapi.fcgi` and set `isolated` to
   `false` and all your applications will run in the same Lua state.

2. WSAPI's standard request processing (`wsapi.request`) duplicates what CGILua used to do in case
   there is more than one field with the same name: creates a list with all the values of this
   field. WSAPI 1.3 adds an *overwrite* option to `wsapi.request` that makes the value of the field
   be the last value that appears, so all fields are always strings. If you want even more control,
   a *builder* option lets you provide your own behavior. A builder is a
   `function (params, field, value)` that is called for each field/value pair, with the table of
   parameters up to that moment, and mutates this table to do what it wants.

3. The WSAPI launchers try to detect whether the web server uses `PATH_TRANSLATED` or `SCRIPT_FILENAME`
   as the path to the script, but this is not foolproof, so there is now an option *vars* that lets
   you specify the order that WSAPI should check these variables, with the default being to check
   `SCRIPT_FILENAME` first.

There are also some fixes for non-critical bugs, that is why the next version is going to be WSAPI 1.3
and not WSAPI 1.2.1. I will be away for a week starting tomorrow, so I pushed the release to the
week of 23-27 of November. What is missing right now is the implementation of the *builder* option
that I outlined above, and changes to the documentation; right now these changes are documented in
my exchanges on the Kepler mailing list.
