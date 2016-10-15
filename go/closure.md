## Goにおけるクロージャ

匿名関数を返す関数を使うことでクロージャが実現出来る。クロージャは生成した関数ごとに異なる局所変数を参照可能となる。

#### クロージャの例

文字列を指定した回数繰り返すような関数は以下の通り。

```
package main

import (
  "fmt"
  "strings"
)

func echo(s string) func(int) string {
  return func(n int) string { return strings.Repeat(s, n) }
}

func main() {
  foo := echo("foo")
  bar := echo("bar")

  fmt.Println(foo(3)) // => foofoofoo
  fmt.Println(bar(5)) // => barbarbarbarbar
}
```

#### ジェネレータの例

クロージャを用いてジェネレータという、関数が呼び出される度に新しい値を生成するような関数を作ることが出来る。

```
package main

import "fmt"

func adder() func(int) int {
  sum := 0
  return func(i int) int {
    sum += i
    return sum
  }
}

func main() {
  a := adder()
  b := adder()

  for i := 0; i < 10; i++ {
    fmt.Println(a(i))   // => 0, 1, 3, 6, ...
    fmt.Println(b(i+1)) // => 1, 3, 6, 10, ...
  }
}
```

ここで`adder`のsum初期化は一度実行された後、変数a, bそれぞれに返した関数を適用する形となる。こうすることによって例えばグローバル変数に合計値を保持するような手法に比べて簡潔に書ける。
