# 目次

- [Dockerのインストール](#dockerのインストール)
  - [0. 古いDockerを削除（入っている場合のみ）](#0-古いdockerを削除入っている場合のみ)
  - [1. 必要なパッケージをインストール](#1-必要なパッケージをインストール)
  - [2. Docker公式のGPGキーを追加](#2-docker公式のgpgキーを追加)
  - [3. Dockerのリポジトリを追加](#3-dockerのリポジトリを追加)
  - [4. Dockerをインストール](#4-dockerをインストール)
  - [5. 動作確認](#5-動作確認)
  - [6. sudoなしで使う設定（任意）](#6-sudoなしで使う設定任意)
  - [7.実習用仮想マシンのダウンロード](#7実習用仮想マシンのダウンロード)
  - [8. 起動](#8-起動)
  - [9. 停止](#9-停止)
  - [10. 再起動](#10-再起動)
  - [11. phpファイルをホスト側で編集](#11-phpファイルをホスト側で編集)
- [VirtualBoxのインストール](#virtualboxのインストール)
  - [1. OSの更新](#1-osの更新)
  - [2. インストール](#2-インストール)
  - [3. 起動](#3-起動)
- [OWASP ZAPのインストール](#owasp-zapのインストール)
  - [1. インストーラのダウンロード](#1-インストーラのダウンロード)
  - [2. インストーラを実行](#2-インストーラを実行)
  - [3. 起動](#3-起動)
  - [4. 設定](#4-設定)
  - [5. 使用方法](#5-使用方法)
  - [6. サーバ証明書の設定](#6-サーバ証明書の設定)

---

## Dockerのインストール

### 0. 古いDockerを削除（入っている場合のみ）

```bash
sudo apt remove docker docker-engine docker.io containerd runc
```

### 1. 必要なパッケージをインストール

```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg
```

### 2. Docker公式のGPGキーを追加

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### 3. Dockerのリポジトリを追加

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo $VERSION_CODENAME) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 4. Dockerをインストール

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 5. 動作確認

```bash
sudo docker run hello-world
```

### 6. sudoなしで使う設定（任意）

```bash
sudo usermod -aG docker $USER
```

その後、再起動

```bash
docker run hello-world
```

### 7.実習用仮想マシンのダウンロード

- [https://wasbook.org/download/](https://wasbook.org/download/)
- 実習用仮想マシン (Docker版 Ver 1.2.0; ウイルス誤検知対策版) のダウンロード(21MB)

ダウンロードした、zipファイルを解凍

### 8. 起動

#### 1. docker-compose.yml がある場所へ移動

ここにdocker-compose.yml とdb/, apache/, nginx/, tomcat/, mail/がある

#### 2. コンテナをビルドして起動

```bash
docker compose up -d --build
```

- up : 起動
- -d : バックグラウンド起動
- --build : build: db などの設定を使ってイメージを作り直す

#### 3. 起動確認

```bash
docker compose ps
```

#### 4. アクセス先

- IP: 127.0.0.1
- ポート: 13128

つまり、ホストOSからは **127.0.0.1:13128**

#### 5. ログを見る

起動に失敗したときはこれです。

```bash
docker compose logs
```

特定のサービスだけ見るなら

```bash
docker compose logs apache
docker compose logs nginx
docker compose logs db
```

リアルタイムで見るなら

```bash
docker compose logs -f
```

### 9. 停止

```bash
docker compose down
```

### 10. 再起動

```bash
docker compose restart
```

### 11. phpファイルをホスト側で編集

#### 1. ホスト側に同期のためのディレクトリを作成

```bash
mkdir -p wasbook-contents/html
```

#### 2. コンテナからホストへのコピー

```bash
docker compose cp apache:/var/www/html ./wasbook-contents/
```

#### 3. docker-compose.ymlの編集

docker-compose.yml の apache に volumes を追加

```YAML
apache:
  build: apache
  environment:
    - MYSQL_HOST=db
    - TZ=Asia/Tokyo
  volumes:
    - ./wasbook-contents/html:/var/www/html
  ports:
    - ${APACHE_IP:-127.0.0.1}:${APACHE_PROXY_PORT:-13128}:3128
  networks:
    internal:
```

#### 4. コンテナの再起動

```bash
docker compose down
docker compose up -d
```

## VirtualBoxのインストール

### 1. OSの更新

```bash
sudo apt update
sudo apt upgrade
```

### 2. インストール

```bash
sudo apt install virtualbox
```

### 3. 起動

```bash
virtualbox
```

## OWASP ZAPのインストール

### 1. インストーラのダウンロード

- [https://www.zaproxy.org/download/](https://www.zaproxy.org/download/)
- 「Linux Installer」をダウンロード

### 2. インストーラを実行

```bash
sudo bash ZAP_2_17_0_unix.sh
```

- 「/opt/zaproxy」にインストールされます

### 3. 起動

「アプリ表示」から「検索キーワードを入力」で、「ZAP」で起動

### 4. 設定

「ツール」-「オプション」

#### 1. 外部プロキシ

「ネットワーク」-「接続」-「HTTPプロキシ」

- 「有効」をチェック
- 「ホスト」に「127.0.0.1」
- 「ポート」に「13128」

#### 2. ブレークポイント

「ブレークポイント」

- 「ブレークボタンモード」で「リクエストとレスポンスのボタンを個別表示」

#### 3. フォント

「表示」

- 「一般フォント」の「フォント名」で「Noto Sans Mono CJK JP」

#### 4. HUB(デフォルトではずれている)

「HUB」

- 「Enable when using the ZAP Desktop」のチェックを外す

### 5. 使用方法

「クイックスタート」-「手動探索」

- 「探索対象URL」は「http://example.jp」
- 「HUDを有効化」はチェックを外す
- 「アプリケーションを探索」で **「Chrome」** を選んで、
- 「ブラウザを起動」をクリック

### 6. サーバ証明書の設定

#### サーバ証明書のダウンロード

「ツール」-「オプション」-「ネットワーク」-「サーバ証明書」-「ルートCA証明書」

- 「保存」ボタンをクリックすると「zap_root_ca.cer」が作成される

#### 登録

「zap_root_ca.cer」があるディレクトリに移動し、

```bash
sudo apt install libnss3-tools
```

```bash
certutil -d sql:$HOME/.pki/nssdb -A \
  -t "C,," \
  -n "ZAP Root CA" \
  -i zap_root_ca.cer
```

chromeを終了し、

```bash
pkill chrome
```

再起動

#### 登録確認

```bash
certutil -d sql:$HOME/.pki/nssdb -L
```

ここに👇があればOK

```bash
ZAP Root CA
```
