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

[宮崎 IT 関連勉強会 Advent Calendar 2019](https://qiita.com/advent-calendar/2019/miyazaki) 23日目の記事です。

UnityではVRは簡単に実装できますが、VRとノーマルモードの切り替えがちょっとややこしいです。
また、VRモードではタッチでの操作が出来ないため、視線イベントが必要となります。
ボタンを3秒見つめたら、ボタンタッチとするという仕組みです。

それと、UnityやGoogleVRのバージョンが上がるたびに仕様が変わっていくので、
備忘録の意味も込めて現時点のバージョンの実装方法を記載します。

説明で用いるプロジェクトを下記リポジトリに用意しているので、良ければ参考にしてください。

https://github.com/sear-azazel/SampleVR.git

# 前置き

Google VR SDK for Unityのgithubリポジトリは現在アーカイブされています。

Cardboardのソフトウェア関連技術がオープンソース化となり、
Daydreamの販売終了とニュースもありますが、Googleが完全にVR手放したとしても、
ソースコードは生き続けるようです。

# 目標

UnityでVRを実装し、iOS or Androidに出力するまでを行います。

また、アプリ起動時は1眼(ノーマルモード)の状態で、ゲーム開始時に2眼(VRモード)になるように組み込みたいと思います。

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

# 開発前補足

VRとそうでない状態の言葉を使い分けるために、VRモード、ノーマルモードという言葉を使います。

# 開発手順

## プラグインのインポート

まずは、プラグインのインポートを行います。

Unityエディタのメニュー→Assets→Import Package→Custom Package…と選択し、ダウンロードしたGoogle VR SDK(GoogleVRForUnity_1.200.1.unitypackage)を選択します。

インポートには少々時間がかかります。

## Unityでプロジェクトを作る

メニューシーンとゲームシーンの2つを用意して、VRモードとノーマルモードを切り替えるための準備をします。
今回は簡単に、メニューシーンに「開始」ボタン、ゲームシーンに「終了」ボタンを設けて実現したいと思います。

### プロジェクト共通

1. Unityを立ち上げて、プロジェクトを作成します。
    - テンプレートは3Dを選択してください。
1. Unityエディタ上に複数タブが表示されますが、下記タブが常時表示されていると開発しやすいと思います。タブの説明を併せて簡単に行います。
    - Hierarchy
        - 3D空間上に配置するオブジェクトが階層状態で表示されます。子階層のオブジェクトは位置/回転、表示状態に依存します。
    - Project
        - Unityプロジェクトで用いるファイルが表示されます。プラグインやスクリプト、3Dオブジェクトモデル、画像、音源などの実ファイルです。Explorer/Finderのファイルがそのまま表示されています。(.metaデータ除く)
    - Inspector
        - Hierarchyタブで選択したオブジェクトのコンポーネントが表示されます。コンポーネントは自由に追加/削除出来ます。全てのオブジェクトはTransformコンポーネントがアタッチされており、このコンポーネントが位置/回転/スケール情報を持っています。
    - Scene
        - 3D空間です。実際に配置したオブジェクトが表示されます。この画面でもオブジェクトを直接移動/回転/リスケール出来ます。
    - Game
        - アプリとして起動する状態が確認できます。

### メニューシーン

1. メニューシーンを作成します。
    - デフォルトで作成されている「SampleScene」をリネームして「Menu」とします。
1. ゲームシーンへ遷移するための「開始」ボタンを配置します。
    1. Hierarchyタブで右クリック→UI→Buttonと選択してください。
        - Gameタブの真ん中にボタンが配置されます。
    1. HierarchyタブでTextオブジェクトを選択し、**Text (Script)** コンポーネントでボタンのラベルを「開始」に変更してください。
    - (補足)ボタンが小さい or 大きい場合は、Canvasの設定で適宜キャンバスサイズを調整してください。
        1. HierarchyタブのCanvasを選択し、**Canvas Scaler (Script)** コンポーネントの**UI Scale Mode**を**Scale With Screen Size**に変更します。
        1. **Reference Resolution**を X: 1280、Y: 720にすると丁度良いかもしれません。
    - (補足の補足)上記設定はキャンバスのサイズを固定化することが出来ます。UnityエディタのGameタブや実機の解像度に関係なくそのサイズでキャンバス内のオブジェクトをサイジングします。
1. 「開始」ボタンにシーン遷移スクリプトを追加します。
    1. Projectタブで右クリック→Create→C# Scriptと選択し、「Menu」のスクリプトを作成してください。
    1. Unityエディタのメニュー→Assets→Open C# Projectと選択し、Visual Studioを起動します。
    1. 下記のように実装します。
        ```C#
        // Menu.cs
        using UnityEngine;
        using UnityEngine.SceneManagement;

        public class Menu : MonoBehaviour {
            public void PlayGame() {
                SceneManager.LoadScene("Game"); // 注1
            }
        }
        ```
        - ___注1: "Game"の文字列は作成したシーン名と同じにしてください。___
    1. スクリプトをアタッチするためのGameObjectを作成します。
        - Hierarycyタブ内の空きスペース(注2)で右クリック→GameObjectと選択してください。
        - ___注2: 別のオブジェクトの配下にGameObjectが追加されても問題はありませんが、気になる場合はDrag&Dropで適宜移動させてください。___
    1. 作成したGameObjectを選択した状態で、InspectorタブにあるAdd Compornentを選択し、先ほど作成したMenu.csを追加します。
    1. Buttonのクリックイベントに先ほど作成したスクリプトのメソッドを紐づけます。
        1. HierarchyタブでButtonオブジェクトを選択し、**Button (Script)** の**On Click ()** 右下の"+"(プラスボタン)をクリックしてください。
        1. **On Click ()** 枠内に左下にある、**None (Object)** の部分に先ほど作成したGameObjectをDrag&Dropします。
        1. **On Click ()** 枠内に右上にある、**No Function** をリストダウンして、Menu→PlayGameと選択してください。
            - 先ほど作成したスクリプトのメソッドです。
    1. 最終的にHierarchyタブはこんな感じになります。
        ```txt
        [Heirarchy]
            - Main Camera
            - Directional Light
            - EventSystem
            - Canvas
                - Button
                    - Text
            - GameObject
        ```
### Utility作成

ゲームシーンを作成する前にVRモードとノーマルモードを切り替えるUtilityを作成します。

Unityが保持するクラスでは、単純に切り替えが出来ないため、視線イベントのためのオブジェクトの設定も加えて実装します。

新規にスクリプトを作成して、下記のように実装します。名前はXRUtilityとします。
```C#
// XRUtility.cs
using System.Collections;
using UnityEngine;
using UnityEngine.XR;

public class XRUtility : MonoBehaviour {
    private const string DeviceNone = "None";
    private const string DeviceCardboard = "Cardboard";

    /// <summary>視線イベントのためのポインタ</summary>
    [SerializeField] private GvrReticlePointer reticlePointer;
    private static XRUtility instance;

    /// <summary>VRモードの切り替え</summary>
    public static bool IsEnabled {
        get => XRSettings.enabled;
        set => instance.StartCoroutine(instance.ChangeEnable(value));
    }

    private void Awake() {
        instance = this;
    }

    private void Start() {
        this.reticlePointer.gameObject.SetActive(false);
    }

    private IEnumerator ChangeEnable(bool enabled) {
        if (XRSettings.enabled == enabled) { yield break; }
        XRSettings.LoadDeviceByName(enabled ? DeviceCardboard : DeviceNone);
        yield return null;
        XRSettings.enabled = enabled;
        this.reticlePointer.gameObject.SetActive(enabled);
    }
}
```

### ゲームシーン

1. メニューシーンと同様にゲームシーンを作成します。
    1. Projectタブで右クリック→Create→Sceneと選択して、「Game」シーンを作成します。
1. メニューシーンへ遷移するための「終了」ボタンを作成します。
    1. 作り方は「開始」と同じです。
    1. メニューシーンと異なるのはCanvasの設定です。
        1. HierarchyタブでCanvasオブジェクトを選択し、Inspectorタブの**Canvas**コンポーネントで**Render Model**を**World Space**に変更してください。
            - そうすると、ボタンが極端に大きくなるので、CanvasのTransformコンポーネントのScaleを0.05前後にしてください。(X, Y, Z同じ値です。)
            - (補足)変更前のCanvasの設定では、画面の上に1枚のレイヤーが存在し、そこにボタンなどのUIを配置するイメージです。現実世界(とゲームの世界の境界線)のインターフェースですが、変更後は、ゲームの世界にボタンを配置するイメージです。そうすることによって、VRとしてボタンを表現することができるようになります。なので、**Render Model**が**World Space**以外だと、VRにはなりません。
    1. 新規にスクリプト作成し、下記のように作成します。名前をGame.csとします。
        ```C#
        // Game.cs
        using System.Collections;
        using UnityEngine;
        using UnityEngine.SceneManagement;
        using UnityEngine.UI;

        public class Game : MonoBehaviour {
            [SerializeField] private Text countdownText;
            private Coroutine countdown = null;

            /// <summary>視線(カーソル)がボタンに重なったときのイベント</summary>
            /// <remarks>カウントダウンの処理を開始します</remarks>
            public void OnEnter() {
                this.countdown = this.StartCoroutine(this.Countdown());
            }

            /// <summary>視線(カーソル)がボタンから離れた時のイベント</summary>
            /// <remarks>カウントダウンの処理を停止します</remarks>
            public void OnExit() {
                this.StopCoroutine(this.countdown);
                this.countdownText.text = string.Empty;
            }

            /// <summary>ボタンが重なって離れるまでに走る処理</summary>
            /// <remarks>カウントダウンが終わったら、シーン遷移します</remarks>
            private IEnumerator Countdown() {
                const int sec = 3;
                float elapsed = 0;
                while (elapsed < sec) {
                    this.countdownText.text = $"{(int)(sec - elapsed)}";
                    elapsed += Time.fixedDeltaTime;
                    yield return null;
                }
                SceneManager.LoadScene("Menu");
            }
        }
        ```
    1. 視線イベントによるカウントダウンを表示するため、CanvasにTextオブジェクトを追加します。
        - Hierarchyタブ内のCanvasオブジェクト上で右クリック→UI→Textを選択してください。
            - 表示位置は適宜修正してください。ボタンと重ならないくらいが丁度良いです。
    1. メニューシーンの時と同様にスクリプトをアタッチするためのGameObjectを作成して、Game.csを追加してください。
        1. Inspectorタブ上の**Game (Script)**コンポーネントに**Countdown Text**項目が表示されるので、先ほど作成したTextオブジェクトをHierarchy上から、**Countdown Text**項目の右に表示されている**None (Text)**部分にDrag&Dropしてください。
        1. メニューシーンと同様にButtonのイベントにメソッドを紐付けます。
            1. **Button**に**Event Trigger**コンポーネントを追加してください。
            1. Add New Event Typeボタンをクリックして、Pointer EnterとPoiner Exitを追加してください。
            1. メニューシーンの作成で、ButtonのOn Clickにメソッドを紐付けたように、以下２つを紐付けてください。
                - Pointer Enter: OnEnter
                - Pointer Exit: OnExit
    1. Utilityスクリプトを追加します。
        1. 先ほど作成したXRUtility.csをGame.csを追加したGameObjectに追加してください。
        1. **XRUtility (Script)**コンポーネントに**Reticle Pointer**が表示されるので、Main Cameraの直下に配置した、GvrReticlePointerをDrag&Dropしてください。
    1. VR用のイベントを拾うための設定を行います。
        1. HierarchyタブでEvent Systemを削除して、GoogleVRのGvrEventSystem(Prefab)をProjectタブからDrag&Dropして追加してください。
            - Assets/GoogleVR/Prefabs にあります。
        1. HierarchyタブのMain CameraにGvrReticlePointer(Prefab)を追加してください。
            - Assets/GoogleVR/Prefabs/Cardboard にあります。
    1. エディタ上で動作確認するためのオブジェクトを配置します。
        - GvrEditorEmulator(Prefab)を追加してください。
            - Assets/GoogleVR/Prefabs/EventSystem にあります。
        - (補足)以前のGoogle VR SDKでは、エディタ上でのゲーム動作も2眼でしたが、現在のバージョンでは、ノーマルとしてしか動作できません。
    1. 最終的にHierarchyタブはこんな感じになります。
        ```txt
        [Heirarchy]
            - Main Camera
            - Directional Light
            - Canvas
                - Button
                    - Text
                - Text
            - GameObject
            - GvrEventSystem 注3
            - GvrEditorEmulator 注3
        ```
        - ___注3: Prefabとして追加したので、青いアイコンです___

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

#### Configuration
- Api Compatibility Level: .NET 4.x

#### [Android]
#### Identification
- Package Name: (よしなに)
- Version: (よしなに)
- Bundle Version Code: (よしなに)
- Minimum API Level: Android 4.4 'KitKat' (API level 19)

#### Target Architectures 
- ARMv7: false
- ARM64: true
- x86: false

#### [iOS]
#### Identification
- Bundle Identifier: (よしなに)
- Version: (よしなに)
- Build: (よしなに)

#### Configuration
- Architecture: ARM64

## ビルド

各Pratform(Android or iOS)向けにビルドを行います。

iOSの場合、Xcodeプロジェクトへのエクスポートになるので、エクスポートした後にXcodeでプロジェクトを開いてビルドしてください。ここではその手順は省きます。

1. Unityエディタのメニュー→File→Build Settings…と選択してください。
    - ビルドしたいPlatformが選択されていない場合は、Switch Pratformを行ってください。
1. Buildボタンを押してください。


# あとがき

長くなった上に、文字が多くて分かりづらいですが、下記リポジトリに作成したプロジェクトをコミットしていますので、参考にしてください。

https://github.com/sear-azazel/SampleVR.git

以上
