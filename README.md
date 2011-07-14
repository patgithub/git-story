GIT STORY
==========

Git story adds the 'story' git subcommand to git (via alias.story=!git-story.rb in the git config).
The story subcommand is used to manage stories as used in XP programming processes (kanban, scrum) terminology.

These stories are stored where they belong: alongside the code - in the a special branch called 'stories'.
This branch is initialised as empty branch (http://feeding.cloud.geek.nz/2010/03/abusing-git-storage.html)
on first usage of the story subcommand.

It is possible to check out this branch and edit stories directly, however git-story is using grit
(https://github.com/mojombo/grit) to directly access the objects in the git repository for fast access.

Story information is shared by all branches (title, description, comments, ...) but the status of a story
depends on how far the implementation is already in a specific branch. To reflect that, the status is
stored using git notes (http://www.kernel.org/pub/software/scm/git/docs/git-notes.html) and connected to
commits. (finishing a story can also be indicated in a commit message, starting a story is indicated by
creating a branch with the story name)

This results in state beeing inherited (and merged) between branches fully automatically when branching
or merging the code and a lot of state can automagically be determind from the repository itself.

usage
-----

    git story add {name}
    git story remove {name}
    git story show {name}
    git story edit {name}
    git story start/finish/accept/reject {name}

examples
--------

    git story add crazy_feature

the commit editor opens and you can describe the story.
(title first line followed by a blank line, followed by a description that may use markdown)

    git story show crazy_feature

displays title, description, requester (you) and status ('not yet started')

    git checkout -b crazy_feature
    git story show crazy_feature

displays title, description, requester, status ('started') and involved people (everyone commiting to this branch)

    git add ...
    git commit ...
    git story finish crazy_fetaure

    git checkout staging
    git story show crazy_feature

displays the status in this branch ('not yet started') and all branches with status that have commits from the feature branch

    git merge crazy_feature
    git story show crazy_feature

displays 'finished' - the merge also merged the status

    git story accept crazy_feature

sets the status to accepted in the 'staging' branch only

    git checkout master
    git merge crazy_fetaure

status in master is now 'finished' (merged from feature branch), status in staging is still 'accepeted' and will also be displayed

    git story accept crazy_feature

status is now accepted in staging and master!

