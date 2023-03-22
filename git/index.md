# Some useful git commands

#### List all commits

```
git log --pretty=oneline
```

#### List all tags

```
git tag
```

#### Create a tag later by using commit checksum

```
git tag -a v1.2 9fceb02
```

if you want sign it

```
git tag -as v1.2 9fceb02
```

#### Push to remote repo

```
git push origin v0.0.0
```

#### Delete local tag

```
git tag -d v0.4.7
```

#### Delete remote tag

```
git push --delete origin v0.4.7
```


