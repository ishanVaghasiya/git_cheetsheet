# Git Cheat Sheet

Here : remote origin means you host like (github , gitlab, etc..)

A quick reference for common Git commands.

Clone new repository
```bash
git clone <url>
```

Add a remote repository URL named `origin` [alias] to your local Git repository.

add new url with name origin
```bash
git remote add origin
```

chnage alreay exist origin url
```bash
git remote set-url origin
```

```bash
git remote add origin <url>
```

### Commit process

SETUP
set username
```bash
git config user.name <user_name>
```

set email
```bash
git config user.email <email>
```

First you need to add you chnage into stash which you need to commit

1) Stash all changes
```bash
git add .
```

2) Add commit
```bash
git commit -a -m <your_message>
```

OR Stages all changes and commits them with a descriptive message.
```bash
git commit -a -m <your_message>
```

Push your changes into remote
```bash
git push
```
OR
```bash
git push [alias] <branch_name>
```


Update your local with remote
```bash
git pull
```
or
```bash
git pull [alias] <branch_name>
```

Undo Last Pull
```bash
git reset --hard HEAD@{1}
```

### Revert Commit Process

Delete the most recent commit, keeping the work you've done
```bash
git reset --soft HEAD~1
```

Delete the most recent commit, destroying the work you've done (use with custion)
```bash 
git reset --hard HEAD~1
```

Revert commit using commit id 
```bash
git revert <commit_id>
```

Remove Code up to Given Commit
```bash
git reset --hard <commit_id>
```

Fetches all branches from all remote (Helpful when someone create new branch in remote and it does not show in you local)
```bash
git fetch --all
```

Checkout to other branch
```bash
git checkout <branch_name>
```

Create new brnach from your local
```bash
git checkout -b <branch_name>
```



### Merge Process

Here i mention three way to merge one brach code to other branch
1 - rebase
2 - merge
3 - pull orign

1) Using Rebase
```base
git rebase <branch_name>
```

Continues the rebasing process if interrupted.
```bash
git rebase --continue
```

Aborts the ongoing rebase.
```bash
git rebase --abort
```

Skips the current commit during rebase
```bash 
git rebase --skip
```


2) Using merge
```base
git merge [alias]/<branch_name>
```

Continues the rebasing process if interrupted.
```bash
git merge --continue
```

Aborts the ongoing merge.
```bash
git merge --abort
```

Skips the current commit during merge
```bash 
git merge --skip
```


3) Using pull
```bash
git pull [alias] <branch_name>
```

+ When using git rebase, you are essentially rewriting the commit history of your current branch on top of another branch. This can be useful for keeping a cleaner history, but should be used with caution, especially if the branch is shared with others.

+ git merge is a straightforward way to combine changes from one branch into another. It creates a new merge commit to capture the combination of changes.

+ git pull is essentially a combination of git fetch followed by git merge. It fetches changes from the remote repository and then merges them into the current branch.

> [!NOTE]
>Always make sure to carefully review any changes, especially when rebasing or merging, to avoid unintended conflicts or changes in the codebase.


> Stash helpful for Temporarily store your incomplete changes without cluttering the commit history

Stash
```bash
git stash
```

Stash with untracked file (Here untrakced file means new file)
```bash
git stash -u
```

Real life scenario for use stash
You're in the middle of working on your new feature, with changes spread across multiple files.
Your changes are not ready to be committed, as the feature is still incomplete.
You need to address the bug immediately on the main branch to deploy a quick fix.
Instead of committing your incomplete changes, risking a messy commit history, you decide to use git stash.
Switch to main brach do your change push he code
Swtich to you incomplete working branch
Do stash apply and contiune your work

2)

Suppose you have completed all your changes in Branch X, but suddenly remember you made changes in the wrong branch. In this case:
```bash
git stash    #add -u  if you want to stash untracked file
```

```bash
git checkout <branch_name>
```

```bash
git stash apply
```


## Advanced 

#### Change Author Name in Commit(s)
if already available this backup refs/original/ folder as backup in git repo, remove this backup folder using. Otherwise it will block you to change the commit author name and email id:

```bash
rm -rf .git/refs/original/
```
For a Single Commit
Change author name and email for a specific commit.

```bash
git filter-branch --env-filter '
  if [ "$GIT_COMMIT" = "COMMIT_ID" ]; then
    export GIT_AUTHOR_NAME="NewName";
    export GIT_AUTHOR_EMAIL="newemail@example.com";
    export GIT_COMMITTER_NAME="NewName";
    export GIT_COMMITTER_EMAIL="newemail@example.com";
  fi
' --tag-name-filter cat -- --branches --tags
git push origin --force --all
```


For Multiple Commits
Change author name and email for multiple commits.

```bash
git filter-branch --env-filter '
  if [ "$GIT_COMMIT" = "COMMIT_ID1" ]; then
    export GIT_AUTHOR_NAME="NewName1";
    export GIT_AUTHOR_EMAIL="newemail1@example.com";
    export GIT_COMMITTER_NAME="NewName1";
    export GIT_COMMITTER_EMAIL="newemail1@example.com";
  fi
  if [ "$GIT_COMMIT" = "COMMIT_ID2" ]; then
    export GIT_AUTHOR_NAME="NewName2";
    export GIT_AUTHOR_EMAIL="newemail2@example.com";
    export GIT_COMMITTER_NAME="NewName2";
    export GIT_COMMITTER_EMAIL="newemail2@example.com";
  fi
' --tag-name-filter cat -- --branches --tags
git push origin --force --all
```
