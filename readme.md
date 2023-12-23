# .gitmodules

## Add a new submodule

```bash
➜  Repository_B git:(master) git submodule add git@github.com:Mehrdad-Farshi/Repository_S.git
Cloning into '/home/mehrdad/projects/submodule/Repository_B/Repository_S'...
remote: Enumerating objects: 23, done.
remote: Counting objects: 100% (23/23), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 23 (delta 5), reused 22 (delta 4), pack-reused 0
Receiving objects: 100% (23/23), done.
Resolving deltas: 100% (5/5), done.
```

#### `ls` in Super Project Directory 
```bash
➜  Repository_B git:(master) ✗ ll
total 0
drwxrwxr-x 1 mehrdad mehrdad  4 Dec 14 20:26 DirB3
drwxrwxr-x 1 mehrdad mehrdad  4 Dec 14 20:26 DirB2
drwxrwxr-x 1 mehrdad mehrdad  4 Dec 14 20:26 DirB1
drwxrwxr-x 1 mehrdad mehrdad 22 Dec 17 16:39 Repository_S
```

#### `git status` in the Super Project Directory

```bash 
➜  Repository_B git:(master) ✗ git status 
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .gitmodules
        new file:   Repository_S

```

#### `cat .gitmodules `

```bash
➜  Repository_B git:(master) ✗ cat .gitmodules 
[submodule "Repository_S"]
        path = Repository_S
        url = git@github.com:Mehrdad-Farshi/Repository_S.git

```

> It’s important to note that
this file is version-controlled with your other files, like your .gitignore file. It’s pushed and pulled
with the rest of your project. This is how other people who clone this project know where to get the
submodule projects from


#### on super project we can see whats happening in submodules

> `git diff --cached`

    This command will display the changes that are staged and ready to be committed. It's useful when you want to review the modifications you've made and are about to include in the next commit.

    Without --cached (or alternatively, using git diff without any additional options), Git would show the differences between the working directory and the staging area, highlighting changes that haven't been staged yet.


#### `git diff --cached --submodule` in super project 

```bash
diff --git a/.gitmodules b/.gitmodules
new file mode 100644
index 0000000..abccf9f
--- /dev/null
+++ b/.gitmodules
@@ -0,0 +1,3 @@
+[submodule "Repository_S"]
+       path = Repository_S
+       url = git@github.com:Mehrdad-Farshi/Repository_S.git
Submodule Repository_S 0000000...4cca342 (new submodule)
```
**Tip** : for not every time mention **--submodule**

`git config --global diff.submodule log`


#### Repository_B git:(master) ✗ git commit -am 'add repository Repository_S as submodule'

```bash 
[master e9c9dd0] add repository Repository_S as submodule
 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 Repository_S
 ```

 #### push to origin the super project

 `➜  Repository_B git:(master) ggpush`
 ```bash
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 469 bytes | 469.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:Mehrdad-Farshi/Repository_B.git
   7cba25a..e9c9dd0  master -> master
```

## Cloning a Project with Submodules

`git clone git@github.com:Mehrdad-Farshi/Repository_B.git`

`➜  Repository_B git:(master) ll`

```bash
total 0
drwxrwxr-x 1 mehrdad mehrdad 0 Dec 17 17:48 Repository_S
drwxrwxr-x 1 mehrdad mehrdad 4 Dec 17 17:48 DirB3
drwxrwxr-x 1 mehrdad mehrdad 4 Dec 17 17:48 DirB2
drwxrwxr-x 1 mehrdad mehrdad 4 Dec 17 17:48 DirB1
```

`➜  Repository_B git:(master) tree Repository_S `
```bash
Repository_S

0 directories, 0 files
```

The Repository_S directory is there, but empty. You must run two commands: `git submodule init` to
**initialize your local configuration file**, and `git submodule update` to fetch all the data from that
project and check out the appropriate commit listed in your superproject

`➜  Repository_B git:(master) git submodule init `
```bash
Submodule 'Repository_S' (git@github.com:Mehrdad-Farshi/Repository_S.git) registered for path 'Repository_S'
```

`➜  Repository_B git:(master) git submodule update `

```bash
Cloning into '/tmp/Repository_B/Repository_S'...
Submodule path 'Repository_S': checked out '4cca34217faabf97a97d8a7b5e6c681b184cbbb3'
```
`➜  Repository_B git:(master) tree Repository_S `

```bash
Repository_S
└── DirA3B3
    ├── a3b3
    └── Sat 16 Dec 12:36:02 +0330 2023.log

1 directory, 2 files
```

you can combine the `git
submodule init and git submodule update` steps by running `git submodule update --init`. To also
initialize, fetch and checkout any nested submodules, you can use the foolproof `git submodule
update --init --recursive`.


### Working on a Project with Submodules

Now we have a copy of a project with submodules in it and will collaborate with our teammates on
both the main project and the submodule project.
### Pulling in Upstream Changes from the Submodule Remote

The simplest model of using submodules in a project would be if you were simply consuming a
subproject and wanted to get updates from it from time to time but were not actually modifying
anything in your checkout. Let’s walk through a simple example there.

If you want to check for new work in a submodule, you can go into the directory and run `git fetch`
and `git merge the upstream branch` to update the local code.

    $ git fetch
    From https://github.com/chaconinc/DbConnector
    c3f01dc..d0354fc master
    -> origin/master
    $ git merge origin/master
    Updating c3f01dc..d0354fc
    Fast-forward
    scripts/connect.sh | 1 +
    src/db.c
    | 1 +
    2 files changed, 2 insertions(+)


There is an easier way to do this as well, if you prefer to not manually fetch and merge in the
subdirectory. If you run `git submodule update --remote`, Git will go into your submodules and fetch
and update for you.

