# Notes of the course

## Git reset
To revert any changes on the last comment
```console
git clean -fd
git reset --hard
```
Go to the preview comment:
```console
git reset HEAD^ --hard
git push -f
```
Undone the last action (delete commit):
```console
git reset --soft HEAD~
git push -f
```
