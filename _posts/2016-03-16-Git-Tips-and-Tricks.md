---
layout: post
title: Git Tips and Tricks
categories: [Git, Tips, Tricks]
tags: [Git, Tips, Tricks]
fullview: false
comments: true
description: Git Tips and Tricks. This article is for all those who want to know about how to work with Git as well as learn more than a list of simple commands such as git init, git add, git commit and git push.
---

This article is for all those who want to know about how to work with Git as well as learn more than a list of simple commands
such as ```git init```, ```git add```, ```git commit``` and ```git push```.

I note that this article is subject to change from time to time and/or updated with new Tips and Tricks about working with Git.

#### So, let's begin with a simple

Suppose that you've committed but you do not like the content or you have made any bug or 
you are for any other reason wish undoing or reset recent changes in your repository.

For these situations Git provided powerful command ```git reset```

#### git reset HEAD

You make some changes in your file and run ```git add file.ext``` command
but forgot to do something, and not yet ready to commit to do and want to return to ```modified``` state.

{% highlight text %}
User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git add file1.md

User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   file1.md
{% endhighlight %}

Just run command ```git reset HEAD```

{% highlight text %}
User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git reset HEAD
Unstaged changes after reset:
M       file1.md

User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   file1.md

no changes added to commit (use "git add" and/or "git commit -a")
{% endhighlight %}

and make changes.

#### git reset HEAD~n soft and hard

Sometimes there are situations when you create a lot of commits with small changes which then can be placed under a single commit, 
thereby reducing the number of commits in your repository.

To do this, you can first run the command ```git log --oneline```
to make sure the number of commits that you would like to make a reset.

{% highlight text %}
User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git log --oneline
12ef4c7 commit-5
9ddfba4 commit-4
a864575 commit-3
309149d commit-2
f32a951 commit-1
bb65e59 commit
61d288d Init
{% endhighlight %}

Then just run the command ```git reset --soft HEAD~n``` where `n` is number of last commits for reset.

```--soft``` key moves branch to HEAD~n and stopped at this place. Without making changes to files.

{% highlight text %}
User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git reset --soft HEAD~5

User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   file1.md
        new file:   file2.md
        new file:   file3.md
        new file:   file4.md
        new file:   file5.md
{% endhighlight %}

Now you can create a new commit and then run the command ```git log --oneline```
to see what happened.

{% highlight text %}
User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git commit -m "Five Changes"
[master 4448563] Five Changes
 5 files changed, 2 insertions(+), 1 deletion(-)
 create mode 100644 file2.md
 create mode 100644 file3.md
 create mode 100644 file4.md
 create mode 100644 file5.md

User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git log --oneline
4448563 Five Changes
bb65e59 commit
61d288d Init
{% endhighlight %}

In addition ```--soft``` key, ```git reset``` have key ```--hard```
which moves branch to ```HEAD~n``` with hard reset all changes in repository to the specified `n`.

{% highlight text %}
User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git log --oneline
4448563 Five Changes
bb65e59 commit
61d288d Init

User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git reset --hard HEAD~1
HEAD is now at bb65e59 commit

User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git status
On branch master
nothing to commit, working directory clean

User@HOMEPC ~/Desktop/Git-Tips-and-Tricks (master)
$ git log --oneline
bb65e59 commit
61d288d Init
{% endhighlight %}
