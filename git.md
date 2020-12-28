# Git

## Create new branch

```bash
function newbranch() {
    git checkout -b "$1";
}
alias nb="newbranch"

# ex:
# nb develop
```

## Checkout branch

```bash
function checkout() {
    git checkout "$1";
}

alias b="checkout"
alias ma="git checkout master"

# ex:
# b develop
# ma
```

## Status

```bash
alias st="git status"
```

## Commit

```bash
function commit(){
        branch=`git status | head -1 | cut -d' ' -f3`
        git commit -m "$branch - $1";
}
function commita(){
        git add .
        branch=`git status | head -1 | cut -d' ' -f3`
        git commit -m "$branch - $1";
}

alias cm="commit"
alias cma="commita"

# ex:
# cm "Commit message"
# cma "Commit message"
```

```bash
function push(){
        branch=`git status | head -1 | cut -d' ' -f3`
        git push --set-upstream origin $branch
}

# ex:
# push
```

## Pull
```bash
alias pull="git pull"

# ex:
# pull
```