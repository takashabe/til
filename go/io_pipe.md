# 非同期に書き込まれている io.Writer の内容をデータ競合させずに読みたい

## tl;dr

`io.Pipe` を使う

## 背景

goroutine で無限ループさせながら特定の `io.Writer` に書き込むようなコードがあった時、テストで正しく書き込まれているか知りたいことがある。
しかしよくテストで使われる `bytes.Buffer` を利用すると、内部のバッファを取り合う形になってしまうのでデータ競合が発生する。

例えば以下のようなコード

* 被テストコード

```go
type A struct {
  w io.Writer
}

func (a *A) loop() {
  for {
    message := []byte("foo")
    fmt.Fprintf(a.w, "%s", message)
    time.Sleep(10 * time.Millisecond)
  }
}
```

* テストコード

```go
func TestRace(t *testing.T) {
  var w bytes.Buffer
  a := &A{
    w: &w,
  }

  go a.loop()
  time.Sleep(50 * time.Millisecond)

  // data race!!!
  bytes := make([]byte, 1024)
  w.Read(bytes)
}
```

## 対応

`bytes.Buffer` の代わりに `io.Pipe` を使う。

* テストコード

```go
func TestNonRace(t *testing.T) {
  pr, pw := io.Pipe()
  a := &A{
    w: pw,
  }

  go a.loop()
  time.Sleep(50 * time.Millisecond)

  // non data race
  bytes := make([]byte, 1024)
  pr.Read(bytes)
}
```
