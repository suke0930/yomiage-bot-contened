
# RVCモデルファイルの導入
`/Docker/RVC/Models`に使いたいモデルを入れてください。
.pthのみ検証しています。

# 構成ファイルの設定
## DiscordのAPI設定
 1.Discordの[開発ポータル](https://discord.com/developers/applications)にアクセスします。  
 2.そこから新規アプリ作成し、ApplicationIDとTokenをメモします。  
 3.Botというタブから「PRESENCE INTENT」,「SERVER MEMBERS INTENT」,「MESSAGE CONTENT INTENT」をすべてオンにします。  
 4.`/Docker/YOMIAGE/configs/config.yml`をテキストエディタで開き、「access_token」、「application_id」をそれぞれ、先程メモしたTokenとApplicationIDに書き換えます。  
```
access_token: DISCORD_ACCESS_TOKEN
client_id: APPLICATION_ID
```
 <br>
 これでDiscordの設定は完了です。

## RVCデフォルトモデルの指定
`/Docker/YOMIAGE/configs/config.yml`の`rvc_default_model`を自分の使いたいモデルの*ファイル名*にしてください。
構文例としては`hogehoge.pth`になると思います。
 
# 実行
``` shell
## `/Docker/`に移動
sudo docker-compose up -d --build 
```