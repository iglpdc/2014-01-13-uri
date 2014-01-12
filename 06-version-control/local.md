#Git local

## Outline

- What is version control?
- Motivation for version control
- Git: create a local repo
- Git: configuration
- Git: three stages for your files' life.
- Git: add, checkout, commit, and reset
- Git: log, status, diff
- Git: branch and merge

## What is version control?

Version control is a way to keep track of the changes in computer files. If you
work with computers, for example, programming or writing, you are probably
using some sort of version control system. You may include some information
about the version of the file in the filename, like `thesis_v_1.txt`. Or maybe
you copy versions of the files to some particular safe location, like a
pen-drive. The problem with this is that you end up with things like
`thesis_final_version_really_final.txt`, and that pen-drives had a tendendy to
get lost.

Computers are good doing dumb things that can be easily automated, like keeping
track of changes in documents. Git is a program to version control projects
used by professional programmers, and is becoming a essential tool in the
scientific realm too.

By the end of this lesson, you will be able to create a version control
repository for your projects, host this repository on the cloud, and share it
with others to collaborate on common tasks.

## Motivation for version control

For a vote:

- how many of you have lost important changes after pulling an all-nighter because
you hit *delete* instead of *save*?

- how many of you have missed a deadline because you forgot to copy over your
files from your laptop to the computer at work or viceversa?

- how many of you have been trapped in never-ending email exchanges to finish a
paper with remote collaborators?

Version control fixes all this, so you will get more time to focus on your
research.

## Creating a local repo 

In version control lingo, a repository, shortened to repo, is a directory in
your filesystem that is under version control.

To create a local repo, use `git init` in the directory you want to transform
into a repo.

A repo is just a regular directory with an additional hidden folder named
`.git`.

Let's create a repo to store our thesis. It could be also a research or
programming project, the important thing is that it'd be fairly complex, with
several different files.

```
$ mkdir thesis 
$ cd thesis
$ pwd
```

Now let's create a file in this directory, and show what's in the directory.

```
$ touch intro.txt
$ ls -F -a
```

Now let's make it a repo 

```
$ git init
$ ls -F -a
$ ls -F -a .git
```

## Git configuration

As said above, one of the most interesting things about version controls system
is that they allow much easier collaboration between people in teams. To
identify ourselves inside the collaboration, it's useful to configure a couple
of things. This has to be done only *once* per computer. Let's do it now.

Identify yourself with your name and email and fancy up messages with some
coloring:

```
$ git config --global user.name "Ivan Gonzalez"
$ git config --global user.email "iglpdc@gmail.com"
$ git config --global color.ui "auto"
$ git config --global core.autocrlf "input"
```

If you're on Windows change `input` to `true`.

Check that you have enterer **your** name and email:

```
$ git config --list
```

Now we are here, choose your favorite editor to use with git:

```
$ git config --global core.editor "gedit -w -s"
```

Depending on your taste, substitute to "nano", "vim", "subl -n -w".

In Windows and for Notepad installed in the standard location, use:

```
$ git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```


## Git: working dir, stage, history.

Git is a version control system for your files. This means that keeps track of
versions of your files. To keep track of versions, git organizes versions in a
chronological way, in what it's called history. The newest version in the
history is dubbed "HEAD". Note that all versions are kept in history, not
simply overwritten, as in a backup system as Dropbox. Although versions are
ordered chronologically into a history, they are not simple labelled by a time
stamp, instead versions can be annotated with a message describing the version
contents or particularities. In fact, this is not optional, but mandatory.

As versions are chronologically ordered, git keeps track only of the
differences between them. Therefore a new version is only the list of the files
that have changed from the previous one, plus the actual changes in each file.

Versions are snapshots of the files in the repo, so they just catch their state
in the particular moment you make the version. Here you have also a lot of
freedom. First, you decide which of the files in the directory are going to be
tracked. Maybe you need to have files there, like temporary files created
by some program, that are not really worth to track. Second, althought the
version is a snapshot of the files that you're tracking, not all the snapshots
have to be taken at the same time. That is, a version of your thesis can be
made by a intro that you polished this morning, a bib file that you finished
after lunch, and a couple of chapters that you have not touch since a month
ago.

To be able to make these customized snapshots, git takes the snapshots not
directy from your working directory but from what is called "stage" or "index".
So to make a version of your files, you first have to add them to the stage.
Once you added the snapshots of the files to the stage, you create a version by
commiting the stage to history.

Let's see all this in an example.

## Git: add, checkout, commit, and reset or how to make versions

Add files to the repo, i.e. make git to track their changes:

```
$ touch chapter_1.txt
$ git add chapter_1.txt
```

