---
layout: post
title:  "Fact #1: Stand-alone scripts"
date:   2021-05-06 10:49:47 +0200
categories: jekyll update
---
**TL;DR**: Kotlin is a good fit for everyday scripting, as a type-safe replacement for bash or Python.

Did you know that Kotlin supports stand-alone scripts (files with `.main.kts` extension), with proper IntelliJ support?
Some time ago one had to use an external library [kscript](https://github.com/holgerbrandl/kscript) to make it possible,
but with release of Kotlin 1.4.0 it seems like JetBrains got it even further than `kscript`. For me it's perfect for any
kind of everyday scripting that is beyond simple shell one-liners.

For example, I wanted to have a script that get rids of local git branches that have been already gone from the remote.
Git lacks built-in local branch pruning (like `git fetch -p` for remote ones), and
[such bash-based solutions](https://stackoverflow.com/a/17029936/1102322) are cumbersome to write and crash whenever git
prints something that is not a list of branches, unless you handle it explicitly. I used a Groovy library
[grgit](https://github.com/ajoberstar/grgit) (a Groovy wrapper for [JGit](https://www.eclipse.org/jgit/), unfortunately
no longer maintained, but still usable) to have an object-based access to git. See
https://github.com/krzema12/PersonalConfigs/blob/master/scripts/removeLocalMergedBranches.main.kts. Shebang makes
executing it as easy as calling `removeLocalMergedBranches.main.kts` if it's in your `PATH`, from your git repo.

Demo:
![Kotlin scripting demo]({{site.baseurl}}/assets/kotlin-scripting-demo.png)
