# Git &#8614; Pivotal Tracker integration

## Setup

In order to use Git &#8614; Pivotal Tracker integration, you have to obtain your Pivotal Tracker API token. It could be found on your Pivotal Tracker profile page at the very bottom. Once you have that, add it to you Git project:

    git config pivotal.token {your-pivotal-tracker-token}

You can verify that config parameter is correct by running:

    git config --get pivotal.token

## Usage

A `post-commit` git hook that sends data to Pivotal Tracker about commit. If story Id is specified in commit message in square brackets:

    [#85421170] Bug in Internet Explorer

then a commit will be added to specified story in form of a comment. If story has an estimate and it is not yet started, such commit will mark story as started.

To automatically finish a story by using a commit message, include `fixed`, `completed` or `finished` in the square brackets in addition to the story ID. You may also use different cases or forms of these verbs, such as `Fix` or `FIXES`, and they may appear before or after the story ID. *Note: For features, this will put the story in the 'finished' state. For chores, it will put the story in the 'accepted' state.*

If you want to mark story as delivered, include `delivers` besides story ID.

It is also possible to assign multiple stories to a commit:

    [#12345677 #12345678] Ultimate bugfix

More info can be found [here](https://www.pivotaltracker.com/help/api/rest/v5#Source_Commits)

## Credits

Heavily influenced by [Tomasz Pewinski](http://pewniak747.info/2012/04/10/pivotaltracker-git-post-receive-hook/)