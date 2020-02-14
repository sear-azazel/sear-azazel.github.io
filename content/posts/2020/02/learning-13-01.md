---
author:
  name: "az"
date: 2020-02-13T00:00:00+09:00
linktitle: "Windows: Visual Studio 6.0 Enterpriseをインストールする方法"
type:
- post 
- posts
title: "Windows: Visual Studio 6.0 Enterpriseをインストールする方法"
draft: false
tags: ["学び", "Windows", "Visual Studio"]
categories: ["Windows"]
description: "
今日学んだこと - 2020/02/13
Windows: Visual Studio 6.0 Enterpriseをインストールする方法
"
---

# Windows/Visual Studio

## Visual Studio 6.0 Enterpriseをインストールする方法
Visual Studio Analyzer を未選択にしてインストールする

以下手順

Visual Studio Analyzerを使用せずにVisual Studio 6.0 Enterprise Editionを再インストールします。
1. カスタムセットアップを使用して、Visual Studio 6.0エンタープライズ-カスタムページで[ エンタープライズツール ]をクリックし、[ オプションの変更 ]をクリックします。
1. [ Visual Studio 6.0 Enterprise-エンタープライズツール]ページの[ オプション]で、[ Visual Studio Analyzer]チェックボックスが選択されていないことを確認します。
1. インストールする他のすべてのVisual Studioコンポーネントをクリックして選択し、[ OK ]をクリックします 。
1. [ 続行]をクリックし、表示される指示に従います。

Visual Studio Analyzerをインストールするには：
1. コントロールパネルで、[ プログラムの追加と削除 ] をダブルクリックし、[ Microsoft Visual Studio 6.0 Enterprise Edition ]をクリックし ます。
1. [ 変更と削除]をクリックします。
1. Visual Studio 6.0 Enterprise Setupで、[ 追加と削除 ] をクリックします。
1. 上のVisual Studio 6.0エンタープライズ-メンテナンスページ、クリックしてください エンタープライズツール]を、[OK]をクリックしますオプションの変更を。
1. [ Visual Studio 6.0エンタープライズ-エンタープライズツール]ページの [ オプション ]で、[ Visual Studio Analyzer ]をクリックし、[ OK ]をクリックします 。
1. [ Visual Studio 6.0 Enterprise-メンテナンス ]ページで、[ 続行 ]をクリックし 、表示される指示に従います。

「現象」に記載されているエラーメッセージが表示されたら、Visual Studioセットアップウィザードを続行します。セットアップは失敗を報告します。

Visual Studio Analyzerを実行するローカルアカウントを作成するには：
1. Visual Studio Enterprise Edition 6.0を実行しているコンピューターで、ローカルユーザーを作成します。ローカルユーザーの作成方法については、Windows 2000ヘルプを参照してください。
1. 分散COM構成（dcomcnfg.exe）を実行します。
1. 上のアプリケーションタブをクリックしMSVSAローカルイベントコンセントレータクラスを、[OK]をクリックします プロパティを。
1. 上のアイデンティティタブ、設定このユーザーをあなたは、この手順のステップ1で作成したユーザーとパスワードに一致します。


### 補足

#### 補足１
インストールに失敗する場合
- データアクセスとグラフィックの項目を選択解除してから実行する
- インストール後、インストーラが応答しなくなるが再起動を行うと、後工程が進行する
- SP6がインストールできない場合は、VB6を再インストールすると完了する

参考

https://lil.la/technology/technology-memo/post-3651

#### 補足２
下記URLの投稿のGoogle翻訳

https://social.msdn.microsoft.com/Forums/vstudio/en-US/f84a9ef0-bee3-4dcf-8045-3b8c56f22180/setup-was-unable-to-create-a-dcom-user?forum%20=%20vclanguage