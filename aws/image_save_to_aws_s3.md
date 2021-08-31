# image_save_to_aws_s3

heroku 本番環境で画像アップロードができないのでAWS S3を使用した方法で表示させる

## 参照

[Amazon Simple Storage Service（Amazon S3）](https://docs.aws.amazon.com/ja_jp/AmazonS3/latest/userguide/Welcome.html)

[AWS S3 を使用して静的アセットとファイルアップロードを格納する \| Heroku Dev Center](https://devcenter.heroku.com/ja/articles/s3)

[【実践編⑧】ポートフォリオ作成のロードマップ〜検索、AWS S3、CSV出力、総仕上げ〜｜こうだい＠フルリモートエンジニア｜note](https://note.com/kodai_0122/n/nb22272cc15ea#mJvuK)

[fog](https://pikawaka.com/rails/carrierwave)

[【Rails】AWS「S3」に画像アップロードする \- Qiita](https://qiita.com/yamato1491038/items/2212b101378d0af21111)

[Heroku \- Herokuデプロイ時\.envの管理について｜teratail](https://teratail.com/questions/75408)

## AWS S3 とは

* Amazon Simple Storage Service の略
* データ容量関係無し
* バケットと呼ばれるデータ格納庫を容易してその中にファイルを保存する
* Accessキー、Secretキー を使用して接続する
* 作成後のバケット名とリージョンは変更不可

## ポイント

* gem `fog` は使用せずに`fog-aws` を使用する(インストールエラー防止)
* gem `fog-aws` は本番でも使用する為グローバルでインストール
* gem `dotenv` を使い開発環境の環境変数を設定
* gem `dotenv`の環境変数はアクセスキー、シークレットキー、リージョン、バケットの4つを設定
* 環境変数はheroku 側に直接設定してrails 側で読み込む
* 開発環境の環境変数で読込、本番環境(heroku)はコマンドで環境変数を設定する
* 本番環境で環境変数が設定されていないとコンパイルエラーが出る(`Precompiling assets failed.`)

## 全体の流れ

段階的にアップロードできるか確認しながら進める

1. 開発環境、本番環境のアップロード先を`:fog` に設定してクラウドストレージにアップするようにする
2. 開発環境からS3にアップできるようにする
3. 開発ブランチをmaster ブランチにマージ
4. master ブランチをheroku にデプロイ

**できれば開発ブランチを1度heroku にデプロイしてみて、動作確認が取れたらmaster ブランチにマージして再デプロイするのが望ましい**

[作業中のブランチをHerokuにデプロイする \- Qiita](https://qiita.com/Chinats/items/c00df6e3ca81184606fe)

[【初心者向け】git fetch、git merge、git pullの違いについて \- Qiita](https://qiita.com/wann/items/688bc17460a457104d7d)

## 手順

1. gem `fog-aws` インストール
2. 本番環境のプリコンパイル有効化
3. 画像アップローダ(`CarrierWave`)の設定をクラウドサービスを使うように設定変更
4. IAM ユーザ作成
5. バケット作成(バケット名: `departure-app`、リージョン: 東京、その他オプションは記事を参照)
6. バケットへのアクセスコントロールリスト(ACL) を確認
  * バケット所有者(AWSアカウント) に`読み取り`、`書き込み`
  * バケット所有者(AWSアカウント) にオブジェクトへの`リスト`、`書き込み`
7. 環境変数読込ファイル作成(`config/initializers/corrier_wave.rb`)
8. `.env` ファイルに開発環境用のAWS S3 の環境変数を追加
9. 開発環境で環境変数が設定されているか確認
10. rails サーバ再起動後に開発環境から画像のアップロードが機能しているか確認
11. 開発環境からアップロードした画像をAWS マネジメントコンソールで確認
12. 本番環境用の環境変数をheroku に設定
13. 手動でアセットコンパイルして動作確認
14. 作業ブランチをmaster ブランチにマージ
15. heroku にpush して動作確認

### 1. gem `fog-aws` インストール

本番環境でも使用するのでグローバルにインストール

```Ruby
# Gemfile
gem 'fog-aws'
```

### 2. 本番環境のプリコンパイル有効化

```Ruby
# config/environment/production.rb
.
  config.assets.compile = true
```

### 3. 画像アップローダ(`CarrierWave`)の設定をクラウドサービスを使うように設定変更

```Ruby
# app/uploaders/picture_uploader.rb

CarrierWave::Uploader::Base
  # Include RMagick or MiniMagick support:
  # include CarrierWave::RMagick
  include CarrierWave::MiniMagick

  # Choose what kind of storage to use for this uploader:
  storage :file ←下記に変更

  # production, development でクラウドストレージに画像を保存する
  if Rails.env.production?
    storage :fog
  elseif Rails.env.development?
  else
    storage :file
  end
```

### 7. 環境変数読込ファイル作成(`config/initializers/corrier_wave.rb`)

```Ruby
# config/initializers/carrier_wave.rb

CarrierWave.configure do |config|
  config.fog_credentials = {
    # Amazon S3用の設定
    :provider              => 'AWS',
    :region                => ENV['S3_REGION'],
    :aws_access_key_id     => ENV['S3_ACCESS_KEY'],
    :aws_secret_access_key => ENV['S3_SECRET_KEY']
  }
  config.fog_directory     =  ENV['S3_BUCKET']
end
```

### 8. `.env` ファイルに開発環境用のAWS S3 の環境変数を追加
j
```Ruby
# .env

S3_REGION = "ap-northeast-1"
S3_ACCESS_KEY_ID = "CSV のAccess key ID"
S3_SECRET_ACCESS_KEY = "CSV のSecret access key"
S3_BUCKET = "departure-app"
```

### 9. 開発環境で環境変数が設定されているか確認

```Shell
# 環境変数の設定を確認

$ rails c
> ENV['S3_REGION']
ap-northeast-1
> ENV['S3_ACCESS_KEY_ID']
CSV のAccess key ID
> ENV['S3_SECRET_ACCESS_KEY']
CSV のSecret access key
> ENV['S3_BUCKET']>
departure-app
```

### 11. 開発環境からアップロードした画像をAWS マネジメントコンソールで確認

AWS にアップロードされているか確認

1. AWS マネジメントコンソールにログイン
2. サービスからS3 選択
3. バケットから`departure-app` を選択
4. `uploads` ディレクトリの作成を確認
5. ディレクトリを辿って画像が保存されていることを確認
6. 画像の`オブジェクトURL` をクリックして画像の表示を確認

### 12. 本番環境用の環境変数をheroku に設定

```Shell
# heroku に環境変数設定

$ heroku config:set S3_ACCESS_KEY="CSV のAccess key ID"
$ heroku config:set S3_SECRET_KEY="CSV のSecret access key"
$ heroku config:set S3_BUCKET="departure-app"
$ heroku config:set S3_REGION="ap-northeast-1"

# heoku の環境変数の確認
$ heroku config
.
S3_ACCESS_KEY:
S3_BUCKET:
S3_REGION:
S3_SECRET_KEY:
```

### 13. 手動でアセットコンパイルして動作確認

手動コンパイルしてデプロイ時にエラーが出ないか確認

```Shell
# 開発環境
$ RAILS_ENV=development bin/rails assets:precompile

# 本番環境
$ RAILS_ENV=production bin/rails assets:precompile
```

### 14. 作業ブランチをmaster ブランチにマージ

作業ブランチをmaster にマージ > heroku デプロイ > seed 再生成

```Shell
$ git checkout master
$ git merge branch_name
$ git push heroku master
$ heroku pg:reset DATABASE
$ heroku run rails db:migrate
$ heroku run rails db:seed RAILS_ENV=production
$ heroku restart
```
