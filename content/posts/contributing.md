---
title: "Contributing"
weight: 15
---

This is a pretty basic and easy to understand workflow to work on keyboards, keymaps, features or anything in general that you **intend to get merged**.

For things that's for your personal use only, check out my [general workflow](/posts/workflow).

One of the best things about `git` is it's branching. It's fast and allows us to experiment without risking our code.

First things first, read the [official QMK Contributor guidelines](https://docs.qmk.fm/#/contributing) in its entirety.

### Branch

*when in doubt, branch*

```shell
[erovia@ws]$ git checkout master                   # make sure I'm on the master branch
[erovia@ws]$ git pull                              # making sure my local master branch is up to date
[erovia@ws]$ git checkout -b new_feature           # create a new branch for the new feature/keymap/keyboard
#   create new stuff
#   fix bugs
#   write documentation
[erovia@ws]$ git status                            # make sure all the changes are committed
[erovia@ws]$ git add file1 file2 fileN             # if not, add the new/modified files to the staging area
[erovia@ws]$ git commit -m 'Your commit message'   # commit the changes with a nice message
[erovia@ws]$ git push -u myfork new_feature        # push the changes to my fork
```

### Open the Pull Request (PR)

1. Go to https://www.github.com

1. Navigate to your `qmk_firmware` fork

1. Github will display a button to open a PR with your changes

1. Make sure the PR is from your fork's `new_feature` branch to the official `qmk_firmware` repo's `master` branch

1. Fill out the template and check in the **Preview** tab if it renders okay

1. Submit the PR

1. Wait patiently :)

### Merge conflicts

In some cases, someone else might have worked on the filed you changed. In this case a merge conflict will happen and you will be asked to resolve it.

Github will display conflicts at the button of the PR page.

```shell
[erovia@ws]$ git checkout master                   # make sure I'm on the master branch
[erovia@ws]$ git pull                              # making sure my local master branch is up to date
[erovia@ws]$ git checkout new_feature              # switch to the feature branch
[erovia@ws]$ git rebase master                     # rebase our changes on the newly updated master
```

```shell
#   git will go from commit to commit in the changes
#   if a conflict is found, it will display the filenames
#   open the files with and editor and look for the conflict indicators
#
#
#   <<<<<<< HEAD
#   the codeblock currently in master
#   ======
#   the same codeblock in the new_feature branch
#   >>>>>>> 38472386162057
#
#
#   remove the line you do not want to keep plus the markers
[erovia@ws]$ git add file1                         # mark the file or files as fixed
[erovia@ws]$ git rebase --continue                 # continue the rebase
#   the process above has to be repeated for every commit that has conflict
```

{{% notice tip %}}
The rebasing can be stopped at any minute with `git rebase --abort`.
{{% /notice %}}

```shell
[erovia@ws]$ git push -f       # push the changes to the new_feature branch on myfork, '-f' is needed, as history is rewritten
```
