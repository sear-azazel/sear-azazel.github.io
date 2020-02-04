---
author:
  name: "az"
date: 2020-02-04T00:00:00+09:00
linktitle: "Flutter: 階層の異なるWidget間の状態変更を通知する"
type:
- post 
- posts
title: "Flutter: 階層の異なるWidget間の状態変更を通知する"
draft: false
tags: ["学び", "Dart", "Flutter"]
categories: ["Flutter"]
description: "
今日学んだこと - 2020/02/04
Flutter: 階層の異なるWidget間の状態変更を通知する
"
---

# Flutter/Dart

## 階層の異なるWidget間の状態変更を通知する
階層が異なるWidget間で、片方のWidgetの状態を変更しても、もう片方は再ビルドされない。
そこで、Widget間で通知を行う処理を実装する。

### Providerパッケージの追加
```yaml
dependencies:
  provider: ^4.0.2
```

### 実装
#### ChangeNorifier
```dart
class HogeChangeNotifier extends ChangeNotifier {
  void hogeChanged() => notifyListeners();
}
```

#### WidgetA
後述のWidgetBとWidgetCの共通の親
```dart
class WidgetA extends StatefulWidget {
  const WidgetA({Key key}) : super(key: key);

  @override
  _WidgetAState createState() => _WidgetAState();
}

class _WidgetAState extends State<WidgetA> {
  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider<HogeChangeNotifier>(
      create: (context) => HogeChangeNotifier(),
      child: Container(),
    );
  }
}
```

#### WidgetB
通知を送るWidget
```dart
class WidgetB extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return RaisedButton.icon(
      // Provider経由でincrement関数を呼ぶ
      onPressed: () =>
        Provider.of<HogeChangeNotifier>(
          context,
          listen: false,  // listen:falseにより、こちらはリビルドされない
        ).hogeChanged();
    );
  }
}
```

#### WidgetC
通知を受けるWidget
```dart
class WidgetC extends StatefulWidget {
  const WidgetC({Key key}) : super(key: key);

  @override
  _WidgetCState createState() => _WidgetCState();
}

class _WidgetCState extends State<WidgetC> {
  @override
  Widget build(BuildContext context) {
    return Consumer<TagSelectionChangeNotifier>(
      builder: (_, __, ___) {
        return Container();
      }
    );
  }
}
```
