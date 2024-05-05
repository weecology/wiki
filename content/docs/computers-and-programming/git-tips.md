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
