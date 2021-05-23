# **php-docker-lambda-sample**

## **ローカルでのテスト方法**
1. カスタムランタイムイメージのビルド
```
$ docker build -t php-docker-lambda-sample .
```
2. 関数をローカルで実行
```
$ docker run -p 9000:8080 php-docker-lambda-sample:latest
```
3. curlで対象のエンドポイントにイベントを投稿
```
$ curl "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{"queryStringParameters": {"name":"Ben"}}'
```

<br>

## **デプロイ方法**
## 初回のデプロイ
### ◇ECRのimageをセット
1. ECRリポジトリの作成
2. AWS CLIのセットアップ（[brewでAWS CLIのインストール](https://qiita.com/okhrn/items/8da6b217d3b1fce63371)）
3. AWS CLIによるログイン
```
$ aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 553532066626.dkr.ecr.ap-northeast-1.amazonaws.com
```
4. イメージをビルド
```
$ docker build -t php-docker-lambda-sample .
```
5. イメージにタグ付け
```
$ docker tag php-docker-lambda-sample:latest 553532066626.dkr.ecr.ap-northeast-1.amazonaws.com/php-docker-lambda-sample:latest
```
6. イメージをプッシュ
```
$ docker push 553532066626.dkr.ecr.ap-northeast-1.amazonaws.com/php-docker-lambda-sample:latest
```

### ◇Lamndaの立ち上げ
1. 関数の作成の際にコンテナイメージを選択
2. ［コンテナイメージ URI］に先ほどpushしたimageを選択する

<br>

## 2回目以降のデプロイ
### ◇ECRのimageをセット
#### 初回デプロイ作業の3~6を行う
1. AWS CLIによるログイン
```
$ aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 553532066626.dkr.ecr.ap-northeast-1.amazonaws.com
```
2. イメージをビルド
```
$ docker build -t php-docker-lambda-sample .
```
3. イメージにタグ付け
```
$ docker tag php-docker-lambda-sample:latest 553532066626.dkr.ecr.ap-northeast-1.amazonaws.com/php-docker-lambda-sample:latest
```
4. イメージをプッシュ
```
$ docker push 553532066626.dkr.ecr.ap-northeast-1.amazonaws.com/php-docker-lambda-sample:latest
```

### ◇Lamndaの立ち上げ
1. Lambdaのコンソール画面で［新しいイメージをデプロイ］ボタンを押す

<br>

## **参考にしたリポジトリ**
* [aws-samples/php-examples-for-aws-lambda](https://github.com/aws-samples/php-examples-for-aws-lambda/tree/master/0.7-PHP-Lambda-functions-with-Docker-container-images)