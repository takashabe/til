# SPAするためのHTTPルータの設定について

SPAアプリケーションではフロント側でルーティングを行う必要が出てくる。
フロント側で行うルーティングにはHash、History APIという2つの手法があるが、ここではより標準的なHistory APIを前提とする。

* やること
    * Webサーバを別途立てる
    * サーバ側で規定のリクエスト以外は全て `index.html(SPAのエントリポイント)` に向ける


#### サンプル

* [takashabe/go-router](https://github.com/takashabe/go-router) を使用
* nginx等でも考え方は同じ

```go
func Routes() *router.Router {
  r := router.NewRouter()

  // Routing of the backend API
  r.Get("/api/login", ...)
  r.Post("/api/login", ...)

  // Routing of the frontend
  r.ServeFile("/", "./public/index.html")
  r.ServeFile("/bundle.js", "./public/bundle.js")
  // Routing to SPA entry point when 404 request
  r.NotFoundHandler = http.HandlerFunc(func(w http.ResponseWriter, req *http.Request) {
    http.ServeFile(w, req, "./public/index.html")
  })

  return r
}
```
