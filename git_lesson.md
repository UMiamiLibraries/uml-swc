# Git Lesson

## SWC Git Lesson

[SWC Github Lesson](https://swcarpentry.github.io/git-novice/index.html)

## Github

Go to [Github](https://github.com) and follow the “Sign up” link at the top-right of the window.


## Git Configurations

```
git config --global user.name "Charles Brown-Roberts"

git config --global user.email "cgb37@miami.edu"

```

**Line Endings**

*Mac*

`git config --global core.autocrlf input`

*Windows*

`git config --global core.autocrlf true`



**Set the associated editor**

`git config --global core.editor "nano -w"`

**Disable line wrapping (no “soft wrap”)**

`-w`

By default, Nano will automatically wrap long lines onto the next line when they exceed the screen width.


**Set default branch name**

`git config --global init.defaultBranch main`


**Check your configuration**

`git config --list --global`



## Create an SSH key pair

```

ls -al ~/.ssh

ssh-keygen -t ed25519 -C "cgb37@miami.edu"

ls -al ~/.ssh

```


**Verify our authentication**

`ssh -T git@github.com`

We should get an error

**View the public key**

`cat ~/.ssh/id_ed25519.pub`

Copy/paste your public key to github

**Verify our authentication**

`ssh -T git@github.com`

You've successfully authenticated, but GitHub does not provide shell access.



## Create a Git Repo

**Create a new directory**

```
cd ~/Desktop

mkdir swc

cd swc
```

**Initialize a Git repo**

```
git init
```

**Inspect the new directory**
```
ls

ls -a
```

