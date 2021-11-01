# js_basic

JavaScript の基礎

## ポイント

* `//`でコメント
* `user strict`; で厳密なエラーチェックを行う
* 処理の対象にid を追加 > 対象に対する処理を追加
* 処理の対象にid を追加 > 対象の対象にclass、id を付与する
* 改行: `\n`, タブ: `\t`
* 定数: `const`、変数: `let`
* 定数、変数には`-` は使えない
* 変数への再代入には`=`を付与する
* `++;`でインクリメント
* `--;`でデクリメント
* `typeof` で型を出力
* 文字列でも数値計算(`*-/`)を自動で行う(`'5' * 3` 結果15)
* `+`は数値計算ではなく文字列連結を行う(`'5' + 3` 結果53)
* `parseInt` で数値化
```
parseint(対象の値, 進数);
console.log(parseInt('5', 10));
```
* 等しいは`===`、等しくない`!==`
* `Boolean` で真偽値表示(`console.log(Boolean('hello'));`)
* 条件分岐のブロックの最後には`;`は不要
* ループ処理
```
for (let i = 1; i <= 10; i++) {
  console.log('hello');
}
```
* テンプレートリテラルは```(バッククオート)`で囲む
```
console.log(`hello ${i}`);
```
* while(先行評価)
```
let hp = 100;
while (hp > 0) {
  console.log(`${hp} HP left!`);
  hp -= 15;
}
```
* do while(遅延評価)
```
let hp = -50;

do {
  console.log(`${hp} HP left!`);
  hp -= 15;
} while (hp > 0);
```
* `continue` でループ処理をスキップ
* `break` でループ処理をストップ
* 関数呼び出しでは`();`が必要
* 仮引数: 関数側で引数を受け取る側
* 実引数: 引数をセットする側
```
// message が仮引数
function showAd(message)

// show message! が実引数
showAd('show message!');
```
* `return`を使用することで関数から値を返す
* `return` 後の処理は実行されない
* 関数式: 関数を定数などに代入する記述
```
// 関数式
const 定数名 = function(仮引数) {
処理;
return 返り値;
}

// 関数呼び出し
定数名(実引数);
```
* 無名関数: 関数式内での`function`以降の名前の無い関数
```
// function(a, b)が無名関数
const sum = function (a, b) {
  return a + b;
}

const total = sum(1, 2) + sum(2, 2);
console.log(total);
```
* アロー関数での書き換え
```
//関数式
const sum = function (a, b) {
return a + b;
}

//アロー関数
const sum = (a, b) => a + b;
```
* グローバルスコープ: 全ての範囲で使用できる値
* `{}`で変数名を囲む事でスコープを分けることができる
```
{
const x = 100;
}
```
* `for`を使用したループ処理
```
  const scores = [80, 90, 40];

  for (let i = 0; i < scores.length; i++) {
    console.log(`Score: ${scores[i]}`);
  }
```
* 配列の先頭に値を追加: `unshift()`
* 配列の末尾に値を追加: `push()`
* 配列の先頭に値を削除: `shift()`
* 配列の末尾に値を削除: `pop()`
* 配列の値の削除(`shift, pop`)は1つづくしか削除できない
* `splice` で配列の任意のインデックスの値を削除、追加ができる
