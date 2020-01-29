---
author:
  name: "az"
date: 2020-01-28T00:00:00+09:00
linktitle: "WPF: Drag&Dropを実装する"
type:
- post 
- posts
title: "WPF: Drag&Dropを実装する"
draft: false
tags: ["学び", "C#", "WPF"]
categories: ["WPF"]
description: "
今日学んだこと - 2020/01/28
WPF: Drag&Dropを実装する
"
---

# WPF/C#

## Drag&Dropを実装する

デザイン

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition></RowDefinition>
        <RowDefinition></RowDefinition>
    </Grid.RowDefinitions>
    <Button 
        Grid.Row="0" x:Name="x" Height="24" Width="100" Content="X" AllowDrop="True"
        PreviewMouseDown="ButtonPreviewMouseDown"
        PreviewMouseMove="ButtonPreviewMouseMove"
        Drop="ButtonDrop"/>
    <Button
        Grid.Row="1" x:Name="y" Height="24" Width="100" Content="Y" AllowDrop="True"
        PreviewMouseDown="ButtonPreviewMouseDown"
        PreviewMouseMove="ButtonPreviewMouseMove"
        Drop="ButtonDrop"/>
</Grid>
```

ロジック

```C#
private Point startPoint;
private const key = "data";

private void ButtonDrop(object sender, DragEventArgs e)
{
    IDataObject dragTarget = e.Data;

    if (!dragTarget.GetDataPresent(key)) { return; }

    int data = (int)dragTarget.GetData(key);
}

private void ButtonPreviewMouseDown(object sender, MouseEventArgs e)
{
    startPoint = e.GetPosition(null);
}

private void ButtonPreviewMouseMove(object sender, MouseEventArgs e)
{
    if (e.LeftButton == MouseButtonState.Released) { return; }

    Point mousePos = e.GetPosition(null);
    Vector diff = startPoint - mousePos;

    if (Math.Abs(diff.X) > SystemParameters.MinimumHorizontalDragDistance ||
        Math.Abs(diff.Y) > SystemParameters.MinimumVerticalDragDistance)
    {
        DataObject dataObject = new DataObject();
        int data = 1;
        dataObject.SetData(key, data);

        DragDrop.DoDragDrop(sender as Button, dataObject, DragDropEffects.Move);
    }
}
```
