# mailhog-eb-sample
mailhogをAWS Beanstalk Multi container Docker で動かすサンプル

# How to deploy

```
zip -r eb-mailhog.zip Dockerrun.aws.json
```

* 環境枠：ウェブサーバー。
* プラットフォーム：複数コンテナのDocker。
    * コンテナひとつ複数のポートを開ける必要があるので
* アプリケーション： 上で作った eb-mailhog.zip を指定
* TCP:80 と TCP:1025 を開放したセキュリティグループを割り当ててください。
* t2.nanoでも立つよう、コンテナのメモリは128mにしてます。上位にするならもっと増やしてOK

これで環境を起動。

環境URLにアクセスすると、メール確認画面
ポート1025にアクセスすると、SMTPでメールを受信してくれます。

# How to send mail

仮に hogehoge-test-mailhog.ap-northeast-1.elasticbeanstalk.com と立てたとする

telnetでsmtpすれば確認できます。telnetがタイムアウトする場合は、セキュリティグループで1025を開けているか、確認してください。

```
$ telnet hogehoge-test-mailhog.ap-northeast-1.elasticbeanstalk.com 1025
Trying 13.230.255.58...
Connected to hogehoge-test-mailhog.ap-northeast-1.elasticbeanstalk.com.
Escape character is '^]'.
220 mailhog.example ESMTP MailHog
EHLO uluru.jp
250-Hello uluru.jp
250-PIPELINING
250 AUTH PLAIN
MAIL FROM:<s_suzuki@example.com>
250 Sender s_suzuki@example.com ok
RCPT TO:<sasasin@sasasin.net>
250 Recipient sasasin@sasasin.net ok
DATA
354 End data with <CR><LF>.<CR><LF>
From: s_suzuki@example.com
To: sasasin@sasasin.net
Subject: Hello World, MailHog on Elastic Beanstalk!!

おためし
.
250 Ok: queued as aKovEWtm2VJVbQHgILe1LhZETJJpJ0jtWmsqOcWQPiY=@mailhog.example

500 Unrecognised command
quit
221 Bye
Connection closed by foreign host.
$
```

http://hogehoge-test-mailhog.ap-northeast-1.elasticbeanstalk.com にアクセスして、上で送信したメールが見れたらOK。
