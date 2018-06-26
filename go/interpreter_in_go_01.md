# Writing An Interpreter in Go 読書会#1

## Indexs

- 全体像
- 字句解析器
- REPL
- おまけ

## 全体像

### この本は何

- Monkeyという本書用の言語を定義し、それが動作するインタプリタを作ることがゴール
    - C言語っぽい構文
    - 整数、文字列、関数が扱える
    - 第一級関数を持つ

### インタプリタ is

- 与えられた文字列を解析し、意味のある結果を返す
- コンポーネント
    - 字句解析器(Lexer)
    - 構文解析器(Parser)
    - 評価器(Evalator)
- 字句解析器
    - ソースコード(文字列)を解析してトークンを生成する
    - 言語が扱えるキーワードが決定する
- 構文解析器
    - トークン列を受け取り、抽象構文木(AST)を生成する
    - プログラミング言語の構文が決定する
- 評価器
    - いわゆる言語処理系
    - ASTを受け取り、処理を行う
    - プログラミング言語の挙動が決定する

### 字句解析器

#### 字句解析やること

- 入出力
    - in: 文字列(ソースコード)
    - out: トークン列
- 処理の流れ
    - 1文字ずつ文字列を走査
    - 事前定義しておいたトークンと走査した文字列を紐づけ
    - トークンを出力

#### コード

- テスト

```
func TestNextToken(t *testing.T) {
  input := `let five = 5;
let ten = 10;

let add = fn(x, y) {
  x + y;
};
...
`

  tests := []struct {
    expectedType    token.TokenType
    expectedLiteral string
  }{
    {token.LET, "let"},
    {token.IDENT, "five"},
    {token.ASSIGN, "="},
    ...
  }

  l := New(input)

  for i, tt := range tests {
    tok := l.NextToken()

    if tok.Type != tt.expectedType {
      t.Fatalf("tests[%d] - tokentype wrong. expected=%q, got=%q",
        i, tt.expectedType, tok.Type)
    }

    if tok.Literal != tt.expectedLiteral {
      t.Fatalf("tests[%d] - literal wrong. expected=%q, got=%q",
        i, tt.expectedLiteral, tok.Literal)
    }
  }
}
```

- トークンの定義

```go
// token/token.go
type TokenType string

type Token struct {
  Type    TokenType
  Literal string
}
```

- 入力文字列の走査

```go
// lexer/lexer.go
type Lexer struct {
  input        string
  position     int  // current position in input (points to current char)
  readPosition int  // current reading position in input (after current char)
  ch           byte // current char under examination
}

func New(input string) *Lexer {
  l := &Lexer{input: input}
  l.readChar()
  return l
}

func (l *Lexer) readChar() {
  if l.readPosition >= len(l.input) {
    l.ch = 0
  } else {
    l.ch = l.input[l.readPosition]
  }
  l.position = l.readPosition
  l.readPosition += 1
}
```

- 文字列からトークンへの変換

```go
// lexer/lexer.go
func (l *Lexer) NextToken() token.Token {
  var tok token.Token

  l.skipWhitespace()

  switch l.ch {
  case '+':
    tok = newToken(token.PLUS, l.ch)
  case '=':
    if l.peekChar() == '=' {
      ch := l.ch
      l.readChar()
      literal := string(ch) + string(l.ch)
      tok = token.Token{Type: token.EQ, Literal: literal}
    } else {
      tok = newToken(token.ASSIGN, l.ch)
    }
...
  default:
    if isLetter(l.ch) {
      tok.Literal = l.readIdentifier()
      tok.Type = token.LookupIdent(tok.Literal)
      return tok
    } else if isDigit(l.ch) {
      tok.Type = token.INT
      tok.Literal = l.readNumber()
      return tok
    } else {
      tok = newToken(token.ILLEGAL, l.ch)
    }
  }

  l.readChar()
  return tok
}

// 空白を飛ばす
func (l *Lexer) skipWhitespace() {
  for l.ch == ' ' || l.ch == '\t' || l.ch == '\n' || l.ch == '\r' {
    l.readChar()
  }
}

// ポインタを進めずに先の文字列を読み取る
func (l *Lexer) peekChar() byte {
  if l.readPosition >= len(l.input) {
    return 0
  } else {
    return l.input[l.readPosition]
  }
}

// 複数文字列を判定する
func (l *Lexer) readIdentifier() string {
  position := l.position
  for isLetter(l.ch) {
    l.readChar()
  }
  return l.input[position:l.position]
}
func isLetter(ch byte) bool {
  return 'a' <= ch && ch <= 'z' || 'A' <= ch && ch <= 'Z' || ch == '_'
}

// 複数数値を判定する
func (l *Lexer) readNumber() string {
  position := l.position
  for isDigit(l.ch) {
    l.readChar()
  }
  return l.input[position:l.position]
}
func isDigit(ch byte) bool {
  return '0' <= ch && ch <= '9'
}
```

### REPL

```go
const PROMPT = ">> "

func Start(in io.Reader, out io.Writer) {
  scanner := bufio.NewScanner(in)

  for {
    fmt.Printf(PROMPT)
    scanned := scanner.Scan()
    if !scanned {
      return
    }

    line := scanner.Text()
    l := lexer.New(line)

    for tok := l.NextToken(); tok.Type != token.EOF; tok = l.NextToken() {
      fmt.Printf("%+v\n", tok)
    }
  }
}
```

### おまけ

- goではどうなの?
    - 字句解析
        - `go/scanner`
        - `go/token`
    - 構文解析
        - `go/parser`
        - `go/ast`
        - (`cmd/gofmt`)
    - 評価
        - `cmd/compile`

- support 🍣
    - runeで実装すれば可能
    - (https://github.com/takashabe/monkey/commit/e980b30f973d594ae8cef14abb109d3e88881e07)[https://github.com/takashabe/monkey/commit/e980b30f973d594ae8cef14abb109d3e88881e07]
