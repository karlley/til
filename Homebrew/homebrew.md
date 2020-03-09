# homebrew 関連メモ

## $PATH

$PATH を見やすく表示

```
$ echo $PATH | tr ':' '\n'
```

## brew cask

https://github.com/Homebrew/homebrew-cask

mac デスクトップアプリをhomebrew でインストールできるようになる

```
$ brew cask
==> Tapping homebrew/cask
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask'...
.
See also "man brew-cask"
$ brew cask doctor
==> Homebrew Version
2.2.6
.
PATH="/usr/local/Homebrew/Library/Homebrew/shims/scm:/usr/bin:/bin:/usr/sbin:/sbin"
SHELL="/bin/zsh
```