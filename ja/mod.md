---
title: ７．MOD紹介
description: おススメのMOD一覧
published: true
date: 2021-04-21T15:09:13.319Z
tags: 
editor: markdown
dateCreated: 2021-04-06T11:57:56.615Z
---

# MOD紹介

こちらのページでは、FVTTでのセッションを更に便利にしてくれるMOD等を紹介しています。

まだまだ情報が少ないので、便利だと思ったMODを、簡単な紹介と共に情報提供してくださる人を募集中です。
既に挙げられているMODに関して、アップグレードされた点や付け加えたい点の情報でも歓迎します。

以下の情報でも書かれていますが、<u>初めてFVTTを使う方は、先ずMODなしの状態で操作に慣れる事をお勧めします</u>。
理由は、何か問題が起きた時に、MOD由来の物なのか、FVTT由来の物なのかの判別がつきにくい為です。

# 使ってみたModのメモと寸評

提供元：Touge
（一部MOD以外のTipsも含まれています）

このドキュメントは、自分が使ってみたModについてまとめたメモ書きです。個人的なリストなので、すべてを網羅しているわけではありません。例えば、自身はボイスセッションを中心に遊んでいるので、チャット機能を支援するModはあまり試していません。また今は「ダンジョンズ＆ドラゴンズ5e」をプレイしているので、ここに掲載しているModは同作以外では未検証です（そもそも5e以外に対応しないものも多いです）。

そもそもFVTTのModは栄枯盛衰が激しく、アップデートで突然使えなくなったり、コア機能に取り込まれ無用になったり、メンテナンスが止まって問題を含んだまま放置されたりすることがよくあります。なので、ここにある寸評もそう遠くないうちに時代遅れになるでしょう。さらに先にも書いたように、プレイスタイルによって必要な/重視する機能も異なるので、人によってはまったく役に立たない可能性もあります。そこを理解したうえで、一つの参考として活用して貰えたら幸いです。

なお、**初めてFVTTを触る人は、Modは使わずにまずコア機能を試してみてください。** そうでないとインストールしたModが何をしているのか分からなくなります。足りない機能を補うのは、それからでも遅くはありません。

更新：2021年3月31日 / FVTT v0.7.9

[TOC]

## 自動化関連

戦闘の自動化に関連したMod群。多くのModの連携とマクロによって実現されているものなので、うまく動作させるには個々のModが何をやっているのか理解する必要がある。全体的にやや上級者向き。

### About Time（翻訳済）
https://gitlab.com/tposney/about-time

ゲーム内での時間経過を管理するMod。アクティブ効果を時間切れにするのに必須。

### Active-Auras（翻訳済）
https://github.com/kandashi/Active-Auras

DAEと連携し、アクティブ効果をオーラ（コマを中心とした範囲に及ぼすAoE）として適用するMod。クルセイダーズ・マントルなどの呪文を表現するのに使用する。

### Active Token Lighting（未対応）
https://github.com/kandashi/Active-Token-Lighting

DAEと連携し、視界や明かりをアクティブ効果として管理するためのMod。

### Better Rolls for 5e（翻訳済）
https://github.com/RedReign/FoundryVTT-BetterRolls5e

ロール拡張。Midi-QOLと連携動作が可能で、個人的にチャットカードのデザインが好きなので使用しているが、自動化には必ずしも必要というわけではない。自動可しない場合はMidi-QOLをインストールせず、こちらだけ使う手もある。

### Combat Utility Belt（翻訳済）
https://github.com/death-save/combat-utility-belt

通称CUB。集中状態の管理や経験値の分配、NPC名の秘匿などさまざまな便利な機能を提供するMod。でも一番重要なのは状態管理を拡張するCondition Lab機能。DAEとも連携して、状態に即した効果をコマに適用できる。あと、戦闘中のイベントを管理する一時的戦闘参加者の機能も代替がなく便利。

### Dynamic Active Effects（翻訳済）
https://gitlab.com/tposney/dae

通称DAE。コア機能のアクティブ効果を拡張し、さまざまなBuffやDebuffを自動化する。Midi-QOLと合わせて自動化の要となる。だいたいなんでもできるが、使いこなすにはマクロやスクリプトの知識が必要かもしれない。

### Easy Target（未翻訳）
https://bitbucket.org/Fyorl/easy-target

Alt＋クリックでコマをターゲットできるようになるMod。Midi-QOLによる自動化と相性が良いのでここに含めた。ターゲットを支援するModには、ほかに「T is for Target」や「Target Enhancements」がある。

### Let Me Roll That For You!（翻訳済）
https://github.com/League-of-Foundry-Developers/fvtt-module-lmrtfy

プレイヤーに対しセーヴや能力判定などを求めるダイアログを表示するMod。単体でも動作するが、Midi-QOLと連携させることでセーヴを要する攻撃を自動化できる。ちなみに判定を即すだけならMonk's TokenBarのほうがお手軽。

### Midi-QOL（翻訳済）
https://gitlab.com/tposney/midi-qol

自動化の要。命中判定やダメージの処理を自動化し、戦闘をスピードアップさせる。ただし設定項目が多く、使いこなすのはけっこう大変。DAEのアクティブ効果をほかのコマに適用するのにも必須となる。

