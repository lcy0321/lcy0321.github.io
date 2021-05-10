---
layout: single
tags:
  - git
  - MacType
redirect_from: /184/git-for-windows-與-mactype-神奇的衝突/
---

# Git for Windows 與 MacType 神奇的衝突

今天在 Windows 7 上用 Git Bash 的時候
遇到了一些奇怪的問題
主要是下 git rebase -i ... 的時候
一直跳出

> There is no tracking information for the current branch.
> Please specify which branch you want to rebase against.

<!--more-->


可是之前在 Windows 10 上用 Bash on Windows 下這指令的時候都沒問題
Google 了一下才發現有人發過這個 issue
解法竟然是...... 關掉 MacType
[when i git rebase -i error · Issue #710 · git-for-windows/git](https://github.com/git-for-windows/git/issues/710)

> ... I added `bash.exe` to MacType utility exclusion list, now all is good.

相關討論和 issue 可以點進去看
剛看到這解法還以為他來亂的
結果我把 MacType 暫時停用後
什麼問題都解決了...
