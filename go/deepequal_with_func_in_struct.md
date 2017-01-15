## reflect.DeepEqualで関数が埋め込んである構造体を比較する

### TL;DR

関数同士の厳密な比較は出来ないので、テスト時に関数をnilに差し替えるなどの工夫が必要

### reflect.DeepEqualの比較について

[reflect.DeepEqual](https://golang.org/pkg/reflect/#DeepEqual)はstruct, map, arrayなどをいい感じに比較してくれる便利なものです。

単純な関数同士を比較することはあまりないと思いますが、structに関数を埋め込んでいる場合などは注意が必要です。

godocに以下の記述がある通り、関数は対象がどちらもnilの場合のみ等しいと見なされます。

よって比較する場合は一時的にnilと差し替えるなどの対応が必要となります。

```
Func values are deeply equal if both are nil; otherwise they are not deeply equal.
```

[実装](https://golang.org/src/reflect/deepequal.go?#L120)は以下のようになっています。

```
case Func:
    if v1.IsNil() && v2.IsNil() {
        return true
    }
    // Can't do better than this:
    return false
```

#### 実行例

```
package main

import (
    "fmt"
    "reflect"
)

type A struct {
    f func() int
}

func main() {
    a1 := A{f: func() int { return 0 }}
    a2 := A{f: func() int { return 0 }}
    fmt.Println(reflect.DeepEqual(a1, a2)) //=> false

    a3 := A{f: nil}
    a4 := A{}
    fmt.Println(reflect.DeepEqual(a3, a4)) //=> true
}
```

https://play.golang.org/p/yxlPGnUZ_Y
