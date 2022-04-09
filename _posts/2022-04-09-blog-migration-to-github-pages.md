---
layout: single
tags:
  - Blog
---

# 本部落格轉移至 GitHub Pages

就某方面滿搞笑的

上一篇兩年多年的文章在講怎麼搬到自架的主機上
過一年多就又搬到 GitHub Pages
而且又過了快一年才發這篇文 Orz

<!--more-->

## GitHub Pages

[GitHub Pages](https://pages.github.com/) 讓人可以把靜態網頁放在 GitHub 上
由 GitHub 提供網域、空間和 TLS 憑證等服務

從自架 Wordpress 轉過來的好處就是省下了維護的成本
畢竟現在網路世界很可怕，自架伺服器都要定期上去更新一些外掛和伺服器系統本身的一些套件
TLS 憑證什麼的也要定期更新（雖然弄個 cron job 去自動更新也行）
轉到 Pages 上就什麼都不用管了

而且靜態網頁的效能肯定比動態網頁快不少
反正我本來就不需要像是留言板之類的功能

簡單來說就是更懶、更快、更安全

## Jekyll

雖然說是靜態網頁
可是也沒有硬到真的整份 HTML 都自己手寫
有很多種靜態網頁生成框架可以用，比較有名的像是 [Jekyll](https://jekyllrb.com/)、[Hugo](https://gohugo.io/)、[Hexo](https://hexo.io/) 等
用這些框架把網頁生出來再丟到 GitHub 上就可以了

而我這邊是使用 Jekyll
主要原因只是因為 GitHub Pages 有內建支援 Jekyll
只要把原始碼丟上去，GitHub 就可以直接產生出網頁了
可以再省下自己生完再部署上去的動作
~~更懶~~

## 從自架 Wordpress 轉移

雖然我 Wordpress 的文章也沒幾篇
不過要一篇一篇轉，對我這種懶人來說還是會嫌麻煩
所以我是用 Wordpress 上的 [Jekyll Exporter](https://wordpress.org/plugins/jekyll-exporter/) 外掛
幫我大致轉成 Jekyll 吃的格式之後，我再自己微調

具體 Jekyll 的設定什麼的，這裡就不寫了
基本上就是照著[官方教學](https://jekyllrb.com/docs/)跑就好了

比較特別的是因為新的 URL 格式不太一樣
有另外透過 [jekyll-redirect-from](https://github.com/jekyll/jekyll-redirect-from) 這個外掛把原本文章的 URL 轉到新的 URL

## 自定義域名

另外就是要把原本的域名拿來給 GitHub Pages 用
把本來指到我自架機器 IP 的 DNS A 紀錄，改成用 CNAME 指到 GitHub 這邊提供的域名（XXXX.github.io）
再去 Pages 的頁面設定一下就差不多了

我是還有再額外設定一下
讓網路流量跟之前一樣透過 CloudFlare 來反向代理
所以 TLS 憑證會看到是 CloudFlare 那邊來的

## 結語

其實就像前一篇結語寫的
這個部落格主要還是架好玩的
反正之前自架 WordPress 的時候就有玩到一些自架的東西了
現在改到 GitHub Pages 上之後就可以不用再額外花時間管了
