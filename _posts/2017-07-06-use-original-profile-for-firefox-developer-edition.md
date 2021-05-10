---
layout: single
tags:
  - Firefox
redirect_from: /138/在-firefox-developer-edition-使用原本的設定檔（profile）/
---

# 在 Firefox Developer Edition 使用原本的設定檔（profile）

用了幾年的第三方編譯版 Firefox 已經一個多月沒更新了
在用這個版本之前 一直都是用官方的 Aurora Channel
現在更名為 Developer Edition （開發者版本） 並加入了一些新功能

想說換回來用用看
卻發現 Developer Edition 會使用獨立的一個設定檔（profile）（以.dev-edition-default結尾）
而且還不能改掉

<!--more-->

本來還以為只能在捷徑那邊加上「-p」參數去強制指定設定檔
還好 Google 了一下後 在 Reddit 有找到別的解法：
只要在

```
%APPDATA%\Mozilla\Firefox\
```

目錄下新增一個名為「ignore-dev-edition-profile」的檔案
Firefox Developer Edition 就會忽略獨立設定檔
轉而使用原本 Firefox 使用的設定檔

---

## 參考資料
* [How do I get Dev Edition to use my profile by default? : firefox](https://www.reddit.com/r/firefox/comments/4uhkwq)
