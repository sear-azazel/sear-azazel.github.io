---
author:
  name: "az"
date: 2020-01-29T00:00:00+09:00
linktitle: "Flutter: FutureBuilderを使って非同期でWidgetを生成する"
type:
- post 
- posts
title: "Flutter: FutureBuilderを使って非同期でWidgetを生成する"
draft: false
tags: ["学び", "Flutter", "Dart"]
categories: ["Flutter"]
description: "
今日学んだこと - 2020/01/29
Flutter: FutureBuilderを使って非同期でWidgetを生成する
"
---

# Flutter/Dart

## FutureBuilderを使って非同期でWidgetを生成する
```Dart
Widget getIcon(String url) {
  return FutureBuilder(
    future: existSite(url),
    builder: (BuildContext context, AsyncSnapshot<bool> snapshot) { // 第二引数はfutureの戻りの型
      if (snapshot.hasData && snapshot.data) {
        return CachedNetworkImage(
          imageUrl: 'http://favicon.st-hatena.com/?url=$url',
          placeholder: (context, url) => Icon(Icons.not_interested),
        );
      } else {
        return Icon(Icons.not_interested);
      }
    },
  );
```

### 参考URL
https://qiita.com/ysknsn/items/76c6326c74dc9059ff20