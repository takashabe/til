## reflectパッケージを使って任意の関数を呼び出す

HTTPルータの実装でreflectを使って動的にhandlerを呼び出そうとしたメモ

### 任意の関数をinterface{}に紐付ける

関数は引数と戻り値を包括して一つの型となる。
しかし関数は第一級オブジェクトなのでinterface{}に代入出来るため、
それを利用して異なるシグネチャの関数を統一的に扱える。

```
type General interface{}

func Fn(g General) {}

func main() {
  Fn(func() {})
  Fn(func(x, y int) int { return x*y })
}
```

### interface{}に紐付けた関数を実行する

reflectパッケージを利用する。
reflect.ValueOf()で一度Value型を取得し、Value.Call()で関数を実行できる。

ref: https://play.golang.org/p/xLyb1503DL
