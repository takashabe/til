## スライスを展開して ...interface{} 型の引数に渡す

#### スライスの展開

スライスargsがあるとき`args...`で展開することが出来る。

```
func A(args ...string) {}

func main() {
  args := []string{"a", "b"}
  A(args...)
}
```

しかし以下のように引数が`...interface{}`となっている場合はコンパイルに失敗してしまう。

```
func A(args ...interface{}) {}

func main() {
  args := []string{"a", "b"}
  A(args...)
}

// => cannot use args (type []string) as type []interface {} in argument to A
```

#### ...interface{}への展開

[FAQ](https://golang.org/doc/faq#convert_slice_of_interface)によると`[]T`から`[]interface{}`にはキャスト出来ないので、明示的にスライスを用意すれば良いと書いてある。

```
func A(args ...interface{}) {}

func main() {
  args := []string{"A", "B"}
  is := make([]interface{}, 0, len(args))
  for _, a := range args {
    is = append(is, a)
  }
  A(is...)
}
```

https://play.golang.org/p/roDM927Vik