`➜  Repository_B git:(master) git submodule update --remote Repository_S`

Tip : FOR EACH !!

```bash
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 255 bytes | 255.00 KiB/s, done.
From github.com:Mehrdad-Farshi/Repository_S
   4cca342..758ab62  master     -> origin/master
Submodule path 'Repository_S': checked out '758ab62b3ca7818024662464c396f40811f17f09'

```


This command will by default assume that you want to update the checkout to the default branch of
the remote submodule repository (the one pointed to by `HEAD` on the remote). You can, however, set
this to something different if you want. For example, if you want to have the `Repisotory_S`
submodule track that repository’s `“development”` branch, you can set it in either your `.gitmodules` file (so
everyone else also tracks it), or just in your local `.git/config` file. Let’s set it in the `.gitmodules` file:

```bash
➜  Repository_B git:(master) ✗ git config -f .gitmodules submodule.Repository_S.branch development
```

`➜  Repository_B git:(master) ✗ git submodule update --remote`

```bash
Submodule path 'Repository_S': checked out '38b2795cf1dcec82a1cd916c4da64d9056b9d24e'
```
`➜  Repository_B git:(master) ✗ git status `

```bash
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   .gitmodules
        modified:   Repository_S (new commits)

no changes added to commit (use "git add" and/or "git commit -a")
```

TIP:

If you set the configuration setting `status.submodulesummary`, Git will also show you a short summary
of changes to your submodules:

```bash 
git config status.submodulesummary 1
```

`➜  Repository_B git:(master) ✗ gst`

```bash
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   .gitmodules
        modified:   Repository_S (new commits)

Submodules changed but not updated:

* Repository_S 4cca342...38b2795 (6):
  > masoud added as a file

no changes added to commit (use "git add" and/or "git commit -a")
```

At this point if you run` git diff` we can see both that we have modified our `.gitmodules` file and
also that there are a number of commits that we’ve pulled down and are ready to commit to our
submodule project.

```bash
diff --git a/.gitmodules b/.gitmodules
index abccf9f..c60d61b 100644
--- a/.gitmodules
+++ b/.gitmodules
@@ -1,3 +1,4 @@
 [submodule "Repository_S"]
        path = Repository_S
        url = git@github.com:Mehrdad-Farshi/Repository_S.git
+       branch = development
Submodule Repository_S 4cca342..38b2795:
  > masoud added as a file
  > mehrdad removed
  > mehrdad zahra sina added in submodule
  > mehrdad In The woods
  > new change from a developer
  > a new file from development branch

```

### nokte mohem 

Git will by default try to update all of your submodules when you run `git submodule update --remote`. If you have a lot of them, you may want to pass the name of just the submodule you want
to try to update.

```bash
git submodule update --remote repository_ASH
```

#### Pulling Upstream Changes from the Project Remote

Let’s now step into the shoes of your collaborator, who has their own local clone of the MainProject
repository. Simply executing git pull to get your newly committed changes is not enough:

*DOIT : clone a repo A to /tmp and after that try to commit new change in submodule of the current project and then try to pull changes in to the /tmp repository :*


**By default, the `git pull` command recursively fetches submodules changes**, as we can see in the
output of the first command above. **However, it does not update the submodules**. This is shown by
the output of the `git status` command, which shows the submodule is “modified”, and has “new
commits”. What’s more, the brackets showing the new commits point left (<), indicating that these
commits are recorded in MainProject but are not present in the local `Repository_S` checkout. To
finalize the update, you need to run `git submodule update`:

`➜  Repository_B git:(master) ✗ gst`

```bash
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   Repository_S (new commits)

Submodules changed but not updated:

* Repository_S 9d1c9b5...38b2795 (1):
  < ashe added

no changes added to commit (use "git add" and/or "git commit -a")
```


`➜  Repository_B git:(master) ✗ git submodule update --init --recursive`

```bash
Submodule path 'Repository_S': checked out '9d1c9b5a6bda0c6d6f84ffdfed2c2d6d5a14ec5a'

```

`➜  Repository_B git:(master) gst`

```bash
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

```
Note that to be on the safe side, you should run `git submodule update` with the `--init` flag in case the MainProject commits you just pulled added new submodules, and with the `--recursive` flag if any submodules have nested submodules.

## Working on a Submodule

So far, when we’ve run the `git submodule update` command to fetch changes from the submodule
repositories, Git would get the changes and update the files in the subdirectory but will leave the
sub-repository in what’s called a “**detached HEAD**” state. **s**
branch (like master, for example) tracking changes. **With no working branch** tracking changes, that
means even if you commit changes to the submodule, those changes will quite **possibly be lost** the
next time you run `git submodule update`. You have to do some extra steps if you want changes in a
submodule to be tracked.


In order to set up your submodule to be easier to go in and hack on, you need to do two things. You
need to go into each submodule and check out a branch to work on.

`➜  Repository_B git:(master) ll`
```bash
total 0
drwxrwxr-x 1 mehrdad mehrdad  4 Dec 17 17:48 DirB3
drwxrwxr-x 1 mehrdad mehrdad  4 Dec 17 17:48 DirB2
drwxrwxr-x 1 mehrdad mehrdad  4 Dec 17 17:48 DirB1
drwxrwxr-x 1 mehrdad mehrdad 60 Dec 17 19:54 Repository_S
```
`➜  Repository_B git:(master) cd Repository_S `

`➜  Repository_S git:(9d1c9b5) gco development `

```bash
Previous HEAD position was 9d1c9b5 ashe added
Switched to branch 'development'
Your branch is behind 'origin/development' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)
```


