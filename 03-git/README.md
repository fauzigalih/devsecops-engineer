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
