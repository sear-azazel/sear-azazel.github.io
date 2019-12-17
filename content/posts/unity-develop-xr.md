---
author:
  name: "az"
date: 2019-12-17 00:00:00
linktitle: "UnityでVRを実装する"
type:
- post
- posts
title: "UnityでVRを実装する"
draft: true
tags: ["Unity", "C#", "開発", "VR"]
categories: ["Unity"]
description: "UnityのVR切り替えについて情報が少ないので備忘録の意味も込めて記事にします。"
---

UnityやGoogleVRのバージョンが上がるたびに仕様が変わっていくので、
備忘録を込めて現時点のバージョンの実装方法を記録します。

# 前置き
Google VR SDK for Unityのgithubリポジトリは現在アーカイブされています。

Cardboardのソフトウェア関連技術がオープンソース化となり、
Daydreamの販売終了とニュースもありますが、Googleが完全にVR手放したとしても、
ソースコードは生き続けるようです。

# 前提条件

- Unity Hubのバージョン
    - 2.2.1
        - [Unity Hub](https://public-cdn.cloud.unity3d.com/hub/prod/UnityHubSetup.exe) ← こちらからダウンロード
        - (補足) Unityをプロジェクトごとにバージョン指定して利用するためのHubツール
            - 複数のバージョンのUnityをディレクトリを分けてインストール出来る
                - 複数プロジェクトを扱う時に便利
            - 利用しなくても問題ない
- Unityのバージョン
    - 2018.4.13f (LTS)
        - Unity Hubからインストール
        - 直接ダウンロードする場合はこちら
            - [Unity ダウンロードアーカイブ](https://unity3d.com/jp/get-unity/download/archive)
            - [長期サポート](https://unity3d.com/jp/unity/qa/lts-releases)
            - [パッチリリース](https://unity3d.com/jp/unity/qa/patch-releases)
- google VRのバージョン
    - v1.200.1
      - [Google VR SDK for Unity](https://github.com/googlevr/gvr-unity-sdk/releases/download/v1.200.1/GoogleVRForUnity_1.200.1.unitypackage)
  
# 開発手順
## Unityでプロジェクトを作る
1. Unityを立ち上げて、プロジェクトを作成します。
    - テンプレートは３Dを選択してください。

1. 

## プラグインのインポート
あああ

## XRの有効化
Unity Editor上でProject Settingsを開いて、

## VR切り替え用スクリプトの作成
あああ

```C#
var x = 0;
```

## Cameraの設定
カメラを固定したい場合に利用します。

VRではデフォルトではトラッキングが有効になっているので、無効にする場合の設定を行います。

以上