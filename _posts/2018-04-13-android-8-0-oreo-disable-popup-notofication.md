---
layout: single
tags:
  - Android
redirect_from: /234/android-8-0-oreo-停用彈出通知視窗/
---

# Android 8.0 Oreo - 停用彈出通知視窗

承[上一篇](2018-04-13-android-no-preferred-sim-selected-for-sending-sms-messages.md)
一買來 HTC U11 之後，立刻就把它升上了 Android 8.0 Oreo
結果遇到一個困擾的問題－有些 App 不給關會擋住一部分畫面的彈出通知

<!--more-->


上一台用的是 Android 6.0 Marshmallow
那時的「彈出通知視窗」的英文是叫做「Peek Notifications」
HTC Desire Eye 設定頁面有一個地方可以把這功能停用

Oreo 的「彈出通知視窗」的英文則是叫做「Heads-up Notifications」
設定頁面好像沒有地方可以整體停用這個功能
除非 App 本身有支援 Oreo 通知的 API
才能調成能正常接收通知，卻不會彈出會擋住畫面的通知視窗
而 Facebook 的 Messenger 不支援 ...
除了直接把 App 的通知停用之外
系統內似乎沒有別的辦法可以阻止它跳出通知視窗

幸好在 Reddit 有查到利用 adb 的解法

```shell
adb shell settings put global heads_up_notifications_enabled 0
```

就能將整體的彈出視窗功能停用

---

## 參考資料

* [Disable whatsapp peek notifications - oreo : GooglePixel](https://redd.it/74tdsn)
