---
title: "Git Tips"
summary: >
  Tips and snippets for doing useful things in git
---

## Deleting all merged branches (locally)

```sh
git switch main
git branch --merged | egrep -v "(^\*|master|main|dev)" | xargs git branch -d
```

Source: <https://stackoverflow.com/a/6127884/133513>

## Pretty git Log

(Hao) I have this in my ~/.gitconfig file to enable the git lg alias on command-line.

```
[alias]
lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%<(40,trunc)%s%C(reset) %C(reverse white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all
lg = !"git lg1"
```
