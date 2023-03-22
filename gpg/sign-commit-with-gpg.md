# How to sign git commit with gpg?

### 1) Get your signatures with gpg

```
gpg --list-signatures
```

### 2) Add your signature key in git config

```
git config --global user.signingkey AF536SGS6382GS8
```

### 3) This will make each commit signed

```
git config --global commit.gpgsign true
```

### 4) Let git know which gpg program to use sign?

```
git config --global gpg.program gpg
```

### 5) Add GPG_TTY environment variable to your profiles:

```
export GPG_TTY=$(tty)
```

or save it in .bashrc to make it permanent.

### 6) Finally your git config should look like this:

```
cat ~/.gitconfig
```

```
...

[user]
        email = user@email.com
        name = John Doe
        signingkey = AF536SGS6382GS8
[commit]
        gpgsign = true

...
```

### Please refer this gist (if exists) for more details:

[https://gist.github.com/trevorlinton/f3be63f1d4b1695e598d12d05f8d7cca](https://gist.github.com/trevorlinton/f3be63f1d4b1695e598d12d05f8d7cca)
