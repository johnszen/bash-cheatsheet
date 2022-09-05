# git cheatsheet

## github keep asking for password
(for Mac) Passphrase stored in keychain may be lost, re-add.
```bash
eval `ssh-agent`
ssh-add ~/.ssh/id_rsa
```

## clone
`git clone git@github.com:johnszen/git-cheatsheet.git`

## branch from branch
`git checkout -b new-branch branch`

## sync with master
```bash
git checkout master
git pull
git checkout <branch>
git merge master
 ```

## move 2 commit back in current branch
```bash
git reset HEAD~2
```

## revert changes
```bash
# add a commit undoing the change to last 2 commits in current branch in local
git revert HEAD~2
```

## undo last commit in remote master 
Amend locally and push to remote
```
git checkout HEAD^ -- /path/to/file
git commit -m "..."
git push
```

Directly to remote
`git push -f origin HEAD^:master`
## undo last 2 commit in master
`git push -f origin HEAD^^:master`
OR
```bash
# add a commit to undo the change in the commit in local
git revert <commit-id>
# force push to server
git push -f
```

## undo last local commit and keep file changes
```bash
# check commit log
git log

git reset --soft HEAD~1

git log
```

## undelete a file
Reference: https://www.quora.com/How-can-I-recover-a-file-I-deleted-in-my-local-repo-from-the-remote-repo-in-Git
If the deletion has not been committed, the command below will restore the deleted file in the working tree.

`git checkout -- <file>`

You can get a list of all the deleted files in the working tree using the command below.
`git ls-files --deleted`

If the deletion has been committed, find the commit where it happened, then recover the file from this commit.
```bash
git rev-list -n 1 HEAD -- <file>
git checkout <commit>^ -- <file>
```

In case you are looking for the path of the file to recover, the following command will display a summary of all deleted files.

`git log --diff-filter=D --summary`

## git tree
`git log --graph --decorate`
 
# submodule

## clone with submodule init
`git clone --recurse-submodules -j8 git@github.com:johnszen/git-cheatsheet.git`

OR

`git submodule update --init --recursive`

## submodule update
`git submodule foreach git pull origin master`

## When submodule goes out of sync
Reference: https://www.deployhq.com/support/common-repository-errors/direct-fetching-of-commit-failed

go to the submodule e.g.

`cd common/modules`

checkout the submodule to specific module that exists

`git checkout 388453879f21e14e3b30473d3e2dba01adb2e6fa`
