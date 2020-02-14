---
author:
  name: "az"
date: 2020-02-14T00:00:00+09:00
linktitle: "JNI: AndroidでC++からJava/Kotlinに値を返す"
type:
- post 
- posts
title: "JNI: AndroidでC++からJava/Kotlinに値を返す"
draft: false
tags: ["学び", "JNI", "C++"]
categories: ["JNI"]
description: "
今日学んだこと - 2020/02/14
JNI: AndroidでC++からJava/Kotlinに値を返す
"
---

# JNI/C++

## Android でC++を実行する

### Native(C++)から、Java/Kotlinに値を返す

Native側の変数の型はjint, jbooleanなど、通常のint型、boolean型とは異なるため、キャストが困難である。

そのため、値を返すために、C++から呼び出し元のJava/Kotlinの変数に値をセットする方法で受け渡しを行う。

```C++
#include <jni.h>

extern "C" {

void Set_double2jdoubleFld(JNIEnv *, jobject, jclass, const char *, double);

JNIEXPORT jboolean JNICALL Java_/*package name*/_KotlinClass_exec
        (JNIEnv *env,
         jobject thiz,
         jstring arg) {

    jclass clazz = env->GetObjectClass(thiz);

    const char *str = env->GetStringUTFChars(arg, 0);
    // strを用いる処理
    env->ReleaseStringUTFChars(arg, srcPath);

    // Java/Kotlinに値を渡す
    double ans = 0.0;
    Set_double2jdoubleFld(env, thiz, clazz, "ans", ans);

    return 1;
}

void Set_double2jdoubleFld(
        JNIEnv *env,
        jobject mythis,
        jclass clazz,
        const char *FieldName,
        double dblbuf) {
    jfieldID fid = env->GetFieldID(clazz, FieldName, "D");
    env->SetDoubleField(mythis, fid, dblbuf);
}

}
```

```kotlin
class KotlinClass {
    companion object {
        init {
            System.loadLibrary("native-lib")
        }
    }

    private external fun exec(arg: String): Boolean

    public var ans: Double = -1.0

    fun execute(arg: String) {
        exec(arg)
    }
}
```

#### 参考
https://w.atwiki.jp/kmhydo/pages/83.html