### Times Up（未翻訳）
https://gitlab.com/tposney/times-up

アクティブ効果の持続時間切れを管理するMod。「次のターンの開始時まで」といった特殊な持続時間にも対応する。About Timeとセットでインストールしておきたい。

### Item Macro（未翻訳）
https://github.com/Kekilla0/Item-Macro

アイテム自体にマクロを埋め込むMod。アイテムのロールをhookしてマクロを実行できる。のみならず、DAEと連携させることでハンターズ・マークなどの複雑なゲーム処理を自動化する。ただしマクロの理解が必須なので難度は高め。

## 戦闘関連

### Always HP（翻訳済）
https://github.com/ironmonk88/always-hp

選択したコマのhpを管理するウィンドウを表示させ、hpの増減や死亡フラグのON/OFFを手軽に行えるようにするMod。「Token Health」の代替に。

### Combat Carousel（翻訳済）
https://github.com/death-save/combat-carousel-public

標準のイニシアチブ表を代替するUIを提供するMod。とくに便利というわけではないが、見栄えは良くなる。お好みで。

### Combat Focus（未対応）
https://github.com/Oromis/foundry-combat-focus

右側のバー内でチャットとイニシアチブ表が同時に開けるようになる。

### Combat Numbers（翻訳済）
https://github.com/1000nettles/combat-numbers

ダメージをFF風にポップアップ表示する。

### Combat Tracker Effect Icon Tooltips（未対応）
https://github.com/schultzcole/FVTT-Combat-Tracker-Effects-Tooltips

イニシアチブ表に表示される状態アイコンにカーソルを合わせると、名称がポップアップするようになる。

### Drag Ruler（翻訳済）
https://github.com/manuelVo/foundryvtt-drag-ruler/tree/master

コマのドラッグ時に距離を表示し、コマの移動速度に応じて色分けするMod。「Show Drag Distance」の代替に。

### Next Up（未対応）
https://github.com/kandashi/Next-Up

ターンが回ってきたNPCのキャラクターシートを自動で開き、終わると自動で閉じてくれる。そのほかターン中のコマにマークをつけて協調したり、自動でコマにパンさせたりもできる。「Turn Marker」の代替に。

### Target Enhancements（翻訳済）
https://github.com/eadorin/target-enhancements

ターゲットしているコマに表示されるインジケータの見た目を変更するMod。Easy Targetとは違い、好みのデザインが選べる。「T is for Target」と同等の機能もある。

### Token Mold（翻訳済）
https://github.com/Moerill/token-mold#token-mold

コマをマップに配置するとき、コマの名前やリソースの表示方法などのトークン設定を上書きし、表示を統一したり、連番やプレフィックスを付けてユニークなとコマ名を自動生成したりできる。同種のMobを大量に出したりするときに便利。


## プレイ補助

ゲームプレイを便利にする、あるいはルールを補助するためのMod群。主にDM向け。

### Arms Reach（未対応）
https://github.com/psyny/FoundryVTT/tree/master/ArmsReach

コマのリーチ外のドアの開閉を制限する。

### Auto-Rotate（未翻訳）
https://github.com/Varriount/fvtt-autorotate

コマの動きに合わせて、コマの画像を回転させる。見下ろし型（top-down）のコマ画像を使っているなら必須レベル。同様のModに「About Face」があるが、「Multilevel Tokens」との相性が悪いようなのでこちらに変更。

### Conditional Visibility（翻訳済）
https://github.com/gludington/conditional-visibility

隠れ身や不可視、隠蔽、魔法の闇などの状態管理と、特殊な視覚によるコマの表示/非表示を自動化するMod。例えば〈隠密〉15で隠れ身をすると、受動〈知覚〉が15以上でないプレイヤーからは、そのコマが見えなくなる。DAE＆CUBとの連携も可能。


### Designer Doors（未対応）
https://github.com/Exitalterego/designerdoors

ドアのアイコンが変更できるようになる。

### DnD5e Helpers（翻訳済）
https://github.com/trioderegion/dnd5e-helpers

D&D5e向けの便利機能を提供するMod。遮蔽ボーナスの自動確認や、アンデッドなどが持つ特徴の再生やしぶとさの自動化、リアクションの管理やソーサラーの荒ぶる魔法の自動ロールなど。シチュエーションは限定されるが、うっかり忘れがちなルールを補助してくれる。

### Forien's Unidentified Items（翻訳済）

https://github.com/League-of-Foundry-Developers/foundryvtt-forien-unidentified-items

アイテムの情報を秘匿し、未鑑定な状態を作り出すMod。魔法の指輪をただの指輪に偽装したりなど。どの部分を隠し、どの部分を開示するかなども細かく設定できる。

### Journal to Canvas Slideshow（未対応）

https://github.com/EvanesceExotica/Journal-To-Canvas-Slideshow

資料の画像をクリックすると、指定したタイルにその画像が表示される紙芝居風Mod。とくにシーン制やマインドシアター系のゲームシステムと相性が良い。D&Dでもうまい使い方がないものか考え中。

### Mess - Moerills enhancing super-suit(e)（翻訳済）
https://github.com/Moerill/mess

