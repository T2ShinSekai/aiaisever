# AI-CHAT-API

## 概要
- 会話用のAIを利用したAPI
- いろいろ話すよ

## 開発環境の構築
### 環境変数の設定
src/.envに利用するIAMユーザーのアクセスキーとシークレットキーを記載する
```
AWS_ACCESS_KEY_ID=[アクセスキー]
AWS_SECRET_ACCESS_KEY=[シークレットアクセスキー]
```
※IAMの設定は以下のポリシーを設定したユーザーを使用する必要がある(もしかしたらもっと権限絞れるかも。。)
-  AWSLambdaFullAccess
-  IAMFullAccess
-  AmazonS3FullAccess
-  AmazonEC2ContainerRegistryReadOnly
-  AmazonEC2ReadOnlyAccess
-  AmazonAPIGatewayAdministrator
-  AWSCloudFormationFullAccess
-  CloudWatchLogsFullAccess

### コンテナ立ち上げ～実行
```
docker compose up -d # コンテナの立ち上げ。docker-compose.yamlがあるディレクトリ内で実行

docker exec -it serverless-chat-ai-api bash # コンテナ内に入る

# ※初回クローン時のみ実行
npm install # node_modulesをgitの管理から除外しているため、package.jsonを基にパッケージを取得

sh /opt/startup.sh # ローカルでAPIを起動
=>

   ┌─────────────────────────────────────────────────────────────────────────┐
   │                                                                         │
   │   GET | http://localhost:3000/dev/hello                                 │
   │   POST | http://localhost:3000/2015-03-31/functions/hello/invocations   │
   │                                                                         │
   └─────────────────────────────────────────────────────────────────────────┘
```

### 実行確認
起動シェルで実行後に出力されたURLにアクセス
```
curl http://localhost:3000/dev/hello
=> {"message": "Go Serverless v1.0! Your function executed successfully!", ............
```

レスポンスが返却されて入ればOK！

## デプロイ
ここまで作成してから serverless コマンドでデプロイすると、
Lambda本体だけでなく、
 serverless コマンドが自動でライブラリをインストールしたイメージを作成し、
 AWS LambdaのLayerとしてアップロードされる
```
sls deploy
```
## 削除する場合
```
sls remove
```

## serverless frameworkのテンプレート作成手順
※最初に必要なだけのメモ書きなので無視してください
```
serverless create --template aws-python3
serverless plugin install -n serverless-python-requirements
```