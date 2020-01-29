---
author:
  name: "az"
date: 2020-01-29T00:00:00+09:00
linktitle: "WPF: コントロールの下にウィンドウを表示する"
type:
- post 
- posts
title: "WPF: コントロールの下にウィンドウを表示する"
draft: false
tags: ["学び", "WPF", "C#"]
categories: ["WPF"]
description: "
今日学んだこと - 2020/01/29
WPF: コントロールの下にウィンドウを表示する
"
---

# WPF/C#

## コントロールの下にウィンドウを表示する
```C#
Point point = this.Button.PointToScreen(new Point());
this.SubForm.Left = point.X;
this.SubForm.Top = point.Y + this.Button.Height;
```
