---
title: "Testing Pull Requests"
menuTitle: "Testing"
---

### Why?

Let's face it, testing new features, bug fixes, etc is time consuming.  
But **you** can help.

While it's true that ultimately the [Collaborators](https://github.com/orgs/qmk/people) decide on things, any feedback is greatly appreciated and can help the whole project move forward.

Knowing how to test PRs also allows you to use the stuff in them even before they get merged.

### How?

It's actually quite easy, but assuming you are familiar with `git` and how its branching works at the basic level.

There are also multiple ways to do this (as is usual with `git`), this is just my workflow

1. Make sure you have the upstream QMK repo available as a remote:

    ```
    [erovia@ws]$ git remote -v
    github  git@github.com:Erovia/qmk_firmware.git (fetch)
    github  git@github.com:Erovia/qmk_firmware.git (push)
    origin  https://github.com/qmk/qmk_firmware.git (fetch)
    origin  https://github.com/qmk/qmk_firmware.git (push)
    ```

    If not, run `git remote add origin https://github.com/qmk/qmk_firmware.git`.

1. Check the number of the PR. It's the numbers after the title, like `#1234`

    ```
    [erovia@ws]$ git fetch origin pull/1234/head:review/1234
    [erovia@ws]$ git checkout review/1234
    ```

{{% notice note %}}
You can call the local branch anything you like, I just usally call them `review/<number>` so I know what it was for even later.
{{% /notice %}}

1. Use the feature, rebase it on the `master` branch, try to break it

    ```
    // Run from the feature branch
    // This will place the changes in the current branch on top of the master branch
    [erovia@ws]$ git rebase master
    ```

    ```
    // Once you are done
    [erovia@ws]$ git checkout master
    [erovia@ws]$ git branch -D review/1234
    ```

1. Add your experiences to the PR.

    There is a tab called "Files changed", you can suggest changes there or approve the PR.