さまざまな便利機能を提供するMod。ただしもうメンテナンスされておらず、現在はその機能のほとんどが、ほかのModで代替されている。残るはテンプレート内のコマを自動でターゲットする機能くらいなのだが……これが便利なので手放せない。とはいえ作者も非推奨を謳っているので、この機能だけどこか引き継いでくれないだろうか。

### Monk's Little Details（翻訳済）
https://github.com/ironmonk88/monks-little-details

痒いところに手が届く、さまざまな便利機能を提供する。戦闘のサポートから、各操作ツールへショートカットキーまで。コマを任意にテレポート移動させたり、倒したモンスターを血糊に変えたりといった機能もある。「Multiple Wall Point Mover」と「Combat Ready!」「Turn Marker」「GM Token-Drag Visibility」の機能は、概ねこれで代替できる。

### Monk's TokenBar（翻訳済）
https://github.com/ironmonk88/monks-tokenbar

PCのhpや特定のステータス（ACや受動〈知覚〉など）を一覧表示し、また能力値判定やセーヴ、対抗判定などをプレイヤーに要求したり、経験値を分配したりできる。「Loot Sheet NPC 5e」と連携すると、戦利品を分配したりも。

### Multilevel Tokens（翻訳済）
https://github.com/grandseiken/foundryvtt-multilevel-tokens

マップ上の特定の地点間をテレポートで移動させるMod。階段などで階層を移動するのに便利なほか、上の階から下の階が覗けるようなマップでも、下の階と上の階でコマの動きを同期させるといったことが可能になる。

### Perfect Vision（未対応）
https://github.com/dev7355608/perfect-vision

コアの光源処理を変更し、明かりと視覚をPHBの記載どおりにするMod。ほかDMのマップの視認性を改善したり、暗視をモノクロで表現したりなど。ただし処理がちょっと重い。「Less Fog」の機能をほぼ代替する。

### Pin Cushion（翻訳済）
https://github.com/death-save/pin-cushion

地点メモのアイコンに好きな画像が指定できるようになる。また地点メモにカーソルを置いたときに、内容をプレビュー表示する機能、プレイヤーが地点メモを作成できるようになる機能も。

### Quick Encounters（翻訳済）
https://github.com/spetzel2020/quick-encounters

モンスターやNPCを地点メモとして保存し、ワンクリックで展開できるようにするMod。コマの初期地位を保存しておけるので、一つのワールドで複数の卓をまわすときなどに便利。

### Token Attacher（翻訳済）
https://github.com/KayelGee/token-attacher

コマに対してほかのコマや呪文などの範囲テンプレート、タイルなどをくっつけられるようになるMod。くっつけられた付属物は、コマの移動に同期して移動するようになるので、いろんなことに応用できる。付属するマクロを使用することで「Mount Up!」および「Follow me!」の機能も代替可能。十全に活用するには「Select tool everywhere」も合わせてインストールすること。


## キャラクターシート拡張

キャラクターシートの機能を拡張するMod群。

### Drag'n'Transfer（未対応）
https://github.com/David-Zvekic/DragTransfer

キャラクターシートからキャラクターシートへアイテムをドラッグ＆ドロップしたとき、ドラッグ元のアイテムを自動削除するMod。通常の動作だと複製になってしまうので、受け渡す動作に変更したい場合にオススメ。

### Lazy Money（未対応）
https://github.com/DeVelox/lazymoney

お金の増減が楽になるMod。GPの欄で「-10」とか入力すると、足りなくてもPPから両替したうえで10ＧＰ減るように計算してくれる。


### Let's Trade 5e（未対応）
https://github.com/KageJittai/lets-trade-5e

プレイヤー同士でアイテムやゴールドの受け渡しを可能にするMod。同様のModに「Give item to another player」があるが、個人的にはこっちのほうが使いやすかった。

### Loot Sheet NPC 5e（未対応）
https://github.com/jopeek/fvtt-loot-sheet-npc-5e

宝箱や商人などのNPCを作成するためのキャラクターシート。敵が落としたアイテムをプレイヤーに拾わせ獲得ゴールドを分配したり、プレイヤーが自主的に購入できるベンダー機能が利用できるようになる。

### Magic Items（翻訳済）
https://gitlab.com/riccisi/foundryvtt-magic-items

呪文が込められたマジックアイテムを再現するMod。埋め込まれた呪文が、呪文書欄から使えるようになる。チャージが尽きると破壊されるようにも。ただし、ロールを拡張する自動化系のツールとは相性問題が発生しやすいようだ。

### Tidy5e Sheet（翻訳済）
https://github.com/sdenec/tidy5e-sheet

デザインと機能性にすぐれたキャラクターシートを提供する。好みの問題はあるが、5eをプレイするならぜひ入れておきたい。

## ビジュアルエフェクト関連

### Automated Animations（翻訳済）
https://github.com/otigon/automated-jb2a-animations

「JB2A」のアニメーションアセットを攻撃や呪文に適用するためのMod。JB2Aのほか「FXMaster」「Token Magic FX」が必要になる。

### FXMaster（翻訳済）
https://gitlab.com/mesfoliesludiques/foundryvtt-fxmaster

マップに雨だったり霧だったりといった環境効果/フィルターを適用するMod。

