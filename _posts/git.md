---
layout: post
title:  "Tutorial on git bisect"
date:   2022-02-10 git
categories: git
---

# Git Bisect Tutorial
Git bisect is a very convenient way to efficiently find the commit that introduced a bug. In this short tutorial I will show you how to use this feature. The corresponding code can be found here. If you want to directly experiment with the code you can directly jump to the [challenge] (https://github.com/steffenkoerner/git)

There are two ways on how to use git bisect. The manual and the automatic one. We will discuess both in the following.

# Manually using git bisect
Git bisect is a tool that helps you to efficiently find a commit that introduced a bug. The only information it needs is a start and end commit id, which specifies the range in which git-bisect should search. The algorithm uses a binary search, which runs in O(log(n)).

You need to go to the root of the repository you want to use and type "git bisect start" to state that you want to use this tool. Now, you need to specify the range in which you want to search. This is between the commit you know that the behaviour was
correct and the current status where it is wrong.

You need to specify this information by typing "git bisect bad <commit_id>" and "git bisect good <commit_id>.

This is all the information git bisect needs and it directly starts after you provide this information.

It now finds the commit in the middle of the range and checks it out. It also estimates how many more steps are needed to find the buggy commit. This can be seen in the image.

![Starting Git Bisect](/images/git_bisect_start.png)

It's now up to you to check if this revision is good or bad. You need to provide this information to bisect. This is done by
entering either "git bisect good" or "git bisect bad". After you typed this information git bisect shows you the next revision to check. You need to evaluate it again and provide the feedback. This proceeds until git bisect tells you the first buggy commit as can be seen in the image.

![Complete Git Bisect](/images/git_bisect_whole_process.png)

Nice job, you found the commit that introduced the bug.

## Aborting git bisect
If you did something wrong like for example typing git bisect bad instead of good, you can stop bisecting by typing "git bisect reset"

## Log of git bisect
If you want to see which revisions git bisect tested you can check the log. Just type "git bisect log". This will show something similar like the image below.

The process was very easy but still very tedious. There have been a lot of manual steps. You needed to manually check each revision and provide feedback The big questions is if we can automat this process. And the lucky answer is that we often can do it. We will have a look how to do it in the next section.

![Git Bisect Log](/images/git_bisect_all_log.png)


# Automatically using git bisect
In the last section we investigated on how to manually find the first commit that introduced a bug. Now we want to automate the process.

The first step is to think about how we manually would check if the current version is good or bad. Then specify this behaviour in a shell script. This can often be done by writing a unit test. This test is then called from the shell script. If the revision is good the script should return 0 and between 1 and 127 (inclusive), except 125, if the current source is bad. 
More details can be found by typing "git bisect --help".

We now have a criteria that automatically can evaluate if a revision is good or bad and call it by our shell script. The last open question is how to call this shell script. Luckily there is a very easy way. We just run

* git bisect start
* git bisect bad <commit_id>
* git bisect good <commit_id>

as we did in the manuall process.
Instead of reviewing this revision and providing manual feedback we tell git bisect to use our script.
This can be done by running git bisect run <script.sh> as can be seen in the image. Now git executes our script on each revision and tells us the buggy commit without and extra step.

In this example I used a unit test that I execute with bazel, which I call from the bisect/bisect_script.sh. You can see that the test is first executed for the revision ec4703baf1d21ebdf3b2b286b0a98481114d944a where it fails. In the next revision it passes and in the last one it fails again. Then git bisect returns the buggy commit. That's really awesome.

![Git Bisect Automatic](/images/git_bisect_automatic.png)

## The devil is in the details
This sounds very straighforward. Unfortunately, there are some details that often make it more complex. You can't just add a unit test to your existing test file. Because git bisect will checkout the next revision that needs to be reviewed. But this one doesn't have your test anymore.

Thus, you need to think about how you enable the execution of your new test in each revision. This highly depends on your test setup.

I normally just create a new test file that contains my test. Then I call this one in every step. That's what you can see in the image above. But this only works, as my test target automatically includes all files inside the test folder.

If you don't have such a test target, but I strongly recommend it, you still have the option to create the test in one of your tests files. You then git stash push <test_file> the test. Then after git bisect checks out a new revision you git stash apply the change and then you can execute the tests. Then you remove the changes and provide feedback with git bisect good/bad. These steps will be repeated until you found the buggy commit.

Of course you can put the steps again into a script, but you need to run it manually on each step as you can have merge conflicts. Of course you first try to run it completely automatically.


# It's your turn
I created a small C++ project with bazel that you can use to experiment with git bisect on your own. Try to find the buggy commit. The code is in the following repository. Just follow the README. Good luck my friend.

