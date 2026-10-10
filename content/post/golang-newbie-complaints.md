---
title: "Golang 新手的抱怨"
date: 2024-04-16T00:00:00+08:00
url: "/2024/04/16/golang-newbie-complaints.html"
tags: ["Programming", "Golang"]
---

睽違了兩年多的部落格更新
是要來抱怨這幾個月學 + 用 Golang 的使用體驗

<!--more-->

## 個人使用程式語言的經驗

我個人算上學生時代除了作業以外，開始會自己課餘寫程式開始
大概有八年的程式開發經驗

其中最熟 Python，不管業餘還是工作上都有用到，寫了六年多
不過沒有碰過 ML、AI、資料分析之類的
主要是在寫後端服務和一些有大有小的工具程式

Java 是在工作上寫過，接觸約兩年
JavaScript 工作上和業餘斷斷續續寫過
工作上也有多少碰到 C、C++、Groovy、Lua 之類的，大概就到略熟悉的程度
還有碰過像是架這個網站用的 Ruby，之前寫過的 PHP，不過都只有入門等級而已

## 開始寫 Golang

最近在公司裡被轉去一個新領域，主要也是開發後端服務
大部分的團隊都是用 Golang 開發，多數專案也是 Golang 寫的
雖然真的硬要用 Python 開發可能也可以
不過一方面懶得去爭論，另一方面想說學一下這個也算熱門的語言也不錯

## 寫這篇文章的原因

邊學邊寫 Golang 大概三個月了
目前都覺得越學越覺得不太喜歡這語言，覺得不吐不快

以下列出一些我覺得的優缺點，主要還是跟我最熟悉的 Python 比較

## 覺得 Golang 的優點

先提一些優點

- 效能
  雖然可能略輸 C/C++
  不過在大部分的情況下肯定比 Python 和 JavaScript 快

- 單一二進制檔案
  不用像 Python 和 JavaScript 之類的語言，部署的時候要想辦法準備環境
  Golang 是編譯成二進制檔案，直接執行就好

- `go fmt`
  官方直接有格式化工具，不用像其它大部分的語言要另外裝第三方工具
  開發者沒有藉口不跑格式化

## 覺得 Golang 的缺點

接下來就是各種抱怨了

### 缺乏文件

不確定具體原因，但我覺得各種 Golang 套件的文件都不是很完整
常常都要進去看原始碼才知道怎麼用

#### 例子：Gin

像是 Golang 前幾大的 web 框架－ [Gin 的文件][1]
根本沒有多少教學或說明，只有一些帶有一兩句說明的範例
API reference 很多也都只有簡短說明，有時候都看不出具體的用法
看起來完全不像是一個有 75k+ stars 的專案
這個也有人開 [issue][2] 在抱怨

#### 例子：gomock

[gomock][3] 是用來產生 mock 物件的工具
Google 後來不維護了，Uber [接去維護][4]
同樣也是只有基礎的教學
像是比較進階的 `matcher` 用法要自己點進去看原始碼參考

#### 對比 Python 套件的文件

以熱門的 web 框架來說
不論是 [Flask][5]、[Django][6]，還是 [FastAPI][7] 的文件都遠遠比 Golang 的 Gin 完整
不用一直去翻原始碼，看看文件就知道怎麼用

### 錯誤處理

Golang 沒有 try-catch
通常都是讓函式多回傳一個 `error` 物件
再從呼叫方 `if err != nil` 來判斷是否有錯誤發生

我個人是覺得這種作法寫起來沒有 try-catch 直覺
而且相較於 Golang 連要分辨錯誤的類型都有點麻煩
Python 那樣可以做 [exception chaining][8]、[exception grouping][9]
還可以根據 exception 的 parent class 來捕捉不同類型的錯誤
整個體驗差滿多的

### 套件管理

Golang 在 1.11 之後加入 module 的概念
套件管理用起來就會比較像 Python 的 Pipenv 或是 JavaScript 的 npm
把套件的版本號寫在 `go.mod` 裡面
工具自動產生套件的 hash 值和版本號到 `go.sum`

