---
layout: single
tags:
  - Android
redirect_from: /232/android-訊息（簡訊-app）-尚未選取傳送簡訊時慣用的sim卡/
---

# Android 訊息（簡訊 App）- 尚未選取傳送簡訊時慣用的SIM卡

最近入手了 HTC U11
因為個人比較習慣 Google 官方的簡訊 App （[Android 訊息](https://play.google.com/store/apps/details?id=com.google.android.apps.messaging)）
就載了下來準備替換掉 HTC 內建的簡訊 App
但是要將其設成預設簡訊程式時
卻跳出「尚未選取傳送簡訊時慣用的SIM卡」的錯誤訊息

<!--more-->

不知道別的品牌的 Android 手機是不是如此
HTC U11 這一台並沒有能設定簡訊慣用 SIM 卡的位置
以中文錯誤訊息當關鍵字下去搜尋查不到解法
後來找到拿英文版的錯誤訊息當關鍵字找（`No preferred SIM selected for sending SMS messages`）
總算在 xda 找到透過 adb 的解法

adb 的使用方法就不詳述了，網路上隨便找都有
然後下

```shell
adb shell settings put global multi_sim_sms 1
```

指令中的「1」視手機雙 SIM 卡的情況修改
就能將該 SIM 卡設定成簡訊慣用 SIM 卡
也能成功將 Android 訊息設定成預設簡訊 App 了

---

## 參考資料：

* [Boban Stojanović (@swashta) \| Twitter](https://twitter.com/swashta/status/953226414676901889)
* [Android Messages - No preferred SIM selected for sending SMS messages \| XDA Developers Forums](https://forum.xda-developers.com/showpost.php?p=75218519&postcount=18)
