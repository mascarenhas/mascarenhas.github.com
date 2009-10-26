---
layout: post
title: LuaRocks with Symlinks
author_url: http://www.linkedin.com/pub/fabio-mascarenhas/a/b32/5a6
author: Fabio Mascarenhas
---

[LuaRocks](http://luarocks.org) can be used as a very nice build system for both pure Lua modules
and Lua/C modules. Coupled with a sandboxed Lua/LuaRocks installation it also makes a nice development
system (using `luarocks make`). But the changes introduced by LuaRocks 2.0 slowed down `luarocks make` to
virtually instantaneous (quick enough to assign a keybinding in your text editor, for example) to taking
more than a second, enough to break the edit-test flow. But monkeypatching comes to the rescue:

<script src="http://gist.github.com/218979.js"></script>

This script, `luarocks_ln.lua`, is a specialized version of `luarocks make` that symlinks files from your
source tree instead of copying them. So run it on a project you want to hack and all your edits in the
source tree are immediately reflected in code ran in your development environment. It works for both
Lua source modules and any scripts that your project installs. Just do a regular `luarocks make`
after your hacking session is done and everything does back to normal.

Notice that this is a pretty big hack that may break other LuaRocks commands, so do a regular `luarocks make`
before running any of them. Use this script only as a way to speed up your edit-test cycles. Obviously it
only works in Unix-like systems, but a version that works for recent Windows versions (that have `mklink.exe`)
is not hard to make, and is left as an exercise for the reader. Enjoy!

