---
{"dg-publish":true,"permalink":"/ressurser/snippets/git-cheatsheet/","dgHomeLink":true,"dgPassFrontmatter":false}
---

# Git Cheatsheet
**Rebase**
```powershell
git fetch
git rebase --interactive origin/master
```

**Create Alias**
```powershell
git config --global alias.cob "checkout -b"
git config --global alias.st "status -sb"
git config --global alias.cm "commit -m"
git config --global alias.pf "push --force"
git config --global alias.pu "push -u origin HEAD"
git config --global alias.rb "rebase -i origin/master"
git config --global alias.dad "!curl -s https://icanhazdadjoke.com && git add"
```