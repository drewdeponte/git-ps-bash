# git-ps - Git patch stack

Git patch stack is an add-on command for Git that facilitates using a patch-stack [Git][] workflow while still using pull requests for peer review. It accomplishes this by managing your stack of patches and behind the scenes creating branches for patches you request review on and pushing them up to origin.

It consists of the following commands:

- `git-ps ls` - list current state of your stack of patches
- `git-ps pull` - fetches upstream origin/master and interactive rebases stack of patches on top of it
- `git-ps rebase` - interactively rebase your stack of patches onto origin/master to rearrange, squash, edit, etc.
- `git-ps rr -i <branch_name>` or `git-ps rr <branch_name> <commit_sha>` - request review of a patch
- `git-ps pub -i <branch_name>` or `git-ps pub <branch_name> <commit_sha>` - push a commit into mainline

## Installation

Simply copy the `git-ps` file into a directory that is in your `PATH` environment variable.

Personally, I like to put `git-ps` in a personal bin directory, `~/bin` I created and added to my `PATH` environment variable via my shell configs.

## Configuration

If you want to have it work with a different branch than `master`, and a different remote than `origin` you can configure it per repository by putting a `.git-ps-config` file in the root of the project with the following content.

```
remote=origin
base_branch=develop
```

This is extremely useful if your team works on a different branch as mainline.

## Configure Git for patch-stack

People using the patch-stack workflow often enable the pull-rebase option in their `.gitconfig` as follows:

```text
[pull]
		rebase = true
```

This makes it so that when you do a `git pull` it will automatically trigger a rebase on top of the tracked branches remote. So, if you are using `master` and you do a `git pull` to get recent changes from `origin/master`. [Git][] will automatically initiate a rebase of your changes that were sitting in `master` ontop of `origin/master` on to the new `origin/master`. This is exactly what you want as conceptually your stack of patches are always sitting on top of the latest `origin/master`.


## What is a patch-stack workflow?

The patch stack workflow is a different mental model for doing development locally. Instead of having feature branches and doing work inside them you generally do all your work directly on `master` by convention. You can think of your local `master` being a conceptual stack of patches (a.k.a. commits) on top of `origin/master`.

The idea is that instead of creating feature branches you simply make commits on your local `master` and perform interactive rebases to squash, reorder, ammend, etc. your patches as you continue to develop. This has an amazing benefit. **It eliminates the dependent branch problem.** You know the one where you created one change in a branch and submitted for review, but then you went off to create another branch on top of the last one, and so on. Soon enough you have to make changes as a result of the review to the first feature branch. And, down the feature branch rebasing train you go.

Patch stack workflows also expect each patch (a.k.a. commit) to be **small, buildable, logical units of work with valuable messages** providing the *what*, *why*, and any *contextual details around how*. This aids drastically in the code review process as each patch becomes a very focused piece of work with clear intent. It also helps conceptually "show your work", the logical progression of changes (patches) needed to accomplish some larger goal. These principles are as extremely valuable when doing historical spelunking to learn a code base or to iron out a bug. These principles bring even more value as they enable you to take advantage of all the amazing features of [Git][], e.g. `git bisect`, etc.


[Git]: https://git-scm.com
[GitHub]: https://github.com
[Bitbucket]: https://bitbucket.com
[skim]: https://github.com/lotabout/skim
