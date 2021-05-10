---
layout: single
tags:
  - Android
  - Mobile Game
redirect_from: /245/mltd（ミリシタ）android-免引繼複製帳號資料/
---

# MLTD（ミリシタ）Android 免引繼複製帳號資料

這篇是要介紹如何在不用登入萬代帳號進行引繼的狀況下
將 MLTD（ミリシタ，アイドルマスター ミリオンライブ！ シアターデイズ）的帳號資料從原本的 Android 裝置複製到另一個 Android 裝置

注意：本篇需要對操作 Android 有一定的熟悉度

<!--more-->

這種方法相對於一般直接引繼有以下優點：

* ~~可以在多個裝置共存帳號~~
  * ~~因為是共用裝置 ID，所以不會讓舊的失效。~~
  * 現在這個已經沒有必要了，現在直接引繼也可以共存的樣子。
* 免登入萬代帳號
  * 像是有些人如我，不是很喜歡在模擬器登入一些帳號。
* 不用怕引繼轉移機種會把石頭清空
  * 因為沒有引繼這個動作，照理來說應該不可能清石頭。
  * 主要是擔心有些模擬器內建的瀏覽器會亂給 UserAgent，導致誤認成 iOS 會清石頭。

而缺點當然就是比較麻煩

---

基本上整個流程的目標就是要把舊裝置中的

```
com.bandainamcoent.imas_millionlive_theaterdays.v2.playerprefs.xml
```

這個檔案複製到新的裝置上

---

## 新舊裝置都能拿到 root 權限

如果新舊裝置都能拿到 root 權限
那就很簡單了

在新裝置裝好 MLTD 打開遊戲至少一次後
直接把舊裝置的

```
/data/data/com.bandainamcoent.imas_millionlive_theaterdays/shared_prefs/com.bandainamcoent.imas_millionlive_theaterdays.v2.playerprefs.xml
```

直接複製到新裝置的同一個位置
完成

---

## 新裝置拿不到 root 權限

如果新裝置拿不到 root 權限
那就用 adb backup + adb restore 解決

不過由於 MLTD 遊戲的檔案數量龐大
非常花時間 而且很容易失敗
可以考慮在舊裝置本身清完資料直接重新引繼
在最乾淨的狀態下 adb backup + adb restore

或是用 android-backup-extractor 把備份檔拆開
僅留下 `sp` 資料夾裡的 `com.bandainamcoent.imas_millionlive_theaterdays.v2.playerprefs.xml`
後再封回去
不過這種方法我沒試過
不確定可行性

---

## 只有新裝置都能拿到 root 權限

如果新裝置能拿到 root 權限
但是舊裝置拿不到

那就先用 adb backup 把 `com.bandainamcoent.imas_millionlive_theaterdays` 這個 app 備份起來

再用 android-backup-extractor 把備份檔拆開
可以在 `sp` 資料夾裡找到 `com.bandainamcoent.imas_millionlive_theaterdays.v2.playerprefs.xml`

再來就可以直接把這檔案丟進新裝置的
`/data/data/com.bandainamcoent.imas_millionlive_theaterdays/shared_prefs` 資料夾裡了（新裝置要開過遊戲至少一次）

---

## 參考資料

* [android-backup-extractor](https://sourceforge.net/projects/adbextractor/)
* [Full local backup infrastructure &#8211; Git repositories on android](https://android.googlesource.com/platform/frameworks/base/+/4a627c71ff53a4fca1f961f4b1dcc0461df18a06)
