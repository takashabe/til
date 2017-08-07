# vimでgoのメソッドにジャンプする

## モチベーション

interfaceで定義したメソッドがどこで実装されているか調べる場合、メソッド名でgrepすると呼び出し元もマッチするのでノイズが多いので、もっと楽に特定箇所に飛びたい。

## deniteからgrepする

以下のdeniteコマンドをベースにして適当にマッピングを書く。ただ単にカーソル位置の単語grepに文字列を加えているだけなので、interfaceの定義場所から飛ぶようにする。
deniteでは元々 `DeniteCursorWord` というカーソル以下の単語をターゲットに入れた状態でdeniteを起動するコマンドが用意されているが、変更するのが難しそうに見えたので `expand('<cword>')` を直接呼んでいる。`DeniteCursorWord` の実装でも同じようにターゲットを組み立てているので問題ないはず。

<https://github.com/Shougo/denite.nvim/blob/e7f0e56d7893689d45ea9ac661d89f5a8b7641e7/autoload/denite/helper.vim#L44>

```vim
:Denite grep -input=func\ \(.*\)\ `expand('<cword>')`\(
```

実際のマッピング全体は次のような感じになると思う。

```vim
nnoremap <silent> [denite]i  :<C-u>Denite grep -input=func\ \(.*\)\ `expand('<cword>')`\( -auto-preview -buffer-name=search-buffer-denite<CR>
```

なお普段deniteのgrepにはagを使っている。`-input` の書き方は他のgrepオプションで動かない場合は適宜調整してもらいたい。

```vim
call denite#custom#var('grep',  'command',  ['ag'])
call denite#custom#var('grep',  'default_opts',
    \ ['-i',  '--vimgrep',  '-G'])
call denite#custom#var('grep',  'recursive_opts',  [])
call denite#custom#var('grep',  'pattern_opt',  [''])
call denite#custom#var('grep',  'separator',  ['--'])
call denite#custom#var('grep',  'final_opts',  [])
```
