# jenkins-unity-build
jenkinsでunityをビルドしたい

# 準備

``` .env
JENKINS_SLAVE_SSH_PUBKEY=YOUR_JENKINS_SLAVE_SSH_PUBKEY
UNITY_USERNAME=jenkins@hoge.com
UNITY_PASSWORD=jenkins
DOWNLOAD_URL=YOUR_UNITY_DOWNLOAD_URL
SHA1=YOUR_UNITY_SHA1
```

ダウンロードURLやSHA1はこちらから: <https://forum.unity.com/threads/unity-on-linux-release-notes-and-known-issues.350256/>

# How to activate

```
docker-compose up -d
```

<http://localhost:8080> にjenkinsが立ち上がります。

```
docker exec -it master bash
xvfb-run --auto-servernum --server-args='-screen 0 640x480x24' /opt/Unity/Editor/Unity -logFile -batchmode -username "${UNITY_USERNAME}" -password "${UNITY_PASSWORD}"
exit
```

上記コマンドを実行すると、ログ内に次のようなxmlが出力されます。

```
LICENSE SYSTEM [2017723 8:6:38] Posting <?xml version="1.0" encoding="UTF-8"?><root><SystemInfo><IsoCode>en</IsoCode><UserName>[...]
```

xml部分を `*.alf` ファイルにコピーします。
<https://license.unity3d.com/manual> でその`*.alf`ファイルをアクティベートしてください。
`Unity_*.ulf` ファイルがダウンロードされる為、それを保存します。

```
cp Unity_*.ulf ./jenkins_home/.local/share/unity3d/Unity/Unity_lic.ulf
```

これで準備ができました。
