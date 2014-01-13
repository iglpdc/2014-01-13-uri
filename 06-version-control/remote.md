# Github

## Outline 

- Github: get a free account
- Put your local repo in Github
- Git push & pull
- Adding collaborators
- Git clone: best backup system ever!

## Github

Github is a website that hosts git repositories. You can host your repos from
free as long as they are public, i.e. their contents can be seen by anyone.
Note that people cannot change the contents of your repo, only see them. You
can upgrade at any time to a paid account that admits private repos.

There are other websites that offer similar services, but Github is probably
the most popular.

Go to [Github](http://github.com) and get a free account. You are going to need the
password often in the class, so don't forget it.

## Put your local repo in Github

We're going to put the local repo we've created in the last class into Github.

- In your Github page, create a new repository. To make things easier, use
the same name as for your local repo. *Do not* click the to initialize
with a README file. 

- Open a shell and `cd` to the dir when you have your local repo.

- Copy the instructions from the second box into the shell in your computer. What this is doing is adding to
your local repo a remote origin, meaning that they share the same history.

We will see next how to move your changes between local an remote repo.

The normal workflow maybe different. Normally, you'll create a new repo
directly in Github, and then the associated local repo. 

## Git push & pull.

To move changes in files between local and remote repos, you have to use two
new commands:

```
$ git push
```
uploads your local history to the remote repo. It merges the new
versions that you have in your local repo into the history of the remote repo.

```
$ git pull
```
downloads the remote history into the local repo. It merges the new versions
that are in the remote repo into the local repo.

Obviously, to do the history merges both repos need to have a past version in
common. Otherwise, git won't know where to merge the new versions. You can only
push to the last version of the remote repo. This means that in practice you
always `git pull` before doing a `git push`. All this matters when there is
more that one person working on the repo.

Let's put our two repos in sync. As your remote history is empty but the local
have a couple of commits, we have to put the local history into the remote repo
first:

```
$ git push
```

You'll have to enter your username and password for your Github account.

Now, let's bring back the remote history to the local repo to be sure
everything is up-to-date.

```
$ git pull
```

Remote and local repos are synced. From now on, you work normally as with a
local repo. You make changes, commits, branches, merges, etc... and then once
in a while, when you have a good set of versions that you want to share, you
`git push` & `git pull`.


## Adding collaborators

Let's unleash the real power of Github: collaboration within teams. You are
going to share your repo with another student and both will contribute with
changes. 

Pair up. Each of you:

- `cd ..` to get out of your current directory.
- Then create a new folder. `cd` in that folder.
- Ask your pair for their GitHub project URL. (`"HTTPS clone URL"` down in the
right menu of the repo's main page).
- Have them add you as a collaborator (by clicking settings on the right menu of
the repo's main page. Then choosing collaborators and adding your username).
- `git clone <"HTTPS clone URL">`

This will give you a clone of their repo in your machine. As you are a
collaborator, you can push from and pull to the repo as it was yours.

So now, make some edits in your pair's repo. Commit that change and push it.
Have your neighbor do a refresh by running a git pull.

## Git clone: best backup system ever!

If you work with several computers, you probably have run into problems syncing
your files. With Github you can solve this easily. Simply `git clone` your own
repo in all the machines you are using. 

- When you arrive to a computer remember to `git pull` to download to this local
repo the changes that you have pushed from other machines into the Github
servers. 

- When you leave one computer, remember to `git push` to upload  to the Github
server the changes you made in this local repo.