Once the files are being tracked and after making some changes, you should put
them into the stage to make a version; for this you also use `add`

```
$ git add intro.txt
```

Once you have all the files you want into the stage you can make a version by
committing the stage to history:

```
$ git commit 
```

You can add and commit selected files at once:

```
$ git commit intro.txt chapter_1.txt
```

or just all the files you are tracking:

```
$ git commit -a
```

You can also move files down for the history to the stage or the working
directory. This is useful to recover mistakes, start over from old versions, etc...

To bring to your working directory the last (`HEAD`) version in history.

```
$ git checkout HEAD --files intro.txt
```

You can bring back any other version counting back from the `HEAD`, like
`HEAD~1` for the previous to last version, or using its hash (more on this in a
bit).

To copy over the files in the stage to the working dir omit the version label,
i.e.:

```
$ git checkout  --files intro.txt chapter_1.txt
```

This is equivalent to discard the changes that you made since the last time you
have staged. 

You can also bring back a version in the history directly to the stage. This is
useful to undo a `git add`.

In all the commands above, you can bring the whole version instead of selected
files, omitting the `--files` flag. 

## Git status and log: see what's going on your repo.

Git associates a status to each of the files. You can inspect the status of
your files in the working directory and the stage with `git status`.

A file can be tracked or untracked, depending on whether or not git is tracking
its changes. When a file is tracked, git tells you whether the files has
changed with respect to the last version commited to history, and also whether
these changes have been staged or not.

```
$ git status
$ git add intro.txt
$ git status
$ echo "Some text in a new line" >> intro.txt
$ git status
```

## Git diff or how to see differences between versions

You can see the changes in your files between versions in the history,
the working dir or the stage.

To compare two versions, e.g. the last one and the previous in the history:

```
$ git diff HEAD~1 HEAD
```

To compare a version of the history with the working directory:

```
$ git diff HEAD~1
```

To compare a version of the history with the stage:

```
$ git diff --cached HEAD~1
```

And, maybe the most useful, to compare the stage and the working directory,
i.e. the changes that you have made but not yet staged:

```
$ git diff
```

Always `git diff` before a commit: you will know what you're commiting and write
better commit messages.

## Git log: inspect your history

One of the strong points of version control is that it encourages you to
describe what is new in each version. This is extremely useful to recover a
particular version, for example, where you code was working before making a
change or similar things.

To inspect the history do:

```
$ git log
```

You have information of who commited this particular version and their contact
info, the commit messages that describe the commit, and the label of the
version. 

Some of the useful variations of `git log` are:

```
$ git log -n 5  
```
shows only the last 5 versions, 

```
$ git log --grep 'words'
```
looks for 'words' in you commit messages. Very useful to find versions with
some particular changes.

To make things pretty you could use:

```
$ git log --graph --decorate --pretty=oneline --abbrev-commit
```

or 

```
git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
```

## Git: branching and merging

Although the point of git is to have all your versions ordered, sometimes it's
useful to break up the history. These parallel histories are called branches in
`git`.

All repos have a `master` branch by default. You can create branches for
several reasons. For example, in programming it's common to create a temporary
branch to fix a bug. Then, you have a team working on fixing the bug, while you
can keep the rest of your team working in the normal development tasks. New
features are also created in separate branches. 

After the work in the branch is done, the branch is merged back into the master
branch, and the two histories are put back together. 

Even if you work alone or in small projects, branch often; this allows you to
separate different tasks easily. Of course, also merge often. 

It's very common to have a master branch with your stable code or writing, and
a development branch for daily work. For example, you could write your thesis
using this scheme. You write and commit often to your develop branch, maybe a
few times a day. At the end of the day, you merge the master branch, but before
you pass all your work in the develop branch thru the spell-checker, check that
all bibliographic references are OK, etc... If someone asks you for a copy of
your thesis, you pull it out from the master copy and you know it's gonna look
good without having to do it every time you commit to the development
branch.

To create a new branch, called `development`:

```
$ git branch development
```

Once you created a branch, you have to check it out to work on it:

```
$ git checkout development
```

To see what branch are you:

```
$ git branch
```

To merge a branch back to another (say develop to master), you need to checkout
the branch you are gonna merge to first:

```
$ git branch
* develop
$ git checkout master
$ git merge develop
```

Merging two branches will add the changes made in the branch you are merging
from (develop) to the branch you are merging to (master).

Git will merge the branches unless there is a conflict. A conflict occurs when
the same line in a file has been changed in both branches since the last
version both branches have in common.

To be able to do the merge you will have to manually resolve the conflict,
saying which of the versions of the file you want to keep.