### JB2A - Jules&Ben's Animated Assets
https://github.com/Jules-Bens-Aa/JB2A_DnD5e

呪文や攻撃のエフェクトを派手にするアニメーションアセット集。「Automated Animations」と組み合わせて攻撃に使用したり、「FX Master」でマップに配置したりするできる。なお有料版（Patreon）の「JB2A - Patreon Complete Collection」もあるので、バリエーションが欲しいときは購入を検討してみよう。

### Theatre Inserts（翻訳済）
https://github.com/League-of-Foundry-Developers/fvtt-module-theatre

アドベンチャーゲーム風の立ち絵Mod。凝ったフォントでナレーションを流すことも可能。シーン演出に使おう。

### Token Magic FX（翻訳済）
https://github.com/Feu-Secret/Tokenmagic

コマにさまざまなエフェクトをかけられるようになるMod。またテンプレートに自動でテクスチャを貼り付けるなど、「MESS」の機能をほぼ代替する。DAEとの連携も可能。

## サウンド関連

### Ambient Doors（未対応）
https://github.com/EndlesNights/ambientdoors

ドアを開閉したときのサウンドをカスタマイズするMod。

### Bellows（翻訳済）
https://github.com/casualchameleon/Bellows

プレイリストにYouTubeのアドレスが登録できるようになるMod。ストリーム再生になるので、ストレージを圧迫しないという利点もある。便利。

### Better Ambient Loop by Blitz（未対応）
https://github.com/BlitzKraig/fvtt-BetterAmbientLoop

環境音のループを調整するMod。音が途中で切れていてうまくループしないときに、うまくつないでくれる。

## チャット機能拡張

### Cautious Gamemaster's Pack（翻訳済）

https://github.com/ShoyuVanilla/FoundryVTT-CGMP

DMが誤ってPCとして話すことの制限、非表示コマの発言の抑止、チャット入力欄での上/下方向キーの挙動変化、チャット入力中であることの通知機能など。

### Illandril's Chat Enhancements（未翻訳）

https://github.com/illandril/FoundryVTT-chat-enhancements

チャット欄の表示を拡張する。PC名にプレイヤー名を添えて表示したり、アクター名をトークン名で置き換えたり、名前クリックでコマを選択したりなど。

### Polyglot（翻訳済）

https://github.com/League-of-Foundry-Developers/fvtt-module-polyglot

PCが修得している言語によって、表示される文字をスクランブルする。つまりエルフ語の発言はエルフ語を修得していないと判別できなくなる。資料の文字も同様に、言語によるスクランブルをかけられる。言語ごとに固有のフォントが用意されているので、雰囲気的にも悪くない。


## UI関連

インタフェースの見た目を変えたり、便利にしたりするMod群。

### Compendium Folders（翻訳済）
https://github.com/earlSt1/vtt-compendium-folders

辞典のフォルダ分けを便利にするMod。とても便利。

### Custom Css（未翻訳）
https://github.com/cswendrowski/FoundryVTT-Custom-CSS

CSSに変更を加えることで、画面内のあらゆるデザイン面をカスタマイズできるようになる。CSSの知識が必要だが、個人的には必須レベル。

### Custom Nameplates（未対応）
https://github.com/earlSt1/vtt-custom-nameplates

コマに表示される名前のフォントファミリーやフォントサイズを変更できる。「Forien's Custom Fonts」と合わせて使用するとよい。

### DF Settings Clarity（未翻訳）
https://github.com/flamewave000/dragonflagon-fvtt/tree/master/df-settings-clarity

Modの各設定が、GM専用（グローバル）なのかプレイヤーごと（ローカル）なのかを識別できるようになる。設定項目についてプレイヤーに説明するときに便利。

### DF Curvy Walls（翻訳済）
https://github.com/flamewave000/dragonflagon-fvtt/tree/master/df-curvy-walls

ウォールを描くときにベジェ曲線が使えるようになる。

### Dice So Nice!（翻訳済）
https://gitlab.com/riccisi/foundryvtt-dice-so-nice

ロールを行ったとき、3Dダイスが表示されるようになる。TRPGやってる感が出るのでオススメ。

### Dice Tray（未翻訳）
https://gitlab.com/asacolips-projects/foundry-mods/foundry-vtt-dice-calculator

ダイス式の入力を補助するMod。ダイスを手動で振るときに便利。

### Expander（未対応）
https://github.com/Sky-Captain-13/foundry/tree/master/expander

右側のチャットバーを縮小したとき、縮小解除ボタンのみならず、チャットボタンでも縮小を開場できるようになる。地味だが便利。

### Forien's Custom Fonts（未翻訳）
https://github.com/Forien/foundryvtt-forien-custom-fonts

Google Fontsから指定したフォントを読み込み、FVTT上で利用可能にしてくれる。とりあえず**Noto Sans JP**と**NotoSansJPBold**を設定しておけば鉄板だろう。個人的には必須レベル。

### Forien's Copy Environment（未翻訳）
https://github.com/League-of-Foundry-Developers/foundryvtt-forien-copy-environment

コア機能やModの設定をエクスポートするMod。別のワールドに環境を移すときなどに便利かも。

### FPS Meter（未対応）
https://github.com/ardittristan/VTTFPSMeter

