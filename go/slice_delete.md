# sliceから任意のキーを削除する

`append`を使って削除したい任意のキー以外を結合する

```
package main

import (
  "errors"
  "fmt"
)

func main() {
  a := []int{1, 2, 3, 4, 5, 6}
  fmt.Println(delete(a, 3))
  fmt.Println(delete(a, 5))
  fmt.Println(delete(a, -1))
}

func delete(s []int, d int) ([]int, error) {
  // make copy destination. need source slice size
  res := make([]int, len(s))
  copy(res, s)

  // delete value
  for k, v := range res {
    if v == d {
      res = append(res[:k], res[k+1:]...)
      return res, nil
    }
  }
  return nil, errors.New("not found delete value")
}
```

https://play.golang.org/p/bE4brB3wDF
