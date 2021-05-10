---
layout: single
tags:
  - FreeBSD
  - Linux
  - Vim
redirect_from: /221/vim-對於-conf-檔案的-syntax-highlighting/
---

# Vim 對於 .conf 檔案的 syntax highlighting

前一陣子在使用 FreeBSD 時
發現開啟 rc.conf 時，vim 沒有將該檔案作為 conf 檔做 syntax highlighting
但是開啟其他 .conf 檔案時就有

<!--more-->

好奇問了一下萬能的 Google 大神
才知道 vim 對於 conf 檔的認定
除了附檔名要是 .conf 之外
還有**檔案第一行必須要是 `#` 開頭的註解**

於是我就在 rc.conf 最前面多加了

```conf
# rc.conf
```

重開 vim 之後就有正常的 syntax highlighting 了

---

## 參考資料

* [freebsd-stable - syntax highlighting of rc.conf with vim](http://freebsd.1045724.x6.nabble.com/syntax-highlighting-of-rc-conf-with-vim-tp3952377.html)
