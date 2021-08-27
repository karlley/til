# aws_iam_user_create

AWS IAM ユーザの作成、アクセスキーの取得

## 参照

[AWS Identity and Access Management](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/introduction.html)

[【Railsチュートリアル】S3に画像をアップロードする設定【13章課題】 \- Qiita](https://qiita.com/ryuchan00/items/8e414562b7122e7ec4fb)

[IAM でユーザーを作成してみた \| ハックノート](https://hacknote.jp/archives/44404/)

[プロが教えるAWSアカウント作成後に行うべき設定 ～セキュリティ編～ \| TOKAIコミュニケーションズ AWSソリューション](https://www.cloudsolution.tokai-com.co.jp/white-paper/2021/0623-241.html)

## IAM とは

* AWS Identity and Access Management の略
* AWS へのアクセスを安全に行うためのウェブサービス
* AWS へのアクセスは基本的にIAM ユーザを使用する
* AWS のルートユーザを使ってのアクセスは推奨されていない

## IAM ユーザ作成

1. [AWS Identity and Access Management](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/introduction.html) にルートユーザでログイン
2. サービス > IAM > IAM ダッシュボードにアクセス
3. ユーザー > `ユーザーを追加` をクリック
4. ユーザ名入力(`S3_departure`)、`プログラムによるアクセス`、`AWS マネジメントコンソールへのアクセス`を選択 **ここでAWS マネジメントコンソールへのログインの可否が決まる**
5. グループ作成 > グループ名入力(`S3_departure`)
6. ポリシーの検索バーで`s3` を検索 > `AmazonS3FullAccess` のチェックボックスを選択 >グループの作成
7. `タグの追加`ページはそのまま次へ
8. `ユーザーを追加`ページで設定を確認し`ユーザーの作成`をクリック
9. `csvのダウンロード`からファイルをダウンロード

ダウンロードしたファイルの中身

* User name
* Password
* Access key ID
* Secret access key
* Console login link

## IAM ユーザでログイン

1. IAM ユーザ作成時に発行されたCSV の中のログインリンクを開く
2. Console login link の12桁の数字が入力された状態でログインページが開く
3. User name, Password を入力する
4. 初回ログイン時のみパスワードを変更する

**ログインにAccess key ID,Secret access key は未使用**
