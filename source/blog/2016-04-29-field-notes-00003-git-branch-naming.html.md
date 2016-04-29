---
title: Field Notes 3 - Push git branches with different name
date: 2016-04-29 3:25 UTC
tags: notes, git, branch, naming
---

Several times it has happened to me that I think "It would be really nice to
have a 'save as' like in Word for a git branch". What I mean is that it would be
useful to be able to save the current branch with a different name (mostly when
pushing it to Github).

This can result handy for many reasons. Maybe you are working in a team with the
rule that your initials have to be prepended to the name of the branch (and you
don't want to have them in your machine because is confusing and redundant). Or
you realize that the name of the branch is not that representative to the code
written and it would be better to change it. Perhaps you misspelled the branch
name and you would like to fix the typo.

It doesn't matter the reason, git should be able to cope with this, and it turns
out it does.

To be able to change the name of the branch on push, you can for example use
this command:


```bash
  git push origin sa-new-feature:sa-new-feature
```

As you see, after `git push` comes the name of the remote branch and then
`your-existing-branch:branch-you-are-pushing-to`. Thus, in this case, your
_new-feature_ branch will be pushed as _sa-new-feature_ in origin.


