---
author:
  name: "az"
date: 2020-01-29T00:00:00+09:00
linktitle: "Flutter: 動的にTextFieldを広げる"
type:
- post 
- posts
title: "Flutter: 動的にTextFieldを広げる"
draft: false
tags: ["学び", "Flutter", "Dart"]
categories: ["Flutter"]
description: "
今日学んだこと - 2020/01/29
Flutter: 動的にTextFieldを広げる
"
---

# Flutter/Dart

## 動的にTextFieldを広げる
```Dart
class MultipleLineTextField extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scrollbar(
      child: TextField(
        maxLines: 10, // ここと
        minLines: 1,  // ここを設定するだけ
      ),
    );
  }
}
```

### 参考URL
https://qiita.com/kouT127/items/da258adcb8b0cd9def50