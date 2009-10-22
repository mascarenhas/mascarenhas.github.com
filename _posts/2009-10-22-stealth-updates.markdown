---
layout: post
title: Stealth Updates
author_url: http://www.linkedin.com/pub/fabio-mascarenhas/a/b32/5a6
author: Fabio Mascarenhas
---

[LuaRocks 2.0](http://luarocks.org) is still using [LuaForge](http://luaforge.net) as its
default repository for rocks, and LuaForge's FTP access is offline for the time being,
which means that no rocks can be updated or added. Version 2.0.1 will have [a new default
repository](http://luarocks.org/repositories/rocks) that should be much more stable and
also easier to update using the new `luarocks-admin add` command.

What does this have to do with the title of this post? This week I have made new releases of
three Kepler libraries that had unreleased commits: [Xavante 2.1.0](http://keplerproject.github.com/xavante), [LuaFileSystem 1.5.0](http://keplerproject.github.com/luafilesystem), and [Rings 1.2.3](http://keplerproject.github.com/rings/). Rockspecs for these new releases are at the new repository only,
so I am waiting until LuaRocks 2.0.1 before announcing them on the mail channels. The updated
documentation pages have also been moved to Github, as the links above show.

The updates are pretty minor, so even those who have proactively updated their LuaRocks' config files
to use the new repository and are already getting the new packages shouldn't be worried. Rings 1.2.3
is a strictly bugfix release, correcting a minor bug. Xavante 2.1.0 is also mostly a fix to a
security bug in `xavante.filehandler` that let you request files outside of the document root, but
also adds a new parameter to `xavante.cgiluahandler` that restores the old CGILua behavior of
using a new Lua state for each request, as there are old CGILua scripts that depend on this "feature".

LuaFileSystem 1.5.0 is the one with the most changes, but it is backward compatible with 1.4.2. The
first new feature is more control over the `lfs.dir` iterator, adding `next` and `close` methods to
control the iteration explicitly (instead of only through a `for` loop). A simple example that
lists all files in the current directory up to a file named `foo`:

<blockquote>
{% highlight lua %}
local iter, dir = lfs.dir('.')
local file = dir:next()
while file and file ~= 'foo' do
  print(file)
  file = dir:next()
end
dir:close()
{% endhighlight %}
</blockquote>

The other feature added to LuaFileSystem 1.5.0 is directory locking via `lfs.lock_dir`. This function
takes a path and atomically checks if a lock file exists in that path and creates one if it does not
exist. In this case it returns a lock object that has a `free` method. If the path is already locked
it returns nil and the "File exists" message. These locks can be used as a crude mutex that is
portable and works both intra and inter-process. [Versium](http://sputnik.freewisdom.org/en/Versium)
uses this to guarantee that two Versium nodes are not being updated at the same time.

And one more thing: the new LuaRocks repository also has a rock for LuaSQL SQLite3 2.2.0, the SQLite3.
This is even more of a stealth release as the other ones, as it is possible that LuaSQL 2.2.0 will
be phased out in favor of splitting LuaSQL and letting the version numbers of the drivers diverge.
I also have got SQLite and MySQL drivers in the pipeline and should release them shortly. These
are the same drivers that have been available from the `rocks-cvs` repository for some time. I hope
releasing them in the main repository will give them more visibility.
