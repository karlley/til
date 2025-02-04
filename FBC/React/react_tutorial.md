# Reactチュートリアル

## トップページ

https://ja.react.dev/

- React コンポーネントは単なるJavaScriptの関数
- マークアップ構文はJSX
- Reactコンポーネントはデータを受け取り、画面に表示するものを返す
- Reactはウェブアプリとネイティブアプリの両方を開発可能
- データが取得中でもHTMLのストリーミングを開始でき、JavaScript コードが読み込まれる前にコンテンツを段階的にロードすることができる

## クイックスタート

https://ja.react.dev/learn

- コンポーネント: 独自のロジックと外見を持つUI部品
- Reactにおけるコンポーネントは、マークアップを返すJavaScript関数
- コンポーネント名は常に大文字で始める必要ある
  - HTMLタグは小文字
- コンポーネントは複数のJSXタグをreturnすることはできない
  - 複数行をreturnする場合は`<div>` や`<>...</>` で囲む必要がある
- JSX内の`{}` の中はJavaScriptを記述できる
- `<li>` 内のkey属性にはid等を指定する必要がある
- `<button onClick={handleClick}>` の`handleClick` はメソッドを呼び出しているのでは無くメソッドを渡している
  - クリック時の処理を渡している
- `useState` で2つが定義される
  - 現在の状態(state)
  - 状態を更新する関数
- 同一コンポーネントでもstateは別々の値を保持する
- props: 親から子に渡される値

## チュートリアル

https://ja.react.dev/learn/tutorial-tic-tac-toe

- イベントを表すpropsには`onSomething` という名前を使用する
- イベントを処理するハンドラ関数の定義には`handleSomething` という名前を使用する
- `export default` キーワードを付けたコンポーネントはトップレベルのコンポーネントとして扱われる

## Reactの流儀

https://scrapbox.io/karlley/React%E3%81%AE%E6%B5%81%E5%84%80

