---
layout: single
tags:
  - Blog
redirect_from: /356/本部落格遷移歷程/
---

# 本部落格遷移歷程

原本這個部落格是放在 [WordPress.com](https://wordpress.com/) 上
不過後來~~閒閒沒事~~想說還是學著架 WordPress 在自己租的主機上看看
於是就買個域名 然後試著遷過來了

這篇不是教學
細節不會多提 主要是分享架站的心路歷程

<!--more-->

## VPS

我原本就有在租 Vultr 的 VPS
平常拿來放一些 Discord 的 Bot
偶爾還可以拿來當日本的跳板
其實會想搬過來也是覺得租了一台 VPS 好像都沒幹嘛也是有點浪費

## 網域名稱

再來就是需要一個域名
原本我有時是直接使用免費的 [NCTU Domain](https://nctu.me)
不過後來覺得自己買域名比較~~好玩~~穩定

考慮了網路上看到的評價以及自身的喜好之後
再加上最現實的＄＄＄考量
決定在 [Porkbun](https://porkbun.com/) 買了這個 .one 的域名
第一年 7.5 鎂，算是可以接受

## WordPress

這部分其實比我想像中的還要簡單方便
基本上就是把 WordPress 載下來、依賴套件裝一裝、Nginx 設定一下
然後從 WordPress.com 匯出，再匯進自架 WordPress
基本上就遷完了

## TLS

都 2019 年了
學架個網站卻沒弄 TLS 完全說不過去
現在也有 [Let's Encrypt](https://letsencrypt.org) 和 [Certbot](https://certbot.eff.org) 這些超級方便懶人的東西
再網路上找個教學 把 Nginx 跟著設定一下就完成了

## CloudFlare

聞名 [CloudFlare](https://www.cloudflare.com) 已久
一直很想玩玩看這東西 現在終於真的要來碰這個
結果其實也沒什麼特別的 就一步一步照著做就結束了XDD
看到 CloudFlare 免費版提供這麼多功能
真的是佛心來的

## 後續調整

以上偏後台的東西都弄好後
就又回來調整 WordPress 了

首先目標是要盡量解決掉 Google 的 [PageSpeed Insight](https://developers.google.com/speed/pagespeed/insights/) 以及 SSL Labs 的 [SSL Server Test](https://www.ssllabs.com/ssltest/index.html)  上偵測出的問題

於是先開始研究一些比較常見的 Plug-In
視需求安裝、調整~~、踩雷~~
同時再根據有偵測出的問題
邊 Google 邊調整伺服器上的設定

之後才發現那些都簡單
最複雜的問題是部落格主題XDD
個人的設計感對不上自己的審美觀
怎麼調都覺得不好看

最後直接採用了 WordPress 內建的 [Twenty Seventeen](https://wordpress.org/themes/twentyseventeen/)
再根據自己的喜好做一些微調（除了模板提供可修改的部分，再微調一些 CSS）
畢竟自架的好處就是可以視自己需求客製化

## 結語

其實架這個部落格的初始目的也不是真的要經營
~~畢竟也沒人在看~~
主要是想自學一些相關的技能知識
在這前提下
自架 WordPress 當然比使用 WordPress.com 更適合
架起來的整體難度也比想像中簡單許多

至於架完之後的發文頻率...
就再看看囉XD
