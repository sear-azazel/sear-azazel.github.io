---
author:
  name: "az"
date: 2020-02-04T00:00:00+09:00
linktitle: "Android: C++のライブラリ(OpenCV)を呼ぶ"
type:
- post 
- posts
title: "Android: C++のライブラリ(OpenCV)を呼ぶ"
draft: false
tags: ["学び", "Kotlin", "Android"]
categories: ["Android"]
description: "
今日学んだこと - 2020/02/06
Android: C++のライブラリ(OpenCV)を呼ぶ
"
---

# Android/Kotlin

## C++のライブラリ(OpenCV)を呼ぶ
```kotlin
package jp.co.densansoft.furue_app

import org.opencv.android.OpenCVLoader
import org.opencv.core.Point
import kotlin.math.cos
import kotlin.math.sin

class ImageProcessing {
    companion object {
        init {
            if (!OpenCVLoader.initDebug()) {
                Log.d("OpenCV", "error_openCV")
            }
        }
    }

    fun pos(i: Double): Point {
        val x = 400 - (cos(i) - i * sin(i)) * 10
        val y = 400 + (sin(i) + i * cos(i)) * 10
        return Point(x, y)
    }
}
```

### 備考
OpenCVの導入は機を見て別途記載予定
