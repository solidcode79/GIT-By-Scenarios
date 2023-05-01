# You setup your credentials, this stuff maybe quite different at each org, nothing like this at mine though : )
```
git config --global user.name "Your Name"
git config --global user.email "first.name@foobar.com"
```

# You want to setup your editor to be used for git
```
git config --global core.editor "vi"
```
# You want to see what all repositories are available on remote
```
git ls-remote remote-URL
```
# You want to see what all branches are available in a repository (and their last commit)
```
git branch -v 
```
# Your mentor tells you the repository URL on github and you clone it to current directory
```
git clone <repository url>
```
# It takes too much time ... clone in another directory without any history
```
git clone --depth 1 https://github.com/exampleuser/example-repo.git my-project-latest-snapshot-only
```
# You messed up your code, you want a fresh start and need to revert the code to it's original state
```
git log --oneline
git fetch --all
git reset --hard <commit-ID> # discard changes and move to commit ID
git reset --soft <commit-ID>
git checkout .
git clean -f
```
- A soft reset moves the HEAD pointer to the specified commit, but it keeps the changes made in the commit, unstaged. This command is useful when you want to undo the last commit but keep the changes so that you can modify them and make a new commit.
- A hard reset moves both the HEAD pointer and the working directory to the specified commit, discarding all changes made after the commit. This command is useful when you want to completely undo all changes since the last commit and start over from the specified commit. Including staging area.
- OTOH, checkout, discards all changes made to tracked files in the working directory, leaves staging area untouched.
- clean also deletes untracked files that were added


# You need do your first feature development so you create a branch
```
git checkout -b new_feature_branch
```
# You make some changes
```
git add . # all changed files including new added to staging area, or
git add .py #only all py files
```


# but you added a some changes by mistake, so you remove file from staging
```
git rm --cached testfile.py # remove from staging, changs still there
git reset HEAD^ -- testfile.py # unstaged, changes reset
```

# So, you not sure of the changes between the working tree and the stage/index area, you do the following to diff
```
git status # shows status of tracked and untracked files
git diff --cached # shows diff of staging area with last commit
```

# You commit
```
git commit -m "my first commit" #only commit
git commit -a -m "my first commit" #stage and commit
```


# A commit is big and you expect merge conflicts, so you dry run!
```
git commit --dry-run
```

# You figure you have a to delete a file 
```
git rm --cached wrongfile
git commit -m "Removed file"
```

# Also, a file has changes that you think you no longer want to commit
```
git reset testfile.py
```

# You commit again
```
git commit -m "my nth commit"
```

# But you don't like the commit message
```
git commit --amend -m "My last commit"
```

# At the end of the day your branch to remote, should your storage drive fail on you!
```
git push origin myBranch 
```
# You've made multiple changes in your branch, your boss calls and you need to push 1 single change to main first
```
git log myBranch # to figure out the commit ID, could've use --oneline
git checkout main
git cherry-pick <commit-id> # Resolve any conflicts that may arise during the cherry-pick process.
git commit -m "Merge commit <commit-id> from myBranch to main"
git push origin main
```

# Bad commit, just kill it rather than fix it!
```
git revert <commit-id>
```

# Your boss calls in the middle and wants you do do something else, with all those changes floating around, you stash them!
```
...
stituation one
git stash save "Work in progress"
git checkout the-branch-to-deal-with-crazies
... # deal with the crazies
git checkout original-branch
git stash pop
... back to situation one
```



# You make x commits, but they seem to clutter the history
```
git rebase -i HEAD~<number of commits to squash>
    ## This will open an editor, just change pick to s for each of the time except anyone of them!
    ## pick abc123 First commit message
    ## pick def456 Second commit message
    ## pick ghi789 Third commit message
```

# You want to see the differences between your branch and master, to build confidence before you merge!
```
git diff mybranch main
git diff --name-only mybranch main # only shows name of files changed rather than the diff itself!
```

# You want to rebase your branch and commit/merge
```
git fetch # maybe, other devs workedon your branch, let's sync to latest of my_branch
git checkout target_branch #Switch to the branch that you want to merge your changes into
git rebase my_branch # Applies my_branch to target_branch's latest, maybe merge conflicts
git rebase --abort # if no show or else git rebase --continue
git merge my_branch # This will create a new merge commit that includes all of the changes from my_branch.
```

# Once you've merge your branch into master and you want to delete your branch
```
git branch -d <branchname>
git push <remote> --delete <branchname>
```

# woops you accidentally deleted a branch
```
git reflog # never used it!
```

# You would like to know which all brnaches have been merged into the current branch
```
git branch --merged # doesn't show deleted branches by the way!
```

# In the standup a colleauges mentions they have pushed some major changes that you want to check further
```
git log --author="Author Name" branch_name #branch_name is optional
```

# You want to see the diff between the latest commit and the previous one
```
git diff <commit1> <commit2>
git diff HEAD~1 HEAD # or head-n and head
```

# There is a pesky bug in the production, you want to see the commit that introduced it using binary search
```
git bisect start # never used it, though :)
git bisect bad commitID1
git bisect good commitID2
```

# Well nobody found a bug like that, from your debugging you know someString change maybe caused this error, you want to check the log for past two weeks commits
```
git log -S "someString" --since=2.weeks -p 
```
 - p is for showing the diffs

# After you made several small commits to the main, your boss calls and you need to revert the last commit
```
git revert HEAD
git revert <commit-id>
```

# After so many calls from your boss, you want to  run some tests before each commit, so you setup a git pre commit hook
```
chmod +x .git/hooks/pre-commit
```

- pre-commit file should return 0 for success or 1 for fail
```
    #!/bin/sh
    # Run unit tests before commit
    echo "Running unit tests..."
    ./run_tests.sh
    RESULT=$?
    if [ $RESULT -ne 0 ]; then
    echo "Unit tests failed. Aborting commit."
    exit 1
    fi
    exit 0
```

# Finally, you want to tag the release
```
git tag -a v1.0 -m "my first release"
git push --tags
```

# You would like to see all tags available on the remote
```
git ls-remote --tags origin
```

# You need to setup a new repository and push it to remote! Devops or big leauge
```
git init  --bare # do not come with working or checked-out source files.
git remote add origin user@server:/path/to/my-repo.git # that this repo is related to this remote
git push -u origin master # this does the sync first time
git push # next time you can just say git push
```

# Just remember!

1. git merge: combine two branches into one, new commit for the merge is created!
2. git rebase: combine two branches but one two of they other with linear history
3. git cherry-pick: single commit from one branch to other
4. git pull: get changes from remote and merge to local
5. git fetch: only get the remote changes don't apply
