# Git &#8614; Pivotal Tracker integration

With this post-commit hook you will be able to add commits to your Pivotal Tracker account project stories as well as automatically starting, finishing and delivering them by making a git commit.

## Requirements

Ruby must be installed.

## Setup

##### 1. Get Pivotal Tracker API token and add it to git config

In order to use Git &#8614; Pivotal Tracker integration, you have to obtain your Pivotal Tracker API token. It could be found on your Pivotal Tracker profile page at the very bottom. Once you have that, add it to you Git project:

    git config pivotal.token {your-pivotal-tracker-token}

You can verify that config parameter is correct by running:

    git config --get pivotal.token

##### 2. Add 'post-commit' file to project git hooks folder

The best way is to clone this repositary and then create symlink in your project's `.git/hooks` folder:

    ln -s ~/Projects/git-pivotal-tracker-hooks/post-commit ~/Projects/<project_name>/.git/hooks/post-commit

Another way is just to put `post-commit` file into your project's `.git/hooks` folder. Of course you can assign the same code to different git hook. In this case rename `post-commit` to appropriate name. Read more about git hooks [here](https://www.atlassian.com/git/tutorials/git-hooks).

## Usage

##### 1. Add story ID to specific commits

A `post-commit` git hook that sends data to Pivotal Tracker about commit. If story Id is specified in commit message in square brackets:

    [#85421170] Bug in Internet Explorer

then a commit will be added to specified story in form of a comment. If story has an estimate and it is not yet started, such commit will mark story as started.

To automatically finish a story by using a commit message, include `fixed`, `completed` or `finished` in the square brackets in addition to the story ID. You may also use different cases or forms of these verbs, such as `Fix` or `FIXES`, and they may appear before or after the story ID. *Note: For features, this will put the story in the 'finished' state. For chores, it will put the story in the 'accepted' state.*

If you want to mark story as delivered, include `delivers` besides story ID.

It is also possible to assign multiple stories to a commit:

    [#12345677 #12345678] Ultimate bugfix

##### 2. Add story ID to feature branch name

A more approriate way to proceed is to reference story ID in the branch name so that all commits to given branch will be connected to specific Pivotal Tracker story. In order to do so you just have to mention story ID in the branch name with a hash in the beginning. Valid branch names look like this:

    feature/#85421170-some-crazy-feature
    feature/super-stuff-#85421170
    #85421170_version_upgrade

If branch name has a story ID in it and you add another story ID to a commit then both story IDs will be kept. This way you can pass additional parameters for example finish a story just by specifying `[finish]`.

More info can be found [here](https://www.pivotaltracker.com/help/api/rest/v5#Source_Commits)

## TODOs

1. Remove possible duplicate story IDs when branch has the same story ID as in a commit.
2. Add comment "Merged into <branch>" to Pivotal Tracker story when merging to `develop`, `master`, `qa` branches (check also cherry-picks).

## Changelog

* Don't send request to API if there's no story ID within commit message.
* If branch name contains story ID then add it to message so that there's no need to add story ID constantly to each message if whole branch is dedicated to specific story.
* Update to Pivotal Tracker API v5.

## Credits

Heavily influenced by [Tomasz Pewinski](http://pewniak747.info/2012/04/10/pivotaltracker-git-post-receive-hook/)