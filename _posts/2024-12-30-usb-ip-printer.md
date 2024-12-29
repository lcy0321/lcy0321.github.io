---
layout: single
tags:
  - Linux
  - Raspberry Pi
---

# 透過 USB/IP 把印表機接到 Windows 電腦上

幾個月前的文章 [將 Raspberry Pi 接上 Epson L360 設定成印表機伺服器](./2024-10-20-epson-l360-on-raspberry-pi.md)
提到了需要把印表機接上 Raspberry Pi 來分享給家裡的其它電腦用
最後是透過 Samba + CUPS 做到
並對 ENPC 的請求封包回傳一個空的回應來解決驅動程式會卡住的困擾

結果之後發現印表機還是會出現一些問題...
後來研究半天，決定改成透過 USB/IP 來把印表機接到 Windows 電腦上

<!--more-->

## 前情提要

一些背景就不重複了，可以參考[前一篇文章](./2024-10-20-epson-l360-on-raspberry-pi.md#前情提要)
簡單來說就是要把一台接到 Raspberry Pi 的印表機
不透過實體 USB 線，弄到可以從 Windows 電腦上列印

### 前一篇文章的最終方案出現的問題

前一篇文章提到的 Samba + CUPS + 空 ENPC 封包方案
剛設定完後
列印都沒問題，操作延遲也都在可接受範圍內

但是過一段時間
打開印表機，從 Windows 電腦列印的時候
印表機卻會印到約 95% 就把紙送出來
然後停住、亮缺紙／夾紙燈

正常來說真的發生缺紙或夾紙的話
把紙處理好，按下印表機上的送紙鍵就會繼續列印

可是現在按下任何按鍵都沒反應
即使把印表機重開都沒辦法解決
最後發現要到 Raspberry Pi 上清掉列印工作、重開 CUPS 才能解決

然後過一陣子又會再發生一樣的情況

### 是 ENPC 封包的問題嗎...？

本來猜說是不是 ENPC 封包的問題
回應空封包可能會讓驅動程式出問題？

於是把印表機暫時接到一台 Windows 電腦上
裝好 Epson 官方的驅動程式，打開 Windows 的分享功能
到另一台 Windows 電腦上安裝驅動程式透過 SMB 連上那台印表機
再用 WireShark 看看正常情況下的 ENPC 封包是要回應什麼

結果發現這種情況下
客戶端的驅動程式根本就不會發送 ENPC 請求封包
這就表示對 Epson 的驅動程式來說
遠端印表機是接在 Samba + CUPS 的組合上，跟接到 Windows 電腦上，是有差異的

到這一步就可以知道要解決 Samba + CUPS 的問題不會很容易
甚至可能是不可能的了
畢竟我們不知道 Epson 的驅動程式具體做了什麼

## USB/IP

後來決定改試試看 USB/IP 協定
這個協定簡單來說，就是伺服器端把 USB 設備透過網路分享出去
客戶端就可以把這個設備當作是接在自己的電腦上一樣使用

詳細說明可以參考以下幾個連結：

- <https://www.kernel.org/doc/html/latest/usb/usbip_protocol.html>
- <https://usbip.sourceforge.net/>
- <https://wiki.archlinux.org/title/USB/IP>

其實一開始就有考慮過這個方案了
雖然 USB/IP 在 Linux 上是[內建在 kernel 裡](https://github.com/torvalds/linux/tree/master/tools/usb/usbip)，又已經有一段時間了
應該算穩定
但我的使用情境裡，客戶端是 Windows
之前查了一下，發現現在 Windows 上的 USB/IP 驅動程式好像還有一些問題
就想說先試試看其它的作法

不過現在看來
還是得試試看這個方案了

### 大致流程

#### 伺服器端（Raspberry Pi）

1. 安裝 [usbip](https://packages.debian.org/bookworm/usbip)
2. 啟動 `usbipd` 服務
3. 把印表機 bind 到 USB/IP 分享出去

#### 客戶端（Windows）

1. 安裝 [cezanne/usbip-win](https://github.com/cezanne/usbip-win) 的驅動程式
2. Attach 到伺服器端的印表機

### 額外說明

#### 伺服器端在重開、重接後自動 bind

由於 USB 裝置因為拔掉或關機而與系統斷開連線時
USB/IP 會取消 bind 的狀態
重新連上也不會自動 bind 回去
所以會需要一些機制來自動把印表機 bind 上去

另外 `usbipd` 這個服務在 Debian 上的 `usbip` 套件裡是沒有包含 systemd 服務的
所以也不會自動在系統開機時啟動

所以我就參考一些網路上看到的作法
寫了一個 systemd unit 檔案來自動啟動 `usbipd` 服務
以及一個 udev rule，在印表機連上時自動跑 `usbip bind`

<https://github.com/lcy0321/systemd-udev-usbip-printers>

#### 客戶端的驅動程式

前面提到 Windows 上的 USB/IP 驅動程式好像有一些問題
是指說歷史比較悠久、驅動程式有簽章的 [cezanne/usbip-win](https://github.com/cezanne/usbip-win) 已經沒在維護了
而有持續在開發的 [vadimgrn/usbip-win2](https://github.com/vadimgrn/usbip-win2) 目前還沒有驅動程式簽章
沒有簽章的話，Windows 必須要開啟 `TESTSIGNING` 模式才能安裝並使用這個驅動程式
這會降低系統的安全性，而且據說有些銀行的程式會不給在這個模式下使用

我還是先選擇舊的 `cezanne/usbip-win` 來試試看
使用的是有簽章的版本 [0.3.6-dev](https://github.com/cezanne/usbip-win/releases/tag/v0.3.6-dev)
目前看起來沒出現什麼問題
不過還是希望能有公司出現幫 `vadimgrn/usbip-win2` 的驅動程式簽章

#### 客戶端自動 attach

類似伺服器端的問題
伺服器端的 USB/IP 裝置因為關機之類的問題而消失時
客戶端也會自動取消 attach
所以需要一個機制來自動 attach

本來是想透過 Windows 的工作排程器來實作
每隔一段時間就執行一次 `usbip attach` 指令
反正如果已經 attach 了，再 attach 也不會有問題
伺服器端的 USB/IP 裝置不在的話，指令會失敗，也不會有什麼問題

不過這樣有兩個問題

第一是工作排程器的最小間隔是 1 分鐘
這就表示打開印表機到客戶端 attach 的時間最多會延遲 1 分鐘

第二是伺服器端上會出現一大堆 error log
顯示有客戶端試圖 attach 一個不存在的 USB/IP 裝置
或是試圖 attach 一個已經被 attach 的裝置

所以後來寫了一個 PowerShell script
只在目前沒有 attach 的情況下
執行 list 指令，看看伺服器端上的 USB/IP 裝置在不在
在的話就 attach；不在的話就不做任何事情
等個幾秒後再重複這個動作

然後再用 [winsw](https://github.com/winsw/winsw) 把它包裝成 Windows 服務
讓它開機時自動啟動，並在意外關閉時自動重啟

## 結論

目前試起來是沒有問題
不過前幾次的問題也是過一陣子才會出現...
所以還是要再觀察一陣子才能確定
