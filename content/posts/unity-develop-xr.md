---
author:
  name: "az"
date: 2019-12-17 00:00:00
linktitle: "UnityでVRアプリを作成しVR/ノーマルの切り替えや視線イベントを実装する"
type:
- post
- posts
title: "UnityでVRアプリを作成しVR/ノーマルの切り替えや視線イベントを実装する"
draft: true
tags: ["Unity", "C#", "開発", "VR"]
categories: ["Unity"]
description: "UnityのVR切り替えについて情報が少ないので備忘録の意味も込めて記事にします。"
---

UnityやGoogleVRのバージョンが上がるたびに仕様が変わっていくので、
備忘録を込めて現時点のバージョンの実装方法を記載します。

また、VRモードではタッチでの操作が出来ないため、視線イベントが必要となります。
その実装についても併せて記載したいと思います。

# 前置き

Google VR SDK for Unityのgithubリポジトリは現在アーカイブされています。

Cardboardのソフトウェア関連技術がオープンソース化となり、
Daydreamの販売終了とニュースもありますが、Googleが完全にVR手放したとしても、
ソースコードは生き続けるようです。

# 目標

UnityでVRを実装し、iOS or Androidに出力するまでを行います。

また、アプリ起動時は1眼(通常モード)の状態で、ゲーム開始時に2眼(VRモード)になるように組み込みたいと思います。

# 前提条件

### 必要なもの
- スマートフォン
    - iOS or Android
- 開発環境/ツール(Unity以外)
    - Android SDK
        - Android NDKもあれば良い(必須ではない)
    - Xcode
    - Visual Studio (for Mac)
        - コーディングで利用する
        - Unityと併せてインストールできる(はず)
        - コードエディタは何でも良いが、利便性を考えるとVisual Studio一択
        - おすすめはVisual Studio 2019 (Windows)
            - 以前のバージョンに比べて3倍速い

### Unity Hubのバージョン
- 2.2.1
    - [Unity Hub](https://public-cdn.cloud.unity3d.com/hub/prod/UnityHubSetup.exe) ← ここからダウンロード
    - (補足) Unityをプロジェクトごとにバージョン指定して利用するためのHubツール
        - 複数のバージョンのUnityをディレクトリを分けてインストール出来る
            - 複数プロジェクトを扱う時に便利
        - 利用しなくても問題ない

### Unityのバージョン
- 2018.4.13f (LTS)
    - Unity Hubからインストール
- その他のバージョン及び、直接ダウンロードする場合はこちら
    - [Unity ダウンロードアーカイブ](https://unity3d.com/jp/get-unity/download/archive)
    - [長期サポート](https://unity3d.com/jp/unity/qa/lts-releases)
    - [パッチリリース](https://unity3d.com/jp/unity/qa/patch-releases)

### google VRのバージョン
- v1.200.1
    - [Google VR SDK for Unity](https://github.com/googlevr/gvr-unity-sdk/releases/) ← ここからダウンロード
        - GoogleVRForUnity_1.200.1.unitypackageをダウンロードする
  
# 開発手順

## Unityでプロジェクトを作る

メニューシーンとゲームシーンの2つを用意して、1眼と2眼を切り替えるための準備をします。
今回は簡単に、メニューシーンに「開始」ボタン、ゲームシーンに「終了」ボタンを設けて実現したいと思います。

1. Unityを立ち上げて、プロジェクトを作成します。
    - テンプレートは３Dを選択してください。
1. メニューシーンを作成します。
    1. デフォルトで作成されている「SampleScene」をリネームして「Menu」とします。
    1. Projectタブで右クリック→Create→Sceneと選択して、「Game」シーンを作成します。
1. メニューシーンに「開始」ボタン、ゲームシーンに「終了」ボタンを配置します。
    - Hierarchyタブで右クリック→UI→Buttonと選択します。
        ```txt
        * Main Camera
        * Directional Light
        * EventSystem
        * Canvas
            * Button
                * Text
        ```
1. ボタンにシーン遷移スクリプトを追加します。
    1. Projectタブで右クリック→Create→C# Scriptと選択し、「Menu」のスクリプトを作成します。
    1. Unityエディタのメニュー→Assets→Open C# Projectと選択し、Visual Studioを起動します。
    1. 下記のように実装します。
        ```C#
        // Menu.cs
        using UnityEngine;
        using UnityEngine.SceneManagement;

        public class Menu : MonoBehaviour {
            public void PlayGame() {
                SceneManager.LoadScene("Game");
            }
        }
        ```
    1. Hierarycyタブで右クリック→GameObjectと選択し、スクリプトをアタッチするためのGameObjectを作成します。
        - 作成したGameObjectのInspectorのAdd Compornentを選択し、先ほど作成したMenu.csを追加します。
    1. 
1. 同様にゲームシーンを作成します。
    1. 
        ```C#
        // Game.cs
        using System.Collections;
        using UnityEngine;
        using UnityEngine.SceneManagement;
        using UnityEngine.UI;

        public class Game : MonoBehaviour {
            [SerializeField] private Text countdownText;
            private Coroutine countdown = null;

            public void OnEnter() {
                this.countdown = this.StartCoroutine(this.Countdown());
            }

            public void OnExit() {
                this.StopCoroutine(this.countdown);
                this.countdownText.text = string.Empty;
            }

            private IEnumerator Countdown() {
                const int sec = 2;
                float start = Time.realtimeSinceStartup;
                float elapsed = sec;
                do
                {
                    this.countdownText.text = $"{(int)(sec - elapsed)}";
                    elapsed = Time.realtimeSinceStartup - start;
                    yield return null;
                } while (elapsed < sec);
                SceneManager.LoadScene("Menu");
            }
        }
        ```

## プラグインのインポート

Unityエディタのメニュー→Assets→Import Package→Custom Package…と選択し、ダウンロードしたGoogle VR SDK(GoogleVRForUnity_1.200.1.unitypackage)を選択する。


## ビルド設定およびXRの有効化

Unityエディタのメニュー→File→Build Settings…と選択し、「Player Settings…」ボタンをクリックする。

主な設定事項は下記。

### [必須] Resolution and Presentation
#### Orientation
- Default Orientation: Landscape (Left or Right)

### [必須] XR Settings (iOS/Android同じ)
- Virtual Reality Supported: true
- Virtual Reality SDKs **(以下の順番は大事。上にある方がアプリ起動時の状態となるため)**
    - None
    - Cardboard

### Other Settings
#### Identification
- Bundle Identifier: (よしなに)
- Version: (よしなに)
- Build: (よしなに)

#### Configuration
- Api Compatibility Level: .NET 4.x (Androidの場合NDKが必須)

#### [Androidのみ]
#### Target Architectures 
- ARMv7: false
- ARM64: true
- x86: false

#### [iOSのみ]
- Architecture: ARM64


## VR切り替え用スクリプトの作成



```C#
var x = 0;
```

## Cameraの設定
カメラを固定したい場合に利用します。

VRではデフォルトではトラッキングが有効になっているので、無効にする場合の設定を行います。

以上