キャンバスの右上にフレームレートが表示されるようになる。なんか重いと言われたときの確認やサポートに。

### Illandril's Token HUD scaler（未翻訳）
https://github.com/illandril/FoundryVTT-token-hud-scale

コマを右クリックして表示される状態管理のアイコンサイズを変更する。便利。

### Macro Folders（翻訳済）
https://github.com/earlSt1/vtt-macro-folders

マクロのフォルダ分けが便利になるMod。

### Permission Viewer（未対応）
https://github.com/League-of-Foundry-Developers/fvtt-module-permission-viewer

アイテムの権限を可視化して表示する。

### Pings（翻訳済）
https://gitlab.com/foundry-azzurite/pings/-/blob/master/README.md

マップ内の特定の位置にマークを表示し、プレイヤーに注目を即す機能を追加する。DM/プレイヤー間の意思疎通に大変便利。

### PopOut!（翻訳済）
https://github.com/kakaroto/fvtt-module-popout

キャラクターシートなどを、現在のブラウザの外もしくは新しいタブで開けるようになる。

### Rounded Distance for Measured Templates（未対応）
https://github.com/cdverrett94/rounded-distance-for-measured-templates

テンプレート操作ツールで配置する効果範囲のサイズが、指定した値で丸めらるようになる。つまり15ft.円錐や20ft.球形といった、切りのいいサイズの効果範囲がフリーハンドで描ける。便利。

### Scene Clicker（未対応）
https://github.com/jegasus/scene-clicker

シーンをクリックしたとき、通常は設定が開くところを「シーンを表示」に置き換える。

### Select tool everywhere（未翻訳）
https://github.com/KayelGee/select-tool-everywhere

すべてのツールに選択ボタンを追加する。「Token Attacher」を使用する場合は必須。

### Show Notes（未対応）
https://github.com/shawndibble/foundryvtt-show-notes

プレイヤーが地点メモを表示する設定にしていなくても、DM側で表示/非表示を決められるようになる。

### Tidy UI - Game Settings（未翻訳）
https://github.com/sdenec/tidy-ui_game-settings

設定画面が見やすくなる。

### TinyMCE Custom Foundry File Picker（未翻訳）
https://github.com/cswendrowski/FoundryVTT-TinyMCE-CustomFilePicker

資料に画像を貼るときに、アドレスを手書きするのでなくファイルピッカーから選択できるようになる。

### TinyMCE toolbar config（未対応）
https://github.com/superseva/mce-config

テキスト編集時に表示するツールバーをカスタマイズする。ボタンを足したり減らしたり。設定はmce-config.jsを直接編集する必要がある。

### Token Tooltip Alt（翻訳済）
https://github.com/bmarian/token-tooltip-alt

コマにカーソルを合わせたときに、設定した情報をポップアップ表示する。似たようなModは沢山あるが、これが一番柔軟で、完成度が高いと思う。ただ細かくカスタマイズできるぶん、設定項目がやや複雑なのが難点か。

### Wallter（未対応）
https://github.com/mtvjr/wallter

ウォールのアンカー位置をキーボードのカーソルキーで操作できるようになる。微調整に便利。

### Your Tokens Visible（翻訳済）
https://github.com/David-Zvekic/TokensVisible

一つのセルにコマが重なったとき、後ろのコマを手前に移動させる機能を提供する。「Cycle Token Stack」の代替。のみならず、コマが壁を越えて移動するときのアニメーションをスキップしたり、コマが画面端に近付いたときにパンさせたりする機能もあり、「GM Token-Drag Visibility」の代替にもなる。

### Zoom/Pan Options（未対応）
https://github.com/itamarcu/ZoomPanOptions

マウスホイールで画面を拡縮するときに、画面中央でなくカーソルの位置を中心として拡縮できるようになる。ほか拡縮のスピード変更や、タッチパッド操作への最適化などの機能も。

### zSync（未対応）
https://github.com/Sk1mble/zsync

一つのセルにコマが重なったとき、どちらが上に表示されるかはクライアント毎に決定されるが、このModを入れるとそれが同期されるようになる。つまりGMが順序を入れ替えたら、ほかのプレイヤーの画面でも同じように入れ替わる。「Your Tokens Visible」と合わせて使うと便利。


## ライブラリ系

さまざまなModにライブラリとして共通の機能/インタフェースを提供するMod群。

### CodeMirror（未翻訳）
https://github.com/League-of-Foundry-Developers/codemirror-lib

コード用のエディタ機能を提供する。

### The Furnace（未翻訳）
https://github.com/kakaroto/fvtt-module-furnace

高度なマクロが使用できるようになるMod。またデバッグモードを有効にすると、動作時にどのHookが呼び出されたのかをコンソールから確認できるようになる。

### lib - Color Settings（未翻訳）
https://github.com/ardittristan/VTTColorSettings

カラーピッカー機能を提供する。

### Library: DF Hotkeys（翻訳済）
https://github.com/flamewave000/dragonflagon-fvtt/tree/master/lib-df-hotkeys

ショートカットキー機能を提供する。

### libWrapper（未対応）
https://github.com/ruipin/fvtt-lib-wrapper

Modの競合管理機能を提供する。

