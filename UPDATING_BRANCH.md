# Keeping a Branch Up-to-Date

In order to keep a branch up-to-date with the remote master there are a few techniques.

1. Fetch and merge changes
2. Rebase and force push your changes

# Fetch and Merge Changes
1. `$ git checkout master`
2. `$ git pull` - pull down any changes from remote master to your local master
3. `$ git checkout [local_feature_branch]`
4. `$ git merge master`
5. If there are any conflicts then fix the conflicts, add the changes, and commit
6. `$ git push` - push your changes to the remote copy. If you haven't set up tracking then you will have to do 
something more like: `$ git push [remote_name] [feature_branch]` where ~remote_name~ is usually `origin`

Alternatively you can also do the following.
1. `$ git checkout [local_feature_branch]` - if you are not already on your local feature branch
2. `$ git fetch --all` - this fetches all of the changes for all remote branches on the repository
3. `$ git merge origin/master` - this will merge the remote master into the current branch
4. Fix any conflicts, add, and commit.
5. `$ git push` - push your changes to the remote feature branch.

# Rebase and Force Push Your Changes
1. `$ git checkout [local_feature_branch]` - if you are not already on your local feature branch
2. `$ git fetch --all` - fetches the changes for all remote branches on the repository
3. `$ git rebase origin/master` - move HEAD and re-apply your changes on top of it
4. `$ git push -f` - you must force push your changes as you have modified commits (do not use this technique if there
are multiple people using a feature branch, in general a branch with your username in front of it can be rebased at 
anytime)