不過相較於 [Pipenv][10] 和 [npm][11] 有 dev dependencies 的概念
Golang 這邊沒有將 dependencies 分類
雖然 Golang 不像 Python 和 JavaScript 需要在執行環境安裝套件
但是仍然可能會有一些「tool dependencies」是只用在開發或 CI/CD 的時候

[官方的解法][12]是把這些套件寫進 `tools.go`
這樣 build 的時候就不會把這些套件包進去
...這什麼繞路的解法，加一個 tool dependency 還要記得改兩個地方

### 缺少一些語言特性

這個有可能只是因為我還不習慣寫 Golang
Golang 作為一個相對新的熱門程式語言
（2009年發行，Python 3.0 2008年發行，其它熱門語言就更舊了）
很多寫起來還是很有「老」程式語言的感覺

直接到官方的 GitHub issues 頁面[以按讚排序][13]
以現在的時間點（2024 年 04 月 16 日）來看就有幾個我遇過的障礙

- 第二名：[proposal: spec: add typed enum support][14]
  沒有 enum 可以用，只能用 `const` 來模擬

  只寫 Golang 就算了，開發後端服務免不了要跟前端或其它服務溝通
  不論是 HTTP API 常用的 OpenAPI
  或是 gRPC API 常用的 Protocol Buffers 都有 enum 這個類別
  在 Golang 要跟這些資料格式轉換的時候就會有點麻煩

- 第五名：[proposal: expression to create pointer to simple types][15]
  沒有辦法用簡單的語法直接建立一個指向新變數的指標

  會常撞到這個問題的其中一個情境跟 Golang 的一個語言特性有關
  就是 struct 建立起來沒有賦值的話就會是「[zero value][16]」
  int 會是 0、string 會是空字串、bool 會是 false，而不會是 nil 之類的
  也就是說沒有辦法分辨真的是 0/空字串/false 還是沒有賦值
  常用解法是把這些欄位的類別改成指標，然後判斷是否是 nil 來分辨是否有賦值

  不過要塞欄位時就麻煩了
  就算只是要塞一個固定的值，還是要先建立一個變數，才能再把變數的指標塞進去

  ```golang
  type MyStruct struct {
      fieldA *int
      fieldB *int
  }
  foo := 20
  bar := 30
  myStruct := MyStruct{
      fieldA: &foo,
      fieldB: &bar,
  }
  ```

  不然就是要另寫一個函式來取出指標

  ```golang
  type MyStruct struct {
      fieldA *int
      fieldB *int
  }
  func IntPointer(i int) *int {
      return &i
  }
  myStruct := MyStruct{
      fieldA: IntPointer(20),
      fieldB: IntPointer(30),
  }
  ```

  像 AWS 的 Golang SDK 就寫了[好幾個函式][17]在做這件事

## 結語

其實還有不少小問題
不過就先列這些比較大的障礙
不知道再多接觸一段時間會是開始接受
還是會有更多抱怨可以寫...

[1]: https://gin-gonic.com/docs/
[2]: https://github.com/gin-gonic/gin/issues/3152
[3]: https://github.com/golang/mock
[4]: https://github.com/uber-go/mock
[5]: https://flask.palletsprojects.com
[6]: https://docs.djangoproject.com
[7]: https://fastapi.tiangolo.com/
[8]: https://docs.python.org/3/tutorial/errors.html#exception-chaining
[9]: https://docs.python.org/3/tutorial/errors.html#raising-and-handling-multiple-unrelated-exceptions
[10]: https://pipenv.pypa.io/en/stable/pipfile.html#package-category-groups
[11]: https://docs.npmjs.com/specifying-dependencies-and-devdependencies-in-a-package-json-file
[12]: https://go.dev/wiki/Modules#how-can-i-track-tool-dependencies-for-a-module
[13]: https://github.com/golang/go/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc
[14]: https://github.com/golang/go/issues/19814
[15]: https://github.com/golang/go/issues/45624
[16]: https://golang.org/ref/spec#The_zero_value
[17]: https://github.com/aws/aws-sdk-go/blob/49079a/aws/convert_types.go#L5
