# vim diff

vim を使用したdiff の使い方

## 参考URL

[[Vim]vimdiffで差分を表示・マージして元ファイルとの差分を確認する](https://wada811.blogspot.com/2013/07/vimdiff-merge-and-difforig.html)
[Vimdiff が良い感じ。](https://loumo.jp/archives/4000)
[意外と知られていない diff に関する機能](https://thinca.hatenablog.com/entry/20130426/1366910837)
[わかりやすい差分(diff)の取り方いろいろメモ](https://qiita.com/bitnz/items/725350b614bafedc581a)
[git logをfzfに流してcommitのshaをさっと選びたい](https://zenn.dev/miyanokomiya/articles/5931a3af9a710d)

[一時ファイル、作業用ファイルを使い倒すための実戦向けテクニック](https://nanasi.jp/articles/howto/file/workingfile.html)
[vimですでに開いているファイルと別ファイルとの差分を表示する方法](https://qiita.com/isseium/items/36b54171c430f381e232)
[vimdiff の使い方 (差分確認 )](https://tyablog.net/2018/03/04/lets-vimdiff/)
[Vimのdiffモード関連Tips](https://rcmdnk.com/blog/2016/02/26/computer-vim/)
[Vimを体系的に学ぶつもりのない人のためのVim講座―Exコマンド編](https://qiita.com/okuramasafumi/items/5e4624cfc8379aec739c)
[vimでキーマッピングする際に考えたほうがいいこと](http://deris.hatenablog.jp/entry/2013/05/02/192415)

## help

```Vim
# help
:help vimdiff
```

## command

file_A, file_B を比較

```Shell
$	vimdiff file1 file2
$ vim -d file1 file2
```

file_A を編集中にfile_B と比較

```Vim
# 左右分割してdiff
:diffsplit file_B
:diffs file_B
```

file_A を編集中にクリップボードにコピーした内容と比較

```Vim
# 空バッファ作成
:enew #無名バッファ作成
:split enew # 垂直分割 + 空バッファ作成
:vsplit enew # 左右分割 + 空バッファ作成
:tabnew # 新規タブ + 空バッファ作成

# file_A, 開いたバッファで下記コマンド
:diffthis
:difft
```

## other command

vimdiff 終了

```Vim
:diffo
:diffo!
```

vimdiff リロード

```Vim
:diffupdate
:dif
```

差異のカーソル移動

```Vim
[c
c]
```

差異の反映

```Vim
# get, put(current_file < oher_file)
# ヴィジュアルで選択後
:diffget
:diffg

# 行指定
:line_number diffget
:line_number diffg

# 短縮形
# 差異行で下記コマンド
:do

# push, obtain(current > other_file)
# ヴィジュアルで選択後
:diffput
:diffpu

# 行指定
:line_number diffput
:line_number diffpu

# 短縮形
# 差異行で下記コマンド
:dp
```

## option

常に左右分割でdiff

```Vim
set diffopt=vertical
```

:保存前に:DiffOrig で編集前と比較

```Vim
set splitright
if !exists(":DiffBefore")
  command DiffBefore vert new | set bt=nofile | r # | 0d_ | diffthis
        \ | wincmd p | diffthis
endif
```

```Vim
:DiffPast をショートコマンド化
nnoremap <Leader>db :DiffBefore<CR>
```

## .vimrc

```Vim
nnoremap <Leader>ds :diffsplit<CR>
nnoremap <Leader>dt :diffthis<CR>
nnoremap <Leader>do :diffoff<CR>
nnoremap <Leader>du :diffupdate<CR>
nnoremap <Leader>dg :diffget<CR>
nnoremap <Leader>dp :diffput<CR>
nnoremap <Leader>db :DiffBefore<CR>
```
