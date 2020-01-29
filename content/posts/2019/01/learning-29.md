---
author:
  name: "az"
date: 2020-01-29 00:00:00
linktitle: "今日学んだこと - 2020/01/29"
type:
- post 
- posts
title: "今日学んだこと - 2020/01/29"
draft: false
tags: ["学び", "C#", "WPF"]
categories: ["学び"]
description: "今日学んだこと - 2020/01/29"
---

### WPF/C#

#### スクロールバーのダブルクリックを無効にする
DataGridを継承したクラスに下記処理を追加する

```C#
protected override void OnPreviewMouseDoubleClick(MouseButtonEventArgs e)
{
    base.OnPreviewMouseDoubleClick(e);

    DependencyObject source = e.OriginalSource as DependencyObject;
    e.Handled = source.FindParent(typeof(ScrollBar)) != null || source is ScrollViewer;
}
```
