---
layout: post
title:  "Never use git push --force"
date:   2023-03-19 git
categories: git
---

I don't see any reason to ever use force push with git push --force. There is a superior method to it, which we will discuss in this article.

I will not go into many details about how it internally works. My main goal is to convince you
to never use the plain --force option.

In the beginning, we will explore why we need some force option. Then we will talk about a better alternative. Let's dive in.

# How does git push work
If we use git push, it refuses to update a remote ref that is not an ancestor of the local ref that is used to overwrite it. This is called the "fast forward rule". 

In simpler words, this means that the commits we want to push should build on the commits that are already on the remote. In even other words, the remote branch has not diverged.

I think everyone experienced an error due to git push. Below, we see one case where the pushing failed.

In this case, I pulled the main branch. Then I made a commit on the remote main branch directly via github. Now, the remote main branch is one commit ahead of my local main branch. Thus, git push will not work. The error tells us that "fast forward" is not possible. That's because the last commit on the remote branch is not an ancestor of the commits we want to push.

In this case, we can just pull and put the commit on top. Then we can successfully push.

![Remote Diverged](/images/git_push_error.png)

Thus, we can only push if git can just move the ref pointer forward to the last commit of the commits we want to push. This is called a "fast-forward". 

More information can be found on the official git page: [3.2 Git Branching - Basic Branching and Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)



## Why git push is not enough
There are cases where fast-forward is not possible. Imagine we want to change the commit message of the last commit on the remote. We change the name, but this leads to a different commit id for this commit. If we now try to push, we get an error.

Git checks if the last commit on the remote is an ancestor of the commits we want to push. But it will not find it, because with the renaming we overwrote the commit id locally. This leads to the fact that we can't push anymore.

Here are some examples when a push is not possible:
* You want to change the name of a commit message
* You want to reorder some commits
* You want to append changes to the last commit with --amend
* You want to squash some commits
* ....

How can we solve this issue?
There is a flag that disables this "fast forward" check. Thus, you can completely mess with the git history, including the risk of losing commits. Please use it with care.

The flag is called --force. Therefore, many people use

```
git push --force
```

, which I strongly not recommend to use. The same is true with many GUI applications that have a checkbox for the --force option. This should be replaced with the better alternative as well.

## The better alternative
The better alternative is called --force-with-lease.

```
git push --force-with-lease
```

It also disables the fast-forward check. But it checks if something happens to the remote branch since we checked it out. In other words, it expects that the history we want to change is the one that we checked out.

It's like taking a lease on the revision we checked out. During pushing, it checks if anything has changed on the remote.

This check is what makes it superior to a force push. It helps to prevent 
the loss of commits.

Imagine that we change the name of the last commit. In the meantime, our colleague pushed a new commit to the remote. If we now just force push, the commit of our colleague is lost, and we actually don't get any feedback that something weird has happened.

If we use the -force-with-lease option, we would get an error during pushing that tells us that
something has happened on the remote since our last checkout. We check the remote and find out that our colleague committed something in the meantime. 

![Remote Diverged](/images/git_force_failed.png)



## Summary
I never had any use case where I needed a force push. I was always able to do my work with the more secure force with lease option. Thus, I recommend always using 

```
git push --force-with-lease
```

