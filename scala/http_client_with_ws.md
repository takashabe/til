## Play WS APIを使ってHTTPリクエストを投げる

Playにバンドルされている非同期HTTP Clientライブラリ

#### 依存関係の解決

* build.sbtに以下を追加

```
libraryDependencies ++= Seq(
  ws
)
```

* コード内で使用する場合は以下のインポートを行う
  * リクエストを投げるだけの場合

```
import play.api.Play.current
import play.api.libs.ws._
import play.api.libs.ws.ning.NingAsyncHttpClientConfigBuilder
import scala.concurrent.Future
```

#### 外部にリクエストを投げる

* リクエストの構築

```
// URLの指定
val holder: WSRequestHolder = WS.url(url)

// HTTPオプションの指定
val complexHolder: WSRequestHolder = holder.withHeaders("Accept" -> "application/json")
  .withRequestTimeout(10000)
  .withQueryString("search" -> "play")

// GETメソッドの実行
val futureResponse: Future[WSResponse] = complexHolder.get()
```

その他詳しいオプションについては下記参照
https://www.playframework.com/documentation/ja/2.4.x/ScalaWS#Making-a-Request

* レスポンスの処理

Futureに対する処理を書く感じになる

```
## Futureを処理するためのcontext
implicit val context = play.api.libs.concurrent.Execution.Implicits.defaultContext

# JSONレスポンスで受け取る
val futureResult: Future[String] = WS.url(url).get().map {
  response => response.json.toString
}
futureResult.onComplete {
  case Success(body) => Logger.debug(body)
  case Failure(t)    => Logger.debug(body)
}
```