### Math.js（未対応）
https://github.com/League-of-Foundry-Developers/mathjs-lib

計算機能を提供する。

### SortableJS（未対応）
https://github.com/League-of-Foundry-Developers/sortablejs-lib

並び替え機能を提供する。


## そのほか

### Actor Attribute Lists（未翻訳）
https://github.com/relick/FoundryVTT-Actor-Attribute-Lists

選択コマのアクターが持つ変数を一覧表示する。DAEのお供に。

### Dynamic Active Effects SRD
https://github.com/kandashi/Dynamic-Effects-SRD

DAEに対応したSRD辞典を提供するMod。ただしすべて英語。DAEの使い方を勉強する教材として活用できる。

### Foundry Community Macros
https://github.com/foundry-vtt-community/macros

海外のコミュニティで作られた便利なマクロ集。ただし全部英語なので、マクロを自作する参考にするのがいいかもしれない。

### Game-icons.net
https://github.com/datdamnzotz/icons/blob/master/README-FoundryVTT.md

膨大なアイコンを提供するアセット集。とても便利。


## 有用だけど今は使ってないもの

### 5e-Sheet Resources Plus（未翻訳）
https://github.com/ardittristan/5eSheet-resourcesPlus

キャラクターシートのリソース欄を増やす。今のところ足りてるけど、不足したら有効にしたい。

### Adventure Importer / Exporter（未翻訳）
https://github.com/cstadther/adventure-import-export

現在のワールドのデータをまるごとエクスポートできる。配布用にはいいかもしれない。

### Always Centred（未対応）
https://github.com/SDoehren/always-centred

選択中のキャラクターが、常に画面の中央に位置するようにカメラが動くようになる。コンピュータRPGみたいで楽しいけど、複数のコマを動かすDMにとってはちょっとうるさい感がある。

### Autocomplete Whisper（未対応）
https://github.com/orcnog/autocomplete-whisper

秘話を送るときに、送り先自動補完してくれる。よく考えたら秘話とか使ってなかった。

### Babele（未翻訳）
https://gitlab.com/riccisi/foundryvtt-babele

辞典の翻訳を補助するMod。DnD5e環境では必須ではないが、そのうち使ってみたい。

### Calendar/Weather（翻訳済）
https://github.com/DasSauerkraut/calendar-weather

「About Time」と連携し、ゲーム内時間の経過を反映したカレンダーを表示する。また天候をランダム生成したり、月齢や祝祭日を細かに設定することも可能。今プレイしているキャンペーン（アヴェルヌス）ではあまり必要なかった。

### Chat Images（未翻訳）
https://github.com/bmarian/chat-images

チャットに画像を貼り付けられるようになる。でも貼り付けたいならDiscordでもいい気がする。

### Combat Enhancements（翻訳済）
https://gitlab.com/asacolips-projects/foundry-mods/combat-enhancements

イニシアチブ表にhpバーを表示したり、イニシアチブ順を入れ替えたりなど、便利な機能を追加できる。「Combat Carousel」に乗り換えたので、今は使ってない。

### CommunityLighting by Blitz（未翻訳）
https://github.com/BlitzKraig/fvtt-CommunityLighting

光源のエフェクト（明滅など）を増やすMod。面白いけど、そんなに沢山は必要なかった。

### Cursor Hider（未翻訳）
https://gitlab.com/foundry-azzurite/cursor-hider

DMやプレイヤーのマウスカーソルを一時的に隠せるようになる。モンスターを配置するときなどに、マウスの位置でバレないようにしたりできて便利。だけどあまり使う機会はなかった。

### Easy Exports（未翻訳）
https://github.com/spetzel2020/easy-exports

アイテムをフォルダごとエクスポートできる。ただあまり使う機会がなく、使うとしてもそのときだけ有効にすれば十分だと思う。

### Give item to another player（未対応）
https://github.com/Sepichat/FoundryVTT-GiveItem

プレイヤー間でアイテムの受け渡しできるようになる。「Let's Trade 5e」でいいと思ったけど、これは好みの問題だと思う。

### Macro Marker（未対応）
https://github.com/League-of-Foundry-Developers/foundry-macro-marker/

マクロホットバーに登録したマクロに、オン/オフ切り替えなどのさまざまな効果を付けられる。ただちょっと使い方が難しい。自動化が進み、オン/オフを可視化する必要がなくなってきたので、今は使ってない。

### MEME - Moerills Expandable Markdown Editor（未対応）
https://github.com/Moerill/fvtt-markdown-editor

標準のTinyMCEを置き換え、アイテム説明や資料にMarkdown記法を使えるようになる。便利だが、相性問題が発生しがちだったので断念。

### Parallaxia（未対応）
https://gitlab.com/reichler/parallaxia

タイルを組み合わせて、アニメーションシーンの作成を可能にする。今は出番がないが、いずれカーチェイスをやるときに使いたい。

### Party Resources（翻訳済）
https://github.com/davelens/fvtt-party-resources

パーティの共有リソースを管理するためのMod。アドベンチャー次第では間違いなく便利だが、今のところ必要がなかった。

### Potato Or Not（未翻訳）
https://github.com/Haxxer/FoundryVTT-PotatoOrNot

