# serverless

AWSの使用料金をdiscordへ通知するツールになります。

## 事前に実施する事項

以下のコマンドを使用できていること。

```bash
servless
aws(aws configureによる設定済であること。)
```

## 参考URL

[AWS CLIを利用するため必要な初期設定について](https://dev.classmethod.jp/articles/aws-cli_initial_setting/)

[Serverless Framework のインストールから AWS へのデプロイまで](https://zenn.dev/ombran/articles/serverless-install-and-aws-deploy)

[外部サービスからDiscordにメッセージを送る（Webhook)](https://zenn.dev/lambta/articles/5edbda4ccb1ec6)

[AWS Secrets Managerを使おう！](https://qiita.com/mm-Genqiita/items/f93285a6058c64b39f23)

[AWS X-RayをServerless Frameworkで簡単構築・可視化まで](https://acro-engineer.hatenablog.com/entry/2018/07/31/120000)

## 構築方法

### discrordにてwebhookURLを入手する。

以下のサイトを参照してwebhook_URLを入手する。
名前は適当でよい。

[外部サービスからDiscordにメッセージを送る（Webhook)](https://zenn.dev/lambta/articles/5edbda4ccb1ec6)

### discordで入手した投稿に必要な情報をAWS SecretManagerに登録する

設定内容
```bash
種類:その他
キー名:DISCORD_WEBHOOK
値:前の手順で入手したURL
名前:discord/prod
```

以下のコマンドを入力して構築する。

```bash
npm install aws-sdk aws-xray-sdk serverless-plugin-tracing
git clone https://github.com/kiyomaru/serverless.git
cd serverless/services/aws-billing
serverless deploy
```

毎日10:00にdiscordにて特定のチャンネルに、AWS利用料が通知されていることを確認する。

## 通知時刻変更方法

LambdaからCloudWatchイベントを開き、スケジュールがUTC1:00(JPT10:00)となっているのを
適当な時刻に修正する。

## 任意のタイミングで通知する方法

Lambdaを開き、batch-prod-aws-billing関数のテスト実行を行う。
テストが成功し、投稿されていることを確認する。

## 削除方法

```bash
cd serverless/services/aws-billing
serverless remove
```

