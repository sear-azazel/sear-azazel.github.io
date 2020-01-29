---
author:
  name: "az"
date: 2020-01-29T00:00:00+09:00
linktitle: "WPF: スクロールバーのダブルクリックを無効にする"
type:
- post 
- posts
title: "WPF: スクロールバーのダブルクリックを無効にする"
draft: false
tags: ["学び", "C#", "WPF"]
categories: ["WPF"]
description: "
今日学んだこと - 2020/01/29
WPF: スクロールバーのダブルクリックを無効にする
"
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
