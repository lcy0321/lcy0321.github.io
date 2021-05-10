---
layout: single
tags:
  - Linux
redirect_from: /114/調整-kali-linux-的滑鼠滾輪滾動方向/
---

# 調整 Kali Linux 的滑鼠滾輪滾動方向

最近在 VirtualBox 上安裝了 Kali Linux
遇到的第一件困擾的事情就是滑鼠滾輪滾動的方向跟常用的方向相反
預設是往上滾滾輪，會向下捲動畫面
不是很習慣

<!--more-->

Google 了一下有找到調整方法
在 Terminal 中執行以下兩條指令即可：

```shell
gsettings set org.gnome.desktop.peripherals.mouse natural-scroll false
gsettings set org.gnome.desktop.peripherals.touchpad natural-scroll false
```

---

## 參考資料
* [gnome3 - How to *disable* natural scrolling? - Unix & Linux Stack Exchange](http://unix.stackexchange.com/questions/307928/how-to-disable-natural-scrolling)
