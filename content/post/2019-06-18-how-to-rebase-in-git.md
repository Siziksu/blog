+++
type = "post"
title = "How to Rebase in Git"
summary = "This post will explain how to do the process of Rebase in Git."
date = 2019-06-18T12:51:07+02:00
categories = ["git"]
tags = ["rebase", "rerere", "git"]
image = "images/action-active-activity-2422264.jpg"
edit = ""
draft = false
+++
A good rule to follow when working with Git, is to use Rebase instead of Merge.

1. First of all, you will need to set the `rerere` command in Git to `true`. You will have to do this only once. Run the command `$ git config --global rerere.enabled true`.

    > With this command set to `true`, Git will remember all the changes that you are applying in each step of the Rebase. If you don't set it to `true`, on each step, Git will ask you on every step to solve the changes that you already solved in the previous steps.

2. Always create a new branch for your work.

3. After finishing your work, or every time develop is updated before your branch is merged, follow this steps:

    {{< highlight Bash >}}
$ git checkout develop     # Go to the branch to rebase from
$ git pull                 # Update the branch
$ git checkout your-branch # Go to the branch to rebase to
$ git rebase develop       # Rebase from develop to your branch
[$ git rebase --continue]  # Solve conflicts, stage them and then use this line to 
                           # continue as many times as needed
{{< / highlight >}}

4. After Rebase has finished, the most important part is to run this command: `$ git push -f`. This will push force your branch.

<br />

And that's it!
