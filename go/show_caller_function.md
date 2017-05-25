# ある関数を呼び出した関数を表示する

* `runtime.Celler()` の引数はスタック数

```
package main

import (
  "log"
  "runtime"
)

func caller() {
  pc, _, _, _ := runtime.Caller(1)
  fn := runtime.FuncForPC(pc)
  log.Printf("Function Name: %s\n", fn.Name())
}

func main() {
  caller()
}
```

https://play.golang.org/p/rpn2Ixfjxk
