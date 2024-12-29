---
layout: single
tags:
  - Linux
  - Raspberry Pi
last_modified_at: 2024-12-30
---

# 將 Raspberry Pi 接上 Epson L360 設定成印表機伺服器

最近搬到新家，客廳的印表機沒辦法拉線接到原本的電腦
所以想說弄一台 Raspberry Pi 接上去，然後透過 Wi-Fi 分享給家裡的其他電腦用
結果遇到了不少問題

<!--more-->

## 2024-12-30 更新

這篇的最終解法在幾個月後還是出現了問題
後來的解法可見[新的文章](./2024-12-30-usb-ip-printer.md)

## 前情提要

### 印表機

家裡的印表機是 Epson L360，是連續供墨的機型
用了快十年，還沒換過幾次墨水匣，滿划算的

本來在舊家放在客廳，是用 USB 接到一台 Windows 電腦上
再透過 Windows 的印表機共用功能，讓家裡的其它電腦可以用

不過搬到新家後，客廳配置不太一樣
放在弱電櫃上方，離電腦有一段距離，沒辦法直接接 USB 線

### 華碩無線路由器

本來是先透過 USB 接上華碩的無線路由器（RT-AC66U B1）
搭配這台路由器的[網路印表機伺服器功能](https://www.asus.com/tw/support/faq/114046/)，分享給其它電腦用

正常的時候，不論是用華碩 EZ 印表機分享或 LPR 協定分享哪種方式
都可以在客戶端連上印表機後，透過 Epson 本身的驅動程式正常列印
只有一個小問題，用 LPR 協定分享的話，客戶端啟動列印到印表機開始列印之間會有一段 10 到 20 秒的延遲

可是不知道是什麼原因
每次重開印表機的時候，路由器都有機率把它認成 modem 裝置
導致印表機分享功能沒有啟動
這時候就要重開這個功能、重插印表機的 USB 線，或是重開印表機看看能不能解決

對我來說已經算麻煩了
更不用說對家人來說根本不知道怎麼處理

再加上網路上搜尋類似問題的時候，也看到也有人在抱怨[華碩的網路印表機伺服器功能不太穩定](https://www.snbforums.com/threads/ez-printer-sharing-problems-solution.13915/)
所以就考慮看能不能換個方式

### Raspberry Pi

後來就想說搞一台單板電腦像是 Raspberry Pi 來當作印表機伺服器
剛好同事有一台 Raspberry Pi 3 Model B 可以借我
就跟他借來試試看
OS 是以 Debian 12 為基礎的 Raspberry Pi OS

## 在 Raspberry Pi 上安裝驅動程式，再透過 CUPS 分享（有問題）

首先嘗試的是在 Raspberry Pi 上安裝 Epson L360 的驅動程式
然後再用 CUPS 透過 IPP 協定分享給其它電腦用

CUPS 的安裝和設定這邊就不贅述了，沒有什麼特別的地方

### 安裝驅動程式

由於這台印表機有點年代了
什麼 driverless 或 IPP Everywhere 都沒有支援
所以還是要想辦法安裝驅動程式

然後 Epson 官方網站上也沒有提供 ARM64 架構的 Linux 驅動程式
所以也只能找第三方的方案
不過即使是第三方的方案，也沒有直接支援 L360 的驅動程式

幸好 Google 了一下發現不少人表示說可以用 Gutenprint 裡的 Epson L210 或 L310 驅動程式替代
只要安裝 `printer-driver-gutenprint`
之後在 CUPS 加入印表機的時候，model 選擇 Epson L210 或 L310 就可以了

### 分享印表機

然後參考 [ArchWiki 這篇教學](https://wiki.archlinux.org/title/CUPS/Printer_sharing)的 2.1 和 2.2 章節設定好分享
就可以在區域網路內其它電腦上看到這台印表機了
有開 DNS-SD 的話，Windows 應該直接按一下「搜尋印表機」就可以看到
沒有的話手動加入 `http://<HOSTNANME>:631/printers/<PRINTER_NAME>` 也可以

### 成功列印...？

Windows 加入這台印表機後會透過 IPP 協定去列印
除了選項比較少之外，基本上還是可以列印的

...本來是這樣想的
結果家父要列印的時候才發現邊界特別大
原本透過官方的驅動程式列印的時候，邊界只有 0.3 公分左右
現在這種方式列印的時候，邊界變成約 1 公分，尤其下邊界更是大到約 1.5 公分
Windows 上的程式要列印的時候就有類似「列印範圍超出紙張範圍」的警告
硬要列印的話，就會有一部分內容被切掉

不過我直接在 Raspberry Pi 上列印同樣的檔案，邊界就是正常的
所以應該是 CUPS 和 Windows 之間 IPP 溝通上有問題

#### 試圖解決

用 IPP 的工具去看 CUPS 回覆的資訊看不出所以然
後來乾脆直接去改 PPD 檔案，把裡面除了 A4 以外的紙張尺寸都刪掉
然後再到客戶端上重新加入印表機

結果上、左、右邊界好像比較正常了
可是印出來的時候似乎內容會變得更靠邊，變得比設定的邊界小
然後下邊界好像還是有點大
而且更嚴重的是有時候最後一行會整個錯位，然後一頁內容被印表機印到兩頁
整個還是不能用的狀態

## 透過 Samba 分享印表機（成功但需要額外處理）

後來發現說還可以有另一種方式分享印表機
就是不在伺服器端安裝驅動程式，直接以 raw 的方式透過 Samba 分享印表機
讓客戶端自己透過驅動程式直接跟印表機溝通

### 設定 Samba 印表機分享

可以參考上面提過的 ArchWiki 教學的 2.3 章節，或是 [Samba 官方 wiki 中分享印表機的教學](https://wiki.samba.org/index.php/Setting_up_Samba_as_a_Print_Server#Enabling_the_Print_Server_Support_in_Samba)
設定好後，從其它 Windows 客戶端打開檔案總管輸入 `\\<HOSTNAME>` 就可以看到這台印表機了
這種方法要客戶端自備驅動程式（沒有在 Sambda 伺服器上附上驅動程式的話）

### 成功列印

這種方法就完全是透過 Epson 官方的驅動程式直接跟印表機溝通
所有功能都是正常的，邊界問題也沒了
除了...

### 延遲問題

透過這種方式加入印表機
每次對印表機進行操作（例如列印、打開「印表機內容」或「列印喜好設定」視窗）都會有一個 30 秒左右的延遲
雖然不影響列印，但是每次操作都要等一段時間還是很怪

#### 觀察

去 Raspberry Pi 上打開 CUPS 和 Samba 的 debug log
看起來在客戶端進行操作的時候，同樣的 request 都會重覆發幾次，中間隔個幾秒
不過看起來沒有什麼錯誤訊息

再用 Wireshark 監控網路封包
發現客戶端在進行操作的時候，都會對伺服器的 3289 port 發送幾次 UDP 封包
不過伺服器沒有聽這個 port 當然沒有回應
模式大概是

1. 客戶端發送 UDP 封包到伺服器的 3289 port
2. 伺服器立刻以 ICMP port unreachable 回應
3. 客戶端等 3 秒左右後再發送下一個 UDP 封包
4. 重複這個過程 8 次

這樣算一算差不多就是將近 30 秒的延遲

#### 釐清

查了一下 UDP 3289 port 會發現 [Epson 官方許多機型的文件](https://download4.epson.biz/sec_pubs/l6260_series/useg/tc/GUID-5F7CDE39-97A0-4BC3-A096-A9F11CC2E7C0.htm)都會提到
是 Epson 用來透過 ENPC 協定「從 EpsonNet Config、印表機驅動程式及掃描器驅動程式等應用程式中發現印表機」
不過網路上都找不到 ENPC 協定的相關資訊或實作
有看到一些討論（例如 [escpos-php](https://github.com/mike42/escpos-php/issues/923) 和 [Stack Overflow](https://stackoverflow.com/questions/62870327/where-to-find-the-documentation-of-enpc-udp-port-3289-printer-discovery)），但是大家也都沒有詳細資訊

#### 解決

反正也不知道 ENPC 協定的 request 下來要回覆什麼
我乾脆就用 Python 寫個簡單的 UDP server
讓它聽 3289 port，然後都回一個空的訊息回去

🎉然後延遲就降到剩 5 秒左右了🎉
基本上就是省下了那段不斷重試 UDP 封包的時間
剩下的應該就是 Samba 或 CUPS 本身的延遲了

到這裡印表機的使用上就算是沒有什麼問題了

## 結語

當初想說印表機用得好好的，不想換新的
沒想到花了好幾天的時間在自己搞印表機伺服器上
幸好最後還是解決了

主要困難點大概是以下幾點：

1. Epson 官方沒有提供 ARM64 架構的 Linux 驅動程式
2. CUPS 相關的開放原始碼社群也沒有提供 Epson L360 的驅動程式，不確定會不會是這台印表機沒有在國外發售？
3. Epson 驅動程式本身用到的 ENPC 協定沒有相關資訊

以後要買印表機的話
應該要考慮直接買有無線網路分享功能的印表機...
近幾年的印表機應該對 IPP Everywhere 的支援比較好，也就比較沒有驅動程式的問題了
