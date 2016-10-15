## Goにおけるクロージャ

匿名関数を返す関数を使うことでクロージャが実現出来る。クロージャは生成した関数ごとに異なる局所変数を参照可能となる。


例えば文字列を指定した回数繰り返すような関数

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
