---
layout: post
title: Sinatra Routes in Orbit
author_url: http://www.linkedin.com/pub/fabio-mascarenhas/a/b32/5a6
author: Fabio Mascarenhas
---

[Orbit 2.1.0](http://keplerproject.github.com/orbit) dispatcher now accepts anything that answers to a `match`
method for its routing patterns and returns a list of captures, instead of just strings that it interprets as
standard Lua patterns. This means that you can now easily plug other kinds of pattern matchers, with the
caveat that you only have the `PATH_INFO` available for matching.

As an example, I made a plugin for having [Sinatra](http://www.sinatrarb.com/)-like routes in your Orbit application, available
[here](http://gist.github.com/222620) for Orbit 2.1.0 and through
[this rockspec](http://luarocks.org/repositories/rocks-cvs/orbit-cvs-2.rockspec) if you have Git
installed and want to track the bleeding edge of Orbit. It has as much of the Sinatra routes as I
could understand from their test file: named parameters delimited by slashes or dots, splats delimited
by slashes or dots, and optional named parameters. The `test_routes.lua` script is a good example of what
is available.

This is "Hello World" example of how you can use this in an Orbit application:

<script src="http://gist.github.com/222642.js"></script>

You need to have [LPEG](http://www.inf.puc-rio.br/~roberto/lpeg) installed, just a matter of `luarocks install lpeg`. 

The implementation essentially compiles routes to an LPEG pattern that receives a string and a table for putting
the extracted params, and if the string matches fills this table and returns it; otherwise it returns `nil`. The
entry point for the compiler is the `route` pattern. It splits the route definition into parameters, optional
parameters, splats and literals, then builds the final pattern using a
[foldr](http://en.wikipedia.org/wiki/Fold_(higher-order_function)), because splats have to match everything up to
the rest of the route definition. 

As an optimization, the compiler builds two patterns instead of one, a "clean"
pattern, that does not capture anything, used to make evaluation of PEG predicates (for the splats) faster,
and a "capture" pattern that does the actual work (and is the final result).

I am thinking of exposing the WSAPI environment to the pattern matchers, so you can write routing plugins that
can match on the query string or on HTTP headers. This would also let me refactor Orbit to decouple
the dispatcher from the creation of the `web` objects. Sounds cool, but I have to think how actually useful
this would be!
