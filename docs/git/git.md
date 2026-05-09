# Git

## Stop tracking ignored folder

```bash
git rm -r --cached path/to/folder
git commit -m "stop tracking ignored folder"
```

`--cached` removes them from Git's index but keeps the actual files on disk. After this IDE's change list will clear and the folder will be properly ignored going forward.
