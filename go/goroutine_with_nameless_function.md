# goroutineで無名関数を使う時に気をつけること

## TL;DR

goroutineで無名関数を使う場合、変わる可能性のある関数外の変数に依存してはいけない

## 問題

golangでは無名関数外のスコープの変数を呼ぶことが出来る。
しかしそれをgoroutine生成時に行うと外側の変数が変わってしまった時に期待する結果が得られない。

例えば以下のコードのように、interface T を実装した T1, T2 を順番に呼び出す。
期待する出力結果は t1, t2 がそれぞれ呼ばれることだ。

```
type T interface {
  foo()
}

type T1 struct{}

func (t *T1) foo() { fmt.Println("from t1") }

type T2 struct{}

func (t *T2) foo() { fmt.Println("from t2") }

func main() {
  ts := []T{
    &T1{},
    &T2{},
  }

  for _, t := range ts {
    go func() {
      t.foo()
    }()
  }
  time.Sleep(time.Second)
}
```

出力結果は次の通り

```
from t2
from t2
```

https://play.golang.org/p/UqqkCrwhbi

## 対応

これを回避するには明示的に引数を渡してあげれば良い。

```
  for _, t := range ts {
    go func(t T) {
      t.foo()
    }(t)
  }
```

https://play.golang.org/p/nwETlqmrjJ
