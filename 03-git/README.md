# Git Documentation

List:
* [Git Basic](#git-basic)

## Git Basic
Configuration git:
```
# git config --global user.name "Your Name"
# git config --global user.email "user@mail.com"
```

Config text editor:
```
# git config --global core.editor "code --wait"
# git config --global diff.tool "default-difftool"
# git config --global difftool.default-difftool.cmd "code --wait --diff \$LOCAL \$REMOTE"
```

Show all config git:
```
# git config --list --show-origin
```

Init git to new directory:
```
# mkdir new-directory
# cd new-directory
# git init
```

Workflow git:
```
Working > Staging Index > Repository
```

Check git status:
```
# git status
```

Add / Modified file to git staging:
```
# git add filename
# git add .
```

Commit file to git repository:
```
# git commit -m "message in here"
```

Restore git staging to git working:
```
# git restore --staged fileame
```

Restore file modified:
```
# git restore filename
```

Delete file git:
```
# rm filename
# ls
# git add filename
# git commit -m "delete filename"
```

Restore delete file in git working:
```
# rm filename
# ls
# git restore filename
```

Restore delete file in git staging:
```
# rm filename
# ls
# git add filename
# git restore --staged filename
# git restore filename
```

Clean all new filename on git working:
```
# touch filename1 filename2 filename3
# ls
# git clean -f
# ls
```

See git log:
```
# git log
```

See git log in one line:
```
# git log --oneline
```

See git log flow graph:
```
# git log --oneline --graph
```

Show detail commit latest:
```
# git show HEAD
```

Show detail commit by id:
```
# git show id_commit
```

Compare git commit:
```
# git diff id_commit_old id_commit_new
# git diff 7e68cc0 HEAD
```

Compare git commit via text editor:
```
# git difftool id_commit_old id_commit_new
# git difftool 7e68cc0 HEAD
```

Rename file:
```
# mv filename filename1
# git add filename
# git add filename1
# git status
# git commit -m "rename filename to filename1" 
```

Git reset:
`--soft` move HEAD pointer, but don't change in staging index and working directory:
```
# git log --oneline
# git reset --soft 7e68cc0
# git log --oneline
# git status

how to back to last HEAD again, example 1e08cc5 is last HEAD:
# git reset --soft 1e08cc5
# git status --oneline
```

`--mixed (default)` move HEAD pointer, changed staging index to repository, but don't change in working directory:
```
# git log --oneline
# git reset --mixed 7e68cc0
# git log --oneline
# git status

how to back to last HEAD again, example 1e08cc5 is last HEAD:
# git reset --mixed 1e08cc5
# git status --oneline
```

`--hard` move HEAD pointer, changed staging index and working directory to repository:
```
# git log --oneline
# git reset --hard 7e68cc0
# git log --oneline
# git status

how to back to last HEAD again, example 1e08cc5 is last HEAD:
# git reset --hard 1e08cc5
# git status --oneline
```

Git ammend commit, is re-commit the previous commit:
```
# git log --oneline
# touch newfile
# git status
# git add newfile
# git commit --ammend -m "re-commit previous commit"
# git log --oneline
```

Git checkout, we can see previous version commit with specified file:
```
# git log --oneline
# git checkout 7e68cc0 -- filename
```

