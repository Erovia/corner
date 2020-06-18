---
menuTitle: Workflow
title: My personal workflow for QMK
---

Let's be honest, `git` can be intimidating.

![xkcd's take on git](https://imgs.xkcd.com/comics/git.png)

While it's true that `git` is an extremely powerful tool, most users can get by with only using a handful of subcommands.  
I recommend the book [Pro Git](https://git-scm.com/book/en/v2) for everyone really interesed in `git`.

So, let's see how I usually handle my QMK stuff, while keeping them up-to-date, too.

{{% notice note %}}
Every command need to be executed from your `qmk_firmware` directory!
{{% /notice %}}

### Setting things up

This part you only need to do once.

So, if you followed the [official getting started guide](https://docs.qmk.fm/#/newbs_getting_started), which you should, this should be the output of running `git remote -v`:

```shell
[erovia@ws]$ git remote -v
origin  https://github.com/qmk/qmk_firmware.git (fetch)
origin  https://github.com/qmk/qmk_firmware.git (push)
```

Create a personal fork of the official repo.

1. Log into your Github account
1. Go to https://github.com/qmk/qmk_firmware
1. Click on the "Fork" button

Add your fork as a remote and create a branch where you will be commiting your changes:

```shell
[erovia@ws]$ git remote add myfork git@github.com:<your_github_username>/qmk_firmware.git
[erovia@ws]$ git checkout -b my_stuff -t my_fork/my_stuff
```

{{% notice note %}}
You can name the remote and the branch anything you'd like, 'myfork' and 'my_stuff' were just an example.
{{% /notice %}}

Check if both remotes are available:

```shell
[erovia@ws]$ git remote -v
origin  https://github.com/qmk/qmk_firmware.git (fetch
origin  https://github.com/qmk/qmk_firmware.git (push)
myfork  git@github.com:Erovia/qmk_firmware.git (fetch)
myfork  git@github.com:Erovia/qmk_firmware.git (push)
```

### Working on your stuff

This is what I usually do on a more or less daily basis.

#### Adding things

If you'd like to work on your keymap/userspace/custom board.

```shell
[erovia@ws]$ git checkout my_stuff                                # make sure I'm on the personal branch
#   do my magic
#   you know, new keymaps and the like
[erovia@ws]$ git add keyboards/example/keymaps/erovia/keymap.c    # add the new/modified files to the staging area
[erovia@ws]$ git commit -m 'My new shiny keymap'                  # commit the changes with a nice message
[erovia@ws]$ git push                                             # push the changes to my Github fork
```

{{% notice tip %}}
You can `git add` multiple files or even whole directories and have their changes in one commit.
{{% /notice %}}

{{% notice warning %}}
Always commit your custom stuff into your own branch(es) or there will be dragons.
{{% /notice %}}

#### Updating

```shell
[erovia@ws]$ git checkout master        # make sure I'm on the master branch
[erovia@ws]$ git pull                   # update master with the new goodies
[erovia@ws]$ git checkout my_stuff      # go back to my personal branch
[erovia@ws]$ git rebase master          # rebase my stuff on top of the newly updated master
```

As long as you keep you don't modify things outside your keymap/userspace/custom board, this will allow you to update QMK without any difficulties.
