---
author:
  name: "az"
date: 2020-02-04T00:00:00+09:00
linktitle: "Flutter: プロジェクト作成コマンド"
type:
- post 
- posts
title: "Flutter: プロジェクト作成コマンド"
draft: false
tags: ["学び", "Dart", "Flutter"]
categories: ["Flutter"]
description: "
今日学んだこと - 2020/02/04
Flutter: プロジェクト作成コマンド
"
---

# Flutter/Dart

## プロジェクト作成コマンド
```sh
flutter create --org com.example -i swift -a kotlin myapp
```

### その他オプション
`--template=plugin`: プラグイン用プロジェクト

### その他コマンド
#### iOSでビルド
```sh
cd ./example
flutter build ios --no-codesign
```

#### Androidでビルド
```sh
cd ./example
flutter build apk
```
