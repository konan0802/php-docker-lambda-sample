# **php-docker-lambda-sample**

## **ローカルでのテスト方法**
1. カスタムランタイムイメージのビルド
```
$ docker build -t php-docker-lambda-sample .
```
2. 関数をローカルで実行
```
docker run -p 9000:8080 php-docker-lambda-sample:latest
```
3. curlで対象のエンドポイントにイベントを投稿
```
curl "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{"queryStringParameters": {"name":"Ben"}}'
```

## **デプロイ方法**
### ◇ECRのimageをセット
1. ECRリポジトリの作成
2. AWS CLIのセットアップ（[brewでAWS CLIのインストール](https://qiita.com/okhrn/items/8da6b217d3b1fce63371)）
3. AWSのコンソールに現れるプッシュコマンド通りにECRにimageをpush
### ◇Lamndaの立ち上げ
1. 関数の作成の際にコンテナイメージを選択
2. ［コンテナイメージ URI］に先ほどpushしたimageを選択する

## **参考にしたリポジトリ**
* [aws-samples/php-examples-for-aws-lambda](https://github.com/aws-samples/php-examples-for-aws-lambda/tree/master/0.7-PHP-Lambda-functions-with-Docker-container-images)