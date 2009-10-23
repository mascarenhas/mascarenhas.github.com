---
layout: post
title: Cosmo Tricks
author_url: http://www.linkedin.com/pub/fabio-mascarenhas/a/b32/5a6
author: Fabio Mascarenhas
---

[Cosmo](http://cosmo.luaforge.net) is a very flexible template engine, and it has evolved quite a lot
since the version that shipped along with the first releases of [Sputnik](http://sputnik.freewisdom.org).
The documentation has grown accordingly, but the newer features that add more expressive power to
Cosmo end up buried among the earlier ones, so at first glance Cosmo may not seem as expressive as
template engines in other languages. For example, the first version of Cosmo only allowed iteration
via special generator functions that called `cosmo.yield` for each item they wanted to produce. This lead
to code such as:

<script src="http://gist.github.com/216341.js"></script>

Newer versions of Cosmo, though, automatically iterate over plain lists, so you can write the above as:

<script src="http://gist.github.com/216342.js"></script>

For this simple example you do not gain much, because the `do_list` function can be made generic, but the
feature becomes more useful when you have lists inside lists. Another often-neglected feature is arguments.
A Cosmo selector can have arguments, and they are passed as a table to the selector (so it must be a
function, or at least a `__call` metamethod). You can easily write "filters" with that:

<script src="http://gist.github.com/216347.js"></script>

Combine arguments with multiple subtemplates and you have the `cosmo.cif` function, which can be set as
an `$if` selector:

<script src="http://gist.github.com/216350.js"></script>

You can do a lot more by writing more selector functions. How about an `$import` selector that looks up a
template file in the filesystem and processes it? Or a `$markdown` filter, as you can have a long string
as an argument? [Orbit Pages](http://orbit.luaforge.net/pages.html) is one example of extendeding Cosmo with
several domain-specific selectors. If you have ideas for generic ones please let me know and I can add
them to the `cosmo` module alongside `cosmo.cif`.

