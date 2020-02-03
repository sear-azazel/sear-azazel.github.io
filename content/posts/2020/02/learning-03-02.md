---
author:
  name: "az"
date: 2020-02-03T00:00:00+09:00
linktitle: "Flutter: http.getのエラーハンドリングについて"
type:
- post 
- posts
title: "Flutter: http.getのエラーハンドリングについて"
draft: false
tags: ["学び", "Dart", "Flutter"]
categories: ["Flutter"]
description: "
今日学んだこと - 2020/02/03
Flutter: http.getのエラーハンドリングについて
"
---

# Flutter/Dart

## http.getのエラーハンドリングについて
`http.get` の際に、exceptionをキャッチしてくれない現象をどう解決するか

### 修正前
```dart
static Future<String> fetch(String url) async {
  try {
    final response = await http.get(url);
    :
  } on Exception catch (_) {
    return Future.value('Page not found...');
  }
}
```

### 修正後
```dart
static Future<String> fetch(String url) async {
  try {
    final response = await http.get(url);
    :
  } catch (_) {
    return Future.value('Page not found...');
  }
}
```

### 補足
ただ、`Avoid catches without on clauses. dart(avoid_catches_without_on_clauses)` の制約に引っかかってしまう
