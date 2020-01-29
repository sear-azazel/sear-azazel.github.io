---
author:
  name: "az"
date: 2020-01-28 00:00:00
linktitle: "今日学んだこと - 2020/01/28"
type:
- post 
- posts
title: "今日学んだこと - 2020/01/28"
draft: false
tags: ["学び", "Dart", "Flutter"]
categories: ["学び"]
description: "今日学んだこと - 2020/01/28"
---

### Flutter/Dart

#### リストをスライドしてアクションを実行するボタンを表示する

[flutter_slidable](https://pub.dev/packages/flutter_slidable)を利用する

- pubspec.yamlに記載して、プラグインを追加する

  ```yaml
  version: 0.0.1+1

  environment:
    sdk: ">=2.2.0 <3.0.0"

    dependencies:
      flutter:
        sdk: flutter
    flutter_slidable: ^0.5.4
  ```

- Widgetを作成する

  ```Dart
  return ReorderableListView(
    children: List.generate(
      _list_.length,
      (index) {
        return Slidable(
          key: ValueKey('$index'),
          actions: <Widget>[
            IconSlideAction(
              caption: 'Edit',
              color: Colors.blue,
              icon: Icons.edit,
              onTap: () {},
            ),
          ],
          secondaryActions: <Widget>[
            IconSlideAction(
              caption: 'Delete',
              color: Colors.red,
              icon: Icons.delete,
              onTap: () async => await _removeSiteContents(index),
            )
          ],
          child: Container(),
          actionPane: SlidableDrawerActionPane(),
        );
      },
    ),
  );
  ```

##### 備考
Dismissableと似ているが、Dismissableはスライドして要素削除

##### 参考URL
https://webbibouroku.com/Blog/Article/flutter-list-item-delete
