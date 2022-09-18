## Useful Git commands

Get Git hash of file

    git hash-object test.txt

Diff current working tree with another commit

    git diff HEAD~2 --word-diff=color

Add origin of Git repository

    git remote set-url origin git@github.com:wmentzel/ubuntu-backup-script.git

Change origin of Git repository

    git remote add origin git@github.com:wmentzel/ubuntu-backup-script.git

Remove origin from Git repository

    git remote rm origin

Set upstream branch of current branch

    git branch --set-upstream-to=origin/master

Show branch graph

    git log --graph --oneline