---
title: Field Notes 2 - Vim grep & quickfix
date: 2016-04-29 1:38 UTC
tags: notes, vim, quickfix, grep
published: false
---

Today I learned a little bit more about how to use vim for searches. The
simplest search that you can do with it is to use the `\search_term`. Depending
on how is vim configured in your system, that would partially highlight all the
matches and once you hit `enter` the cursor will be placed in the first match of
the searched string. 

Keep in mind that the search is done not by matching letters or works but by
returning all matches of the regular expression searched (that, by the way is
the same as matching the word/s that you wrote). This is important in case you
need to search for symbols or not conventional characters.

Nothing new under the sun so far. But what about searching in the whole
project? It's almost impossible to make much progress in a project without the
need of finding all the instances for a given word or expression.

Sure, you can go to the terminal and (if you have, for example, the silver
searcher installed, run `ag [search term]`. 

However everyone who uses vim can tell you that one of the major advantages of
this editor is that you don't have to leave it for _almost_ any reason.



