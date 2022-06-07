# Git Cheat Sheet
Information and best practices about using Git

# Caveat
A general caveat I make to this cheat sheet is that these are available commands, but not necessarily the correct 
commands that you want to use. Please understand the effects of running a command before you run it as some, like rebasing and force push, can change history.
 
# General

## Reset local repository branch to be just like remote repository HEAD
Removes local changes and checks out the branch to HEAD version.

`$ git fetch origin`

`$ git reset --hard origin/master`

## Cloning a Remote Repository
Creates a local copy of the entire repository.
`$ git clone [url]`

### Examples
Simple cloning of project called brycetest. This will create a local repository with the directory name brycetest, 
branch master, as well as creating a remote to GitHub with the name origin.

`$ git clone git@github.com:gaoagong/brycetest.git`

Clone project, but put it in a directory of a different name. Will still create a local branch called master with a 
remote called origin pointing to GitHub.

`$ git clone git@github.com:gaoagong/brycetest.git [directory_name]`

## Show All Branches

`$ git branch -a`

## Switching Between Branches
Switches from the current branch to branch_name branch.

`$ git checkout [branch_name]`

## Adding Changed Files to the Changelist
Adds modified files to the cache to be later committed.

`$ git add [path_to_file | directory]`

## Removing Files from Being Tracked
It will keep the file.

`$ git rm --cached <file>`

If you want to remove a whole folder, you need to remove all files in that folder recursively.
`$ git rm -r --cached <folder>`

## Committing Changes
Commits the file changes that you have added to the cache via git add.

`$ git commit -a -m 'some message'`

## Merging a Branch
Merges the changes in a branch into the current branch.
Navigate to the branch you want to merge into, then

`$ git merge [branch_name]`

If there are no conflicting changes then merging will often fast forward changing meaning that it will simply update the 
pointer to the new commit hash. Use --no-ff to create a merge node even when it would normally perform a fast forward 
merge.

## Undoing a Merge

`$ git reset --hard [sha_of_C8]`

