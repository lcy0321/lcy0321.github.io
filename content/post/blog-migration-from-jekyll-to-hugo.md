---
title: "又改本 Blog：從 Jekyll 轉移至 Hugo"
date: 2026-10-10T15:00:00+08:00
tags: ["Blog"]
---

[四年前](./blog-migration-to-github-pages.md)才把這個部落格從自架 WordPress 搬到 GitHub Pages 上
現在又要把原本生成靜態頁面用的 [Jekyll](https://jekyllrb.com/) 改成 [Hugo](https://gohugo.io/)

<!--more-->

## 動機

其實動機只是幾個不是很重要的要素：

### 想拔掉 Jekyll

如同當初的文章提的
當初選 Jekyll 就只是因為 GitHub 內建透過它生成頁面
可以不用自己寫 GitHub Actions workflow
不然我本來就不太想在本地安裝平常沒在用的 Ruby

但現在一方面我已經很熟 GitHub Actions
~~讓 LLM Agent~~ 寫起來沒什麼負擔
另一方面 GitHub 官方也是比較[建議自己寫 workflow 去發佈頁面](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll)
這樣就沒什麼理由繼續使用 Jekyll 了
不如使用熱度比較高、速度比較快、自訂比較方便的 [Hugo](https://gohugo.io/)

### AI Agent Readiness

2026 的現在
一個網站沒特別針對 AI agent 友善好像跟不上時代

Jekyll 要讓網站同時提供原始 Markdown 檔案、以及要另外生出 llms.txt 雖然不是做不到
但還是比較麻煩一點
就想說乾脆轉移到原生就支援多格式輸出的 Hugo（雖然仍需要自己加模板，但至少比 Jekyll 方便）
然後搭配能生成 llms.txt 的主題 [PaperMod](https://github.com/adityatelange/hugo-PaperMod)

### 消化 ChatGPT Plus 額度

平常會使用 ChatGPT Chat 做一些 survey 和問一些廢問題
為了能在 Chat 使用 Sol 等級的模型，有訂閱 ChatGPT Plus

ChatGPT Plus 內含一些 Codex 使用額度
但最近沒需要寫或改什麼專案
乾脆就挑這個幾百年沒動的部落格下手

結果用 GPT-6.1 Sol Max 改半天
一週額度還花不到四分之一
這個目的好像沒達成 XD

## 過程

一開始是拿 ChatGPT Chat 去 survey 適合替代 Jekyll 的方案
重點就是：好搬移、熱度高、好維護、頁面簡潔、適合做 AI Agent Readiness
來回幾次就決定選 Hugo

一開始照 ChatGPT 的建議選擇 [Stack](https://github.com/CaiJimmy/hugo-theme-stack) 這個主題
然後再丟給本地的 Codex 去改
改著改著發現怎麼要覆寫的設定和檔案越來越多

後來才又重新找到 [PaperMod](https://github.com/adityatelange/hugo-PaperMod)
同樣簡潔，要做到 AI Agent Readiness 更簡單
要覆寫的檔案也比較少

讓 Codex 初步把設定改一改之後
再跟它來回幾次把不需要的改動拿掉、加上一些我想要的額外設定
就差不多完工了

## 結果

### 改動幅度

追加：

- 一個新的 GitHub Actions workflow
- 三個覆寫 Hugo 或 PaperMod 內容的檔案

刪除：

- 四個覆寫 Jekyll 內容的檔案
- 兩個 Ruby 的依賴檔案

修改：

- 將 Jekyll 設定檔案改成 Hugo 的
- 將 14 個文章檔案的 metadata 改成 Hugo 版本，包含保留原有 URL 的設定

沒增加多少複雜度
甚至還可能簡化了一點

### 發佈速度

不知道是 Hugo 比 Jekyll 快，還是 GitHub Actions 現在很快
生成檔案 + 推送到 Pages 只要不到 1 分鐘
比我想像中快很多

### LLM 花費

初步的 survey 是在 Chat 上所以不消耗額外額度
後續包括：

- Hugo + Stack 版本的轉移
- Hugo + Stack 版本的來回修正
- 把 Stack 改成 PaperMod
- Hugo + PaperMod 版本的來回修正
- 後續發佈之後的小修正與確認

全程都使用 GPT-6.1 Sol 搭配 Max reasoning effort
這樣只消耗一週額度的 22%
看起來 GPT-6.1 Sol 真的是很省

用 ccusage 看：

- Input：2M
- Output：390K
- Cache Read: 41M
- Cost：$12.03

不太確定準不準

### AI Agent Readiness

現在部落格有 [llms.txt](https://blog.lcy.one/llms.txt)
每一篇文章的 HTML `<head>` 內，也都有 `<link rel="alternate" type="text/markdown">` 指到 Markdown 版本
[robots.txt](https://blog.lcy.one/robots.txt) 也有 [Content-Signal](https://contentsignals.org/) 了

其實也不太確定實際上是不是有讓 LLM Agent 比較好爬
反正現在標準也都是各講各的
就先從簡單明瞭的隨便做一做
