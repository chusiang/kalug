---
title: "Go 語言的 init 函式"
date: 2018-04-19T03:17
type: blog
author: AppleBoy
link: https://blog.wu-boy.com/2018/04/init-func-in-golang/
layout: post
comments: true
---

<a href="https://www.flickr.com/photos/appleboy/24407557644/in/dateposted-public/" title="Go-brown-side.sh"><img alt="Go-brown-side.sh" src="https://i1.wp.com/farm2.staticflickr.com/1622/24407557644_36087ca6de.jpg?w=840&#038;ssl=1" /></a>

本篇會帶大家認識 <a href="https://golang.org">Go 語言</a>的 init 函式，在了解 init func 之前，大家應該都知道在同一個 Package 底下是不可以有重複的變數或者是函式名稱，但是唯獨 init() 可以在同一個 package 內宣告多次都沒問題。底下看<a href="https://play.golang.org/p/AN-6MK4qVVL">例子</a>，可以發現的是不管宣告多少次，都會依序從最初宣告到最後宣告依序執行下來。

<pre class="brush: go; title: ; notranslate">
package main

import (
    "fmt"
)

func init() {
    fmt.Println("init 1")
}

func init() {
    fmt.Println("init 2")
}

func main() {
    fmt.Println("Hello, playground")
}
</pre>

<span id="more-7013"></span>

<h2>從其他 package 讀取 init func</h2>

有種狀況底下，主程式需要單獨讀取 package 內的 init func 而不讀取額外的變數，這時候就要透過 <code>_</code> 來讀取 package。假設要讀取 <a href="https://github.com/lib/pq">lib/pq</a> 內的 <a href="https://github.com/lib/pq/blob/master/conn.go#L48-L50">init</a>，一定要使用 <code>_</code>

<pre class="brush: go; title: ; notranslate">
import(
    // Needed for the Postgresql driver
    _ "github.com/lib/pq
)
</pre>

如果沒有加上 <code>_</code>，當編譯的時候就會報錯，原因就是 main 主程式內沒有用到 pq 內任何非 init() 的功能，所以不可編譯成功。如果有多個 package 的 init 需要同時引入，這邊也是會依照 import 的順序來讀取。

<h2>init 執行時機</h2>

大家一定很好奇 init 的執行時間是什麼時候，底下舉個<a href="https://github.com/go-training/training/blob/990af0ec6605e1e5f9ce239cc9380d79d80ddbce/example16-init-func/main.go#L10-L22">例子</a>

<pre class="brush: go; title: ; notranslate">
var global = convert()

func convert() int {
    return 100
}

func init() {
    global = 0
}

func main() {
    fmt.Println("global is", global)
}
</pre>

或者是把 init() 放到最上面

<pre class="brush: go; title: ; notranslate">
func init() {
    global = 0
}

var global = convert()

func convert() int {
    return 100
}

func main() {
    fmt.Println("global is", global)
}
</pre>

兩種結果都是 0，這邊大家就可以知，init 執行的時機會是在執行 main func 之前，所以不管前面做了哪些事情，都不會影響 init 的執行結果。最後提醒大家，只要 package 內有 init 的 func，在引入 package 時都會被執行。

<h2>線上影片</h2>



<hr />

Go 語言線上課程目前特價 $1600，持續錄製中，每週都會有新的影片上架，歡迎大家參考看看，請點選底下購買連結:

<h1><a href="http://bit.ly/intro-golang">點我購買</a></h1>
<div class="wp_rp_wrap  wp_rp_plain"><div class="wp_rp_content"><h3 class="related_post_title">Related View</h3><ul class="related_post wp_rp"><li><a class="wp_rp_title" href="https://blog.wu-boy.com/2017/03/error-handler-in-golang/">Go 語言的錯誤訊息處理</a><small class="wp_rp_comments_count"> (0)</small><br /></li><li><a class="wp_rp_title" href="https://blog.wu-boy.com/2016/05/package-management-for-golang-glide/">Golang 套件管理工具 Glide</a><small class="wp_rp_comments_count"> (2)</small><br /></li><li><a class="wp_rp_title" href="https://blog.wu-boy.com/2018/01/write-golang-in-aws-lambda/">在 AWS Lambda 上寫 Go 語言搭配 API Gateway</a><small class="wp_rp_comments_count"> (3)</small><br /></li><li><a class="wp_rp_title" href="https://blog.wu-boy.com/2017/05/go-struct-method-pointer-or-value/">Go 語言內 struct methods 該使用 pointer 或 value 傳值?</a><small class="wp_rp_comments_count"> (1)</small><br /></li><li><a class="wp_rp_title" href="https://blog.wu-boy.com/2017/02/write-command-line-in-golang/">用 Golang 寫 Command line 工具</a><small class="wp_rp_comments_count"> (1)</small><br /></li><li><a class="wp_rp_title" href="https://blog.wu-boy.com/2017/07/go-framework-gin-release-v1-2/">Go 語言框架 Gin 終於發佈 v1.2 版本</a><small class="wp_rp_comments_count"> (2)</small><br /></li><li><a class="wp_rp_title" href="https://blog.wu-boy.com/2017/03/golang-dependency-management-tool-dep/">Go 語言官方推出的 dep 使用心得</a><small class="wp_rp_comments_count"> (6)</small><br /></li><li><a class="wp_rp_title" href="https://blog.wu-boy.com/2016/07/new-coverage-service-codecov-io/">新的 code coverage 線上服務 codecov.io</a><small class="wp_rp_comments_count"> (0)</small><br /></li><li><a class="wp_rp_title" href="https://blog.wu-boy.com/2017/04/build-minimal-docker-container-using-multi-stage-for-go-app/">用 Docker Multi-Stage 編譯出 Go 語言最小 Image</a><small class="wp_rp_comments_count"> (1)</small><br /></li><li><a class="wp_rp_title" href="https://blog.wu-boy.com/2016/11/golang-gofight-support-echo-framework/">輕量級 Gofight 支援 Echo 框架測試</a><small class="wp_rp_comments_count"> (0)</small><br /></li></ul></div></div>