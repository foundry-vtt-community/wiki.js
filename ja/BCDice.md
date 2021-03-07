---
title: BCDiceの使い方
description: BCDiceの使い方
published: true
date: 2021-03-07T07:11:32.266Z
tags: 
editor: markdown
dateCreated: 2021-03-04T05:53:50.607Z
---

**このModはまだベータ版です、実際に使ってみてフィードバックをください。**

# BCDiceとは
日本で最も使われている、TRPG用ダイスロール処理システムです。どどんとふ、ココフォリア、ユドナリウム、TRPGスタジオなど様々なオンセツールで使われています。（https://bcdice.org/ より引用）

## 対応TRPGシステム
-   クトゥルフ神話TRPG, 新クトゥルフ神話TRPG
-    ソード・ワールドRPG, 2.0, 2.5
-    シノビガミ
-    インセイン
-    アリアンロッドRPG, 2E
-    ダブルクロス The 2nd Edition, The 3rd Edition
-    永い後日談のネクロニカ
-    マギカロギア
-    メタリックガーディアンRPG
-    ログ・ホライズンTRPG
-    銀剣のステラナイツ
-    フタリソウサ

など**100以上のシステム**に対応 （https://bcdice.org/ より引用）

# FVTTでのBCDice（必読）
BCDiceは拡張機能モジュール（モッド・Mod）としてFVTTに組み込み可能です。このModはオンセ工房FVTT日本語支部の支援金によってコミッションされています。支援をしたい方は https://www.patreon.com/onsekobo に登録してください。

現在ではBCDiceに登録されている各種システムの便利なコマンドを使うウィンドウ程度の機能です。十分に卓に使えますが、まだチャットパレットににた機能を内包しているわけではありません。チャットパレットも今月にコミッションが始まり、４月末に終わる予定ですので、今の状態で使っていただきフィードバックをいただけると次回のリリースがスムーズになります。

## 導入方法
リポジトリ：https://github.com/jsinme/fvtt-bcdice
（機能提供、プルリク歓迎）

以下のリンクをモジュールとして導入します。
https://github.com/jsinme/fvtt-bcdice/releases/download/0.2.3-beta/module.json

![bcdice_install.jpg](/images/japanese-community/bcdice_install.jpg =700x500)

## 使い方

### Modを起動
ワールドに入って、設定の「モジュールの管理」を開き、モジュール一覧からBCDiceを選択して、モジュールを更新します。
＊現在はどのシステムのワールドでも使用可能ですが、特に希望が無い場合は「Simple World-Building」で十分です。
![bcdice_enable.jpg](/images/japanese-community/bcdice_enable.jpg =700x500)

### Modの使い方
左のツール一覧の下にダイスのアイコンが出現するので、それをクリックするとBCDiceの画面が開きます。システムを選択して、下のコマンドボックスにコマンドを入力するとそれがFVTTのチャットに表示されます。

便利なショートカット：
- CTRL+Shift+B: BCDiceの画面を表示して、キーボードの入力をコマンドボックス合わせます。
- Enter: 現在のコマンドをチャットに送信します。BCDice画面はそのまま。
- Shift+Enter: 現在のコマンドをチャットに送信し、BCDiceの画面を閉じ、FVTTのチャットにキーボードの入力をあわせます。（すなわち、CTRL+Shift+Bとこのコマンドでキーボードから手を話さずにBCDiceを呼び出せます）

![bcdice_use.jpg](/images/japanese-community/bcdice_use.jpg =900x500)

# Q&A

Q: なんで個別ウィンドウなの？
A: 理由は２つです。一つは後にチャットパレットModと融合させるときに拡張しやすいように、もう一つはFVTTの既存のチャットが今後のアップデートで変わったとしてもBCDiceのみは単独で行動するように（管理を楽に）しています。

Q: 不具合がある！　どこに相談すれば良い？
A: 不具合により報告する場所が違います。 FVTTに関わる問題は https://foundryvtt.wiki/ja/home のオンセ工房日本語支援サーバへ（ツイッターは見ませんので直接報告していただけると幸いです）。BCDiceのコマンドそのものに関する不具合は https://bcdice.org/ の公式チャットより報告するようにお願いします。