PCスペックから最適な設定をサジェストしてくれるMod。FVTTでのオンラインセッションに初めて参加する人にはよいと思う。

### Reset Movement（未対応）
https://github.com/jessev14/Reset-Movement

ターン中の移動をリセットし、ターン開始時の位置に戻すボタンを追加する。便利だけど「Monk's Little Details」や「Next Up」のマーカー機能で代用できる。でも面白い。

### Roofs ＆ Overhead Tiles（未翻訳）
https://github.com/League-of-Foundry-Developers/roofs

コンピュータRPGのように、外から見ると屋根があるのに、中に入ると屋根が消えるという建物が作れるようになる。ただアセットの準備が面倒で、かつウォールの配置が煩雑になってしまう。FVTT0.8.xでコア機能にと入れられるそうなので、そのときにまた考えたい。

### Terrain Layer（未翻訳）
https://github.com/wsaunders1014/TerrainLayer

移動困難地形をマップ上に設定できるようになる。「Terrain Ruler」や「Drag Ruler」との組み合わせで、移動困難地形を自動処理できるようになるけど、あんまり有用性が感じられなかった。

### Terrain Ruler（未翻訳）
https://github.com/manuelVo/foundryvtt-terrain-ruler/tree/master

標準の距離測定機能を上書きし、移動困難地形による距離の増加を測定に反映させる。上記のとおり、使い道があまり思いつかない。

### Token Action HUD（翻訳済）
https://github.com/espositos/fvtt-tokenactionhud

選択中のコマが実行可能なアクションをすばやく選択できる簡易メニューを表示するMod。とても便利ではあるものの、実際使ってみると、自分はあまり使っていないことに気付いた。あと表示位置的に、「Combat Carousel」と相性が良くないようにも感じる。

### Token Animation Tools（未対応）
https://github.com/ruipin/fvtt-token-animation-tools

GMがコマをドラッグして動かすとき、移動をアニメーションさせずテレポートさせる。移動距離に応じて動作を変えるなど細かな設定が可能だが、「Monk's Little Details」と「Your Token Visible」で代替できるので外すことにした。

### Popout Resizer（未対応）
https://github.com/Cardagon/popout-resizer

右側のメニューからポップアウトさせて開いたウィンドがリサイズできるようになる。これもあまり使う機会がなかった。

### Simplefog - Manual Fog of War（翻訳済）
https://github.com/League-of-Foundry-Developers/simplefog

FVTTの光源処理を使わずに、プレイヤーの可視範囲をDMが手動で指定できるようになる。今はあまり使う機会がないが、「魂を喰らう墓」みたいなヘクスクロールものには便利そうではある。

### SoundBoard by Blitz（未翻訳）
https://github.com/BlitzKraig/fvtt-SoundBoard

専用のメニューから、さまざまなサウンドエフェクトを鳴らすことができるMod。悪くはないのだが、日本語環境だとちょっと使いにくい。

### Grid Scale Menu（未対応）
https://github.com/UberV/scaleGrid/

マップ画像とFVTTのグリッドをうまくマッチさせるのを助けるMod。標準機能よりも便利だが、Dungeon Draftなどで作られた最近のアセットは、そもそもオンラインセッション用に作られているので、グリッドが合わせにくいということは少ない気がする。

## 以前使っていたが引退したもの

### Better Target（未対応）
https://github.com/sPOiDar/fvtt-module-better-target

ターゲットのマーカーを見やすくする。「Target Enhancements」で代替。

### Combat Ready!（翻訳済）
https://github.com/smilligan93/combatready

ターンが回ってきたことをプレイヤーに知らせる。「Monk's Little Details」で代替。

### Condition Automation（未翻訳）
https://github.com/kandashi/condition-automation

状態にあわせて移動速度や視覚などを変更するModだった。CUBとアクティブ効果におおむね代替され、有用性が減ったので引退。

### Conditions for D&amp;D 5e（未翻訳）
https://github.com/trdischat/conditions5e

残りhpに応じてコマにアイコンを表示する。CUBとアクティブ効果で代替。

### Cone Measurement Angle（未対応）
https://github.com/thre-z/cone-measurement-angle

円錐テンプレートの角度を変更する。ワールドスクリプトで代替。
### Cycle Token Stack（未翻訳）
https://github.com/aka-beer-buddy/fvtt-cycle-token-stack

重なったコマの順序を入れ替える。「Your Tokens Visible」で代替。

### Follow Me!（未対応）
https://github.com/Brunhine/FollowMe

指定したコマのあとを連結したコマがついていくようにする。「Token Attacher」で代替。

### GM Token-Drag Visibility（未対応）

https://github.com/SteffanPoulsen/token-drag-visibility

DMが視覚能力を有効にしたコマをドラッグで動かすときに、一時的に自動で視覚能力を無効化する。便利だが「Perfect Vision」があればそんなに困らないので今は使ってない。

### Hide GM Rolls（未翻訳）
https://github.com/sPOiDar/fvtt-module-hide-gm-rolls

DMのロールを秘匿する。Midi-QOLに同等の機能がある。

### Less Fog（未対応）
https://github.com/trdischat/lessfog

視線が通らず見えない範囲の視認性を改選する。「Perfect Vision」で代替。

