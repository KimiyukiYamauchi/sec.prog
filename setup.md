# 目次

- [VirtualBoxのインストール](#virtualboxのインストール)
  - [1. OSの更新](#1-osの更新)
  - [2. インストール](#2-インストール)
  - [3. 起動](#3-起動)
- [OWASP ZAPのインストール](#owasp-zapのインストール)
  - [1. インストーラのダウンロード](#1-インストーラのダウンロード)
  - [2. インストーラを実行](#2-インストーラを実行)
  - [3. 起動](#3-起動)

---

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