(https://git-scm.com/blog/2010/03/02/undoing-merges.html)

## Deleting a Branch
Deletes a branch locally.

`$ git branch -d [branch_name]`

## Printing the Status

`$ git status`

Prints the status of the current change list.

## Renaming a Branch

`$ git branch -m [branch_name] [new_name]`

If you have the branch currently checked out you can also use:

`$ git branch -m [new_name]

# Resources
[Git - Basic Branching and Merging] (https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

## Rebasing
Why NOT to rebase... Do not rebase commits that exist outside your repository. If you follow that guideline, you’ll be 
fine. If you don’t, people will hate you, and you’ll be scorned by friends and family. In general the way to get the 
best of both worlds is to rebase local changes you’ve made but haven’t shared yet before you push them in order to clean 
up your story, but never rebase anything you’ve pushed somewhere.

### General Rebasing Usage

`$ git rebase [base_branch] [topic_branch]`

Fast Forward master branch

`$ git checkout master`

`$ git merge [topic_branch]`

### Replay Branch Off Topic Branch into Master

`$ git rebase --onto master [topic_branch] [branch_to_replay]`

Fast Forward master branch

`$ git checkout master`

`$ git merge [branch_to_replay]`

### If you do incorrectly rebase and cause issues...
The following command can take some of the pain away if someone does a rebase and screws up history you have based your 
work off of.

`$ git pull --rebase`
or

`$ git fetch`

`$ git rebase teamone/master`
 
### Make --rebase option the default during git pull command

`$ git config --global pull.rebase true`

### Squash / cleanup commits before pushing
This command is useful for combining small local commits into a larger one before pushing. It can also be used to update 
commit messages. It opens your $EDITOR and then you can update the "pick" prefix to "squash" if that commit should be 
squashed.
 
`$ git rebase -i [branch_to_replay]`
 
Rebase starting from head to back 4 commits.
 
`$ git rebase -i HEAD~4`

### Resources
[Git - Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

# Branching

## Create New Branch

`$ git branch [branch_name]`

`$ git checkout [branch_name]`

or a shorthand way is:

`$ git checkout -b [branch_name]`

# Log

## Log Information

`$ git log --oneline --decorate`
```
1193711 (HEAD, test_branch) Merge branch 'master' of github.com:gaoagong/brycetest into test_branch
2d3ad14 (origin/master, origin/HEAD, master) Version 91
589944a Version 3
53c4f6f Version 90
be75714 Version 2.2
029ee59 Version 2.1
b3ddbb6 Initial commit
```

## Show Branch Divergence

`$ git log --oneline --decorate --graph --all`
```
*  1193711 (HEAD, test_branch) Merge branch 'master' of github.com:gaoagong/brycetest into test_branch
|\
| * 2d3ad14 (origin/master, origin/HEAD, master) Version 91
* | 589944a Version 3
|/
* 53c4f6f Version 90
* be75714 Version 2.2
* 029ee59 Version 2.1
| * 835abe4 (origin/test_branch) Version 3
| * 2f9d0ac Version 2
|/
| * 217b8a0 (origin/bgriner-patch-1) Update README.md
|/
* b3ddbb6 Initial commit
```

# Tags
 
## Listing Tags
Lists available tags.

`$ git tag`

## Searching for Tags that Match
Search for tags the match a given expression.

`$ git tag -l 'v1.8.5*'`

## Creating a Tag
Types: lightweight and annotated
Creates a tag with a commit message.
Annotated

`$ git tag -a [tag_name] -m '[my_message]'`

Lightweight (don't provide -a, -s, or -m for lightweight tags)

`$ git tag [tag_name]`

Previous Commit
If you have moved along in a branch and later decide you want to tag a certain commit you can do so. If 9fceb02 is the 
commit you want to create a tag on you could use the following:

`$ git tag -a [tag_name] 9fceb02`

## Pushing Tags Remotely
Tags aren't automatically pushed to remote branches. You must manually do this.

`$ git push origin [tag_name]`

## Checking Out Tags
To obtain a local copy of a snapshot defined by a tag use the following:

`$ git checkout -b [branch_name] [tag_name]`

## Resources
[Git - Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

# Working with Remote Branches

## Setting Upstream Remote
If you created a local branch and pushed it remotely and want to track it without specifying the remote and branch every 
time you can use the following command once. Usually remote will be origin.

`$ git branch --set-upstream-to [remote]/[branch_name]`

or

`$ git branch [branch_name] -u [your_new_remote]/[branch_name]`

## Tracking Remote Branches
Allows you to track someone else's remote branch.

`$ git checkout -b [branch] [remote_name]/[branch]`

## Set Upstream Tracking While Checking Out Remote Branch

`$ git checkout -u [remote]/[branch_name]`

## View Remote Branches
(run inside of the git repository in your file system)
Shows all short names of remote branches.

`$ git remote`

Typical result will be:
`origin`

## View Remote Branches Shorthand List
Shows all remote branches including short name and url.

`$ git remote -v`

```
origin git@github.com:gaoagong/brycetest.git (fetch)
origin git@github.com:gaoagong/brycetest.git (push)
```

## View Tracking Branches

`$ git branch -vv`

## Adding New Remote Repositories
Adds a new short name for a repository url for convenience in future reference.

`$ git remote add [shortname] [url]`

## Fetching Changes
Downloads the changes, but does not merge them.

`$ git fetch [remote-name]`

or to fetch all remote branch changes

`$ git fetch --all`

## Pulling Remote Changes
Downloads and merges changes.

`$ git pull [remote-name][branch_name]`

## Pushing Remote Changes
Pushes your local branch changes to the remote branch.

`$ git push -u [remote-name] [branch-name]`

The -u adds the upstream reference (used by git pull without arguments). The -u only needs to be used the first time you push your local branch remotely.

## Inspecting a Remote
Gives lots of information about remotes.

`$ git remote show origin`

```
* remote origin
  Fetch URL: git@github.com:gaoagong/brycetest.git
  Push  URL: git@github.com:gaoagong/brycetest.git
  HEAD branch: master
  Remote branches:
    bgriner-patch-1 tracked
    master          tracked
    test_branch    tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local refs configured for 'git push':
    master      pushes to master      (up to date)
    test_branch pushes to test_branch (local out of date)
```

## Renaming Remote Branches

`$ git remote rename [existing_branch_name] [new_branch_name]`

## Deleting Remote Branches

`$ git remote rm [remote_branch]`

or

`$ git push origin --delete [remote_branch]`

## How to tell which local branch is tracking which remote branch in Git?

`$ git remote show origin`

## Getting Rid of Old Tracking Branches (prune)

`$ git remote prune origin`

## Resources
[Git - Working with Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)
[Git Cheat sheet](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf)
[Git - Remote Branches](https://git-scm.com/book/en/v2/Git-Branching-Remote-Branches)
 
# Links
[Webinar: Git Workflow for SaaS Teams - YouTube] 
(https://www.youtube.com/watch?v=O4SoB3TFkjA&feature=youtu.be&list=UUL1yMVRMh3vxitPiVaXfkoA)