### Otigons Animation Macros
https://github.com/otigon/otigons-animation-macros

JB2Aのビジュアルエフェクトをマクロを介して適用する。「Automated Animations」で代替。

### Multiple Wall Point Mover (MWPM)（未対応）
https://github.com/Reaver01/mwpm

つながった壁をまとめて移動できるようにする。「Monk's Little Details」で代替。

### Narrator Tools（翻訳済）
https://github.com/elizeuangelo/fvtt-module-narrator-tools

DM発言をナレーションとして画面に出してくれるMod。「Theater Insert」にも似た機能がある。

### Token Auras（未翻訳）
https://bitbucket.org/Fyorl/token-auras

コマを中心とした指定範囲に色を付ける。「Token Attacher」と「Active Aura」で代替。

### Token Health（未翻訳）
https://github.com/tonifisler/foundry-token-health

コマを選択してエンターキーを押すと、hpを増減させるダイアログを表示する。「Always HP」で代替したが、個人的にはこっちのほうが便利な気も。ただ長らくメンテナンスされておらず、コンソールログにエラーが残りまくるので、見切りをつけた。

### Token Vision Tweaks（未対応）
https://github.com/ruipin/fvtt-token-vision-tweaks

視界に関する処理の厳密性や最大視界などをできる。「Perfect Vision」「Your Token Visible」にも似た機能があるので、今は使ってない。

### Torch（未翻訳）
https://github.com/RealDeuce/torch/

コマのHUDに、たいまつかライトを使用して明かりを付けるボタンを追加する。「Active Token Lighting」で代替。

### Turn Marker（翻訳済）
https://github.com/kckaiwei/TurnMarker-alt

ターン中のコマと、そのコマのターン開始位置にマーカーを表示する。「Next Up」および「Monk's Little Details」で代替。

## 試したけど不採用だったもの

### 5e Encounter Builder（未翻訳）
https://github.com/RaySSharma/fvtt-encounter-builder

遭遇難易度を自動計算するMod。「Monk's Little Details」でもできるほか、この機能自体もあまり使わなかった。

### Character Actions List dnd5e（未翻訳）
https://github.com/ElfFriend-DnD/foundryvtt-dnd5eCharacterActions

キャラクターシートにD&D Beyond風のアクションリストを表示する。悪くはないけど、「Tidy5e Sheet」のお気に入り機能で十分だと思った。

### Dynamic Illumination（未翻訳）
https://github.com/delVhariant/illumination

ダイナミックイルミネーションと昼/夜の段階をカスタマイズする。長らくメンテナンスされておらず、不具合も多いようなので断念。

### Forien's Quest Log（翻訳済）
https://github.com/League-of-Foundry-Developers/foundryvtt-forien-quest-log

コンピュータRPG風のクエストログを表示する。面白いと思うが、実際使ってみると煩雑に感じた。

### Health Estimate（未翻訳）
https://github.com/Shylight/healthEstimate/

残りhpに応じて、コマの上部に大まかな指標（瀕死など）を表示するMod。細かく分けるならhpバーを表示すればいいし、そうでないならCUBや「Token Tooltip Alt」でいいと思った。

### I See You 5e（未対応）
https://github.com/herasrobert/icu5e

隠れ身と〈知覚〉判定を高度に自動化する。あまりメンテナンスされておらず、「Conditional Visibility」でも代用できるので断念。

### Party Overview（未翻訳）
https://github.com/League-of-Foundry-Developers/party-overview

パーティのhp、AC、受動〈知覚〉、修得言語、所持金などを一覧表示する。便利だがとは思うが、あまり使わなかった。

### Pick-Up-Stix（未対応）
https://github.com/kamcknig/pick-up-stix

アイテムを地面に置いたり、戦利品用の宝箱を作成できるMod。コンセプトはとてもいいのだが、どうにも不安定だったので断念。

### Ruler-from-Token（未翻訳）
https://github.com/Nordiii/rulerfromtoken

Ctrlキー1つで距離測定が行える。Ctrlキーはよく使うキーなので、暴発が多くて個人的にはイマイチだった。

### Show Art（未翻訳）
https://github.com/zeel01/TokenHUDArtButton

コマのHUDにポートレートやコマ画像を表示するボタンを追加し、プレイヤーに見せることができる。「Tidy5e Sheet」の画像ボタンでいいかなと。

### Show Drag Distance（未翻訳）
https://github.com/wsaunders1014/ShowDragDistance

「Ruler-from-Token」とほぼ同じ。やっぱり暴発が多くてイマイチに感じた。

### Spell Template Manager - for DnD5e（未翻訳）
https://github.com/bitkiller0/spellTemplateManager

呪文から配置されたテンプレートを、持続時間によって自動削除するMod。便利だと思ったが、実際にはあまり使わなかった。

### Status Icon Counters（未翻訳）
https://gitlab.com/woodentavern/status-icon-counters

状態アイコンに数字を表示して、増減させたりカウントダウンしたりできるMod。何かに使えるかなと思ったけど、実際は使わなかった。

### Tweak Playlist（未対応）
https://github.com/trdischat/tweakplaylist

プレイリストの機能を強化する。悪くないのだが、「Bellows」と競合するので断念。