---
layout: single
tags:
  - git
redirect_from: /15/切割git-repo、並更改其部分內容/
---

# 切割 Git Repo、並更改其部分內容

因為一些原因
想把某個 Git Repo 中的資料夾另外分出來
分出來之後又發現其中當初不該丟上去的東西

<!--more-->

我又對 git 不熟Orz
就邊 Google 邊想辦法弄出來
在這做個紀錄

---

1. 先 clone 下來。
  `git clone [Repo位址]`
2. 切換目錄。
  `cd [目錄名稱]`
3. rebase -i 到欲更改 commit 的 parent commit。
  `git rebase -i [欲更改commit的sha]^1`
4. 想改的改一改，想刪的刪一刪。
  詳情參照參考資料中的教學。
5. 更新檔案變動。
  `git add -A`
6. 修改 commit。
  `git commit --amend`
7. rebase 回去。
  `git rebase --continue`
8. 把要留的資料夾切出來。
  `git filter-branch --prune-empty --subdirectory-filter [資料夾名稱]`
9. 更改遠端 Repo 的位址。
  `git remote set-url origin [Repo位址]`
10. Push，完成。
  `git push -u origin [BRANCH名]`

---

## 參考資料

* [Splitting a subfolder out into a new repository - GitHub Docs](https://docs.github.com/en/github/getting-started-with-github/splitting-a-subfolder-out-into-a-new-repository)
* [Git - Rewriting History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)
* [6. 使用 rebase -i 修改提交【教學3 改寫提交】 | 連猴子都能懂的Git入門指南 | 貝格樂（Backlog）](https://backlogtool.com/git-guide/tw/stepup/stepup7_6.html)
