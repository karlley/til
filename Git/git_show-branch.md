# git show-branch

`git show-branch` の使い方

## 参照

* ローカル/リモートのブランチの状況を確認できる
* ブランチの派生元を確認できる

[git show\-branch を使っていないだなんて！ \- Qiita](https://qiita.com/t_uda/items/9b6055aa93215cb8bac1)

[$ git show\-branchって意外と使えるかもしれない \- helen's blog](https://helen.hatenablog.com/entry/2016/05/25/115248)

[git show\-branch の見方 \- 言語ゲーム](https://propella.hatenablog.com/entry/20090220/p1)

## ブランチの派生元の修正

`main` ブランチから派生させるところを`fizzbuzz` ブランチから派生させてしまった場合の修正方法

[Gitでブランチの派生元を間違えたときの解決方法（rebase \-\-onto、cherry\-pick） \| 株式会社グランフェアズ](https://www.granfairs.com/blog/staff/git-mistake-parent-branch)

1. 派生元を間違った`calender` ブランチで`git stash -u` でコードを退避
2. `git fetch origin` でリモートから最新に更新
3. `git rebase origin/main` で派生元を`main` に修正
4. `git stash apply` で退避させたコードを復元

修正前

```
~/Workspace/ruby-practices calender
❯ git show-branch --all
* [calender] ファイル末尾に改行を追加
 ! [fizzbuzz] イテレータを使った記述に修正
  ! [main] Merge pull request #157 from fjordllc/improve-readme
   ! [origin/HEAD] Merge pull request #157 from fjordllc/improve-readme
    ! [origin/fizzbuzz] ファイル末尾に改行を追加
     ! [origin/main] Merge pull request #157 from fjordllc/improve-readme
------
 +     [fizzbuzz] イテレータを使った記述に修正
*+  +  [calender] ファイル末尾に改行を追加
*+  +  [calender^] Add fizzbuzz
------ [main] Merge pull request #157 from fjordllc/improve-readme
```

```
~/Workspace/ruby-practices calender
❯ git show-branch --all
* [calender] Merge pull request #157 from fjordllc/improve-readme
 ! [fizzbuzz] イテレータを使った記述に修正
  ! [main] Merge pull request #157 from fjordllc/improve-readme
   ! [origin/HEAD] Merge pull request #157 from fjordllc/improve-readme
    ! [origin/fizzbuzz] ファイル末尾に改行を追加
     ! [origin/main] Merge pull request #157 from fjordllc/improve-readme
------
 +     [fizzbuzz] イテレータを使った記述に修正
 +  +  [origin/fizzbuzz] ファイル末尾に改行を追加
 +  +  [origin/fizzbuzz^] Add fizzbuzz
------ [calender] Merge pull request #157 from fjordllc/improve-readme
```
