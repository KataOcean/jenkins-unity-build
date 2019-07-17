# jenkins-unity-build
jenkinsでunityをビルドしたい

kataocean/jenkins_unity:latest (Unity2018.4.2f1)

# 参考

[https://github.com/wtanaka/docker-unity3d](wtanaka/docker-unity3d)
[https://gitlab.com/gableroux/unity3d](https://gitlab.com/gableroux/unity3d)

# 準備

``` .env
UNITY_USERNAME=jenkins@hoge.com
UNITY_PASSWORD=jenkins
```

ダウンロードURLやSHA1はこちらから: <https://forum.unity.com/threads/unity-on-linux-release-notes-and-known-issues.350256/>

ex.
```
Unity2018.4.2f1
DOWNLOAD_URL=https://beta.unity3d.com/download/d6fb3630ea75/UnitySetup-2018.4.2f1
SHA1=967f3f280ca422a222b58a50850f96e99604aef9
```

# How to build

```
docker build [-t <NAME>] --build-arg DOWNLOAD_URL=<YOUR_UNITY_DOWNLOAD_URL> SHA1=<YOUR_UNITY_DOWNLOAD_SHA1> [COMPONENTS=<Unity|Windows|Windows-Mono|Mac|Mac-Mono|WebGL>] .
```

# How to activate

```
docker-compose up -d
```

<http://localhost:8080> にjenkinsが立ち上がります。

```
docker exec -it -u root master bash
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
