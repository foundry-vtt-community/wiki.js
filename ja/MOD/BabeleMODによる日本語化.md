---
title: Babele MODによる日本語化
description: Babele MODを使用した辞典の日本語化の解説です。
published: false
date: 2022-10-14T15:07:47.880Z
tags: 
editor: markdown
dateCreated: 2022-10-14T15:03:47.104Z
---

# Babele MODによる日本語化

最終更新：2022/10/15

このページでは [Babele](https://gitlab.com/riccisi/foundryvtt-babele) MOD（以下Babele）を使用した辞典の日本語化について解説します。
Babeleを使用するとことで辞典を日本語化するための翻訳ファイルの生成と日本語に書き換えた翻訳ファイルを辞典へ反映させることができます。
下記の内容が間違っていたり分かりにくい場合は、オンセ工房Discordサーバーまで連絡をお願いします。

## 基本的な使い方

まず日本語化したい辞典があるゲームシステムのワールドを作成しBabeleを有効にします。
日本語化したい辞典を開きウィンドウ上部にある翻訳ボタンを押すと翻訳ファイル抽出というダイアログが表示されるので、従来か共同翻訳のどちらかを選んで出力ボタンを押すと翻訳ファイルが生成されます。
（共同翻訳を選ぶと複数人で編集するようなWebサービスで利用できる形式で出力されます。自分だけで編集する場合はどちらで出力しても大差ありません。）
このページでは従来形式で出力した場合で解説を行います。

![babele_01.png](/images/japanese-community/shoki/babele_01.png)

翻訳ファイルはzip圧縮で生成されるので解凍し、中のjsonファイルをテキストエディタやjsonファイルを扱えるWebサービスなどで開きます。
jsonファイルには`"label"`、`"id"`、`"name"`、`"description"`の各要素とそれに対応したデータが右側に記述してあります。
`"id"`、`"name"`、`"description"`は`"entries"`の中に辞典の各アイテムごとに並んでいます。
- `"label"`：辞典の名前、変更可能
- `"id"`：辞典原文での名前、紐づけに使用するので変更不可
- `"name"`：アイテムの名前、変更可能
- `"description"`：アイテムの説明文、変更可能

Pathfinder1eシステムのRace辞典の翻訳ファイルの例
```
{
	"label": "Races",
	"entries": [
		{
			"id": "Aasimar",
			"name": "Aasimar",
			"description": "<h2>Starting Age</h2>\n<table style=\"border-spacing: 0px; background-color: #ffffff; margin: 10px; table-layout: fixed; border-color: #888888; min-width: 200px; color: #333333; font-family: Varela, sans-serif;\" border=\"1\" cellpadding=\"5\">\n<thead style=\"box-sizing: border-box; background-color: #cfe2f3;\">\n<tr style=\"box-sizing: border-box;\">\n<th class=\"text\" style=\"box-sizing: border-box; padding: 2px; text-align: left; border: 1px solid #888888;\">Adulthood</th> 以下省略",
		},  
```
上の例を日本語に書き換えると次のようになります。
```
{
	"label": "種族",
	"entries": [
		{
			"id": "Aasimar",
			"name": "アアシマール",
			"description": "<h2>アアシマールの種族特性</h2><ul><li><strong>＋2【判断力】、＋2【魅力】：</strong>アアシマールは洞察力があり、自信に満ち、魅力的だ。</li><li><strong>現住の来訪者：</strong>アアシマールは（原住）の副種別を持つ来訪者である。</li><li><strong>中型：</strong>アアシマールは中型クリーチャーであり、サイズによるボーナスもペナルティも受けない。</li><li><strong>普通の移動速度：</strong>アアシマールは30フィートの基本移動速度を持つ。</li><li><strong>暗視：</strong>アアシマールは最大60フィートまでの暗闇を見通せる。</li><li><strong>得意技能：</strong>アアシマールは〈交渉〉および〈知覚〉判定に＋2の種族ボーナスを得る。</li><li><strong>擬似呪文能力：</strong>アアシマールは1日1回@Compendium[pf1.spells.7x2z0i8rcx7s81fk]{デイライト}を擬似呪文能力として使用することができる（術者レベルはアアシマールのレベルと等しい）。</li><li><strong>天上の抵抗：</strong>アアシマールは［強酸］、［雷撃］、［氷雪］に対する抵抗5を持つ。</li><li><strong>言語：</strong>アアシマールはプレイ開始時に共通語及び天上語の会話能力を持っている。高い【知力】を持つアアシマールは次に挙げる言語を選択することができる：エルフ語、ドワーフ語、ノーム語、ハーフリング語、森語及び竜語。</li></ul>以下省略",
		},
```
`"description"`ではhtmlタグを使用することができます。ただし `"` をそのまま書くとBabeleで読み込む際にエラーが出るので `\"` と記述してください。

書き換えた翻訳ファイルをFVTTで有効にするには、モジュール設定のBabaleの項目の**翻訳ファイルディレクトリ**を設定する必要があります。
翻訳ファイルディレクトリに設定したフォルダ（画像ではtranslationフォルダとしています）の中に**jaフォルダ**を作り、その中に書き換えた翻訳ファイルを置くことで辞典を日本語化することが出来ます。
![babele_02.png](/images/japanese-community/shoki/babele_02.png)

## Mappingについて

`"name"`、`"description"` 以外のデータを書き換える方法としてBabeleではMappingが用意されています。

まず書き換えたい項目を辞典である**dbファイル**のどこに格納されているかを調べる必要があります。
ゲームシステムがインストールされたフォルダから目的のdbファイルを探しテキストエディタで開くと、アイテム1つにつき1行で構成されているので1行全てコピーして適当なテキストファイルに貼り付けます。
`{ } ,` に従って整形するとデータの構造が分かりやすくなり探しやすくなります。

参考例
```
{
	"_id":"FHY3oIk5zSOFncJH",
	"name":"Aasimar",
	"type":"race",
	"img":"systems/pf1/icons/races/aasimar.png",
	"data":{
		"description":{
			"value":"<h2>Starting Age</h2>\n<table style=\"border-spacing: 0px; background-color: #ffffff; 省略 (they can see perfectly in the dark up to 60 feet.)</li>\n</ul>"
		},
		"tags":[],
		"changes":[
			{
				"_id":"9znnwx5o",
				"formula":"2",
				"operator":"add",
				"subTarget":"wis",
				"modifier":"racial",
				"priority":0,
				"value":0,
				"target":"ability"
			},{
				"_id":"78f2uhd2",
				"formula":"2",
				"operator":"add",
				"subTarget":"cha",
				"modifier":"racial",
				"priority":0,
				"value":0,
				"target":"ability"
			},{
				"_id":"z8dsccn8",
				"formula":"2",
				"operator":"add",
				"subTarget":"skill.dip",
				"modifier":"racial",
				"priority":0,
				"value":0,
				"target":"skill"
			},{
				"_id":"9nqnt98u",
				"formula":"2",
				"operator":"add",
				"subTarget":"skill.per",
				"modifier":"racial",
				"priority":0,
				"value":0,
				"target":"skill"
			}
		],
		"contextNotes":[],
		"links":{
			"children":[]
		},
		"armorProf":{
			"value":[]
		},
		"weaponProf":{
			"value":[]
		},
		"languages":{
			"value":["common","celestial"]
		},
		"flags":{
			"boolean":[],
			"dictionary":[]
		},
		"scriptCalls":[],
		"creatureType":"outsider",
		"subTypes":[["Native"]]
	},
	"effects":[],
	"folder":null,
	"sort":0,
	"permission":{"default":0},
	"flags":{}
}
```
この例では `"description"` は `"data"` の中に格納されていますが、本文は`"description"`の中の `"value"` に記述されていることが分かります。
実際にBabeleのソースコードでは`"description": "data.description.value"`と書かれていますので、これを参考にMappingを行います。
例えば `"subTypes"` を書き換えたい場合は `"data"` の中にありますので`"subTypes": "data.subTypes"` となります。

これを実際に翻訳ファイルに書くと次のようになります。"subTypes"はdbファイルでは `[[]]` で括られていたのでそれも合わせます。
```
{
	"label": "種族",
    "mapping": {
			"subTypes": "data.subTypes"
    },
	"entries": [
		{
			"id": "Aasimar", 
			"name": "アアシマール",
			"description": "省略",
			"subTypes": [["原住"]]
		},
```
Mappingで追加した項目を各アイテムに追記する時は行末に `,` を入れて区切るのを忘れるとエラーになります。最後の項目に `,` は入れません。
手動で各アイテムに項目を追加するのが面倒な場合は、BabeleのソースコードにMappingの内容を追記してから翻訳ファイルを生成すると各アイテムに項目が追加されますので試してみてください。