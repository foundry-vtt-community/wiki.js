---
title: 聊天
description: 
published: true
date: 2021-05-07T01:53:34.362Z
tags: 
editor: markdown
dateCreated: 2021-04-21T14:51:15.058Z
---

## 聊天，Macro 及 格式指南

### 聊天命令

輸入信息時，用戶可以在命令前添加信息前綴。 下面列出了一些與發送聊天信息有關的命令：

### 角色發言

句型: `/ic {message}`

使信息由關聯角色說出。 如果已選擇角色的Token（或通過“玩家配置”視窗選擇說話者）後，玩家將自動以角色說話，而無需為每條信息輸入此命令。

### 超遊

句型: `/ooc {message}`

使信息以超遊（OOC）角度發出。 OOC信息將以玩家的顏色進行輸出，以使其更易於識別。沒有指定說話者或選定TOKEN的玩家將自動(OOC)。

### 動作

句型: `/emote {message}` or `/em {message}` , `/me {message}`

使信息成為所選角色執行的表情動作。 表情是玩家通過文本傳達的角色內動作，因此要求玩家已選擇Token（或通過“玩家配置”鏈接角色）。 輸入`/ emote 揮動他的手`。同時一個叫Simon的角色將發送信息：`Simon揮動他的手。`

### 私訊

句型: `/whisper {target} {message}` or `/w {target} {message}` or `@{target} {message}`

對目標低語。非GM玩家需要獲得“Whisper Private Messages私訊”權限才能發送私訊；如果這樣，GM / Assistant將看不到這些信息。

要將信息發送給姓名中帶有空格的玩家，請將該姓名括在方括號中：`/ w [Arthur Dent]毛巾在哪裡？`

請注意，您可以通過在方括號中用逗號分隔的形式，同時向多個用戶發送信息。 例如`/w [Andrew, Tim, Julia] 你覺得怎樣？` 或 `@[James,Alicia]我們應該進攻還是偷偷溜走？`
最後，用`GM`或`players`可以分別私訊所有GM用戶或所有非GM用戶。

## 什麼是macro?

Macro是一種讓玩家快捷地去進行某些動作的方式。它可以是MOD命令，私訊或格式化的文本。

您可以在[此處](https://foundryvtt.com/article/macros/) 上了解有關Macros的更多信息。

## 擲骰

基本上，骰子的工作方式如下：

    /r {數量}d{面數} + {增減}}

例子:

    /r 2d10 + 5

哪個發送到聊天：

![示範](https://paper-attachments.dropbox.com/s_18A9487B0EE61A81F393FA91A13C89DE192362383B6581DB8EF3234F6BDD2D71_1587942384206_image.png)

您可以在 [這裡](https://foundryvtt.com/article/dice/) 中找到更複雜的骰子語法（例如Roll和保留）。

### 更多擲骰提示

* 支援某些JS 方式運算擲骰 [Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) . 例如 `[[ 1d6 + round(7/2) ]]`

### 擲骰指令

`/r` or `/roll`該命令會進行正常的公開擲骰。 您也可以使用以下替代命令：

`/gmr` or `/gmroll` 僅向您和GM顯示該擲骰。

`/br` or `/broll` or `/blindroll` 僅向GM顯示該擲骰（並向您隱藏該擲骰）。

`/sr` or `/selfroll` 僅向您顯示該擲骰。

### 使用屬性

如果您使用的是`Simple Worldbuilding System`，則角色卡會在“屬性”中儲存有關角色的不同基本屬性。 你可以將屬性用作公式中的變量，因此，當您更改一個屬性時，所有公式，Macros等都會同步更新。

您可以擲骰指令中加入 `@屬性` 中來引用這些屬性。

對於具有“init”的屬性，它看起來像這樣：

    /r 2d10 + @prof

當您使用它時，它將看起來像下面那樣，假設init屬性包含數字“ 2”：

![示範](https://i.imgur.com/Vw8jedz.png)

除非您選擇了其他Token，否則它將自動從你所分配到角色的屬性中提取數據。

#### 查看屬性

如果您想查看可以使用的 @屬性 列表，請打開控制台（F12）並鍵入`_token.actor.data.data`（已選擇token）。 您將看到可以用 點. 表示 的選項列表。 例如，如果您希望引用stealth ，則為： `@skills.ste.mod`

您可以使用@符號訪問的頂級屬性是：`@abilities，@attributes，@bonuses，@currency，@details，@resources，@skills，@spells，@traits`

## 格式

### 方括號擲骰

您可以通過在方括號在文字中進行 *任何* 擲骰中，如下所示：

```Sad 一步跳起 [[2d10+5]]米 跳過那隻頑皮的章魚龍.```

當您發送到聊天信息時，擲骰結果將顯示在文本內，如下所示：

![示範](https://i.imgur.com/jlOgjv6.png)

您還可以將“延遲”或“可點擊”的擲骰插入文本內。 這將顯示骰子公式，而不是自動擲骰，當你單擊時，才會擲骰。 您可以通過在方括號內包含擲骰命令來完成此操作。 像這樣：

```Sad 一步跳起 [[/r 2d10+5]]米 跳過那隻頑皮的章魚龍.```

當您發送到聊天時，它將如下所示：

![示範](https://i.imgur.com/dIY1rFf.png)

當您單擊擲骰公式時，它將進行擲骰，如下所示：

![示範](https://i.imgur.com/7rDxMMD.png)

您可以將內嵌式擲骰嵌入到Foundry上的任何文本框中（包括在角色卡，日記條目，Macros和聊天框裡），並且可以使用HTML格式，只要在粘貼之前壓縮代碼即可 。有關更多信息，請參見HTML格式。

## 註釋

您可以通過在文字末尾的後面加上＃來添加註釋，如下所示：

    /r 2d10+5 #This is my attack roll

結果如下所示：

![示範](https://i.imgur.com/KOfyoN9.png)

您還可以在文本中包含壓縮的html格式，如下所示：

    /r 2d10+5 #<h2> This is my attack roll </h2>

結果如下所示：

![示範](https://i.imgur.com/wHaW2Hs.png)

有關HTML的更多信息，請參閱HTML格式。

## 引用

您可以在Foundry的 *任何* 文本框中引用Foundry中的幾乎所有項目，如下所示：

| Entity          | Code                                |
| --------------- | ----------------------------------- |
| 角色 | `@Actor[Character Name]` (請注意，必須存在一個使用該名稱的角色。 不只是合集條目.)           |
| 場景           | `@Scene[Scene Name]`                |
| 道具            | `@Item[Item Name]` （與角色一樣，它指的是一個項目（請參見項目邊欄）。僅提供一個合集條目是不夠的。）                                    |
| 日誌   | `@JournalEntry[Journal Entry Name]` |
| 合集      | `@Compendium[Entry Name]`           |
| 擲骰表      | `@RollTable[Roll Table Name]`       |
| Macro           | `@Macro[Macro Name]`                |

請注意，鏈接到Macro或擲骰表不會自動擲骰-它會建立一個必須單擊進行擲骰的按鈕。

條目/對象鏈接區分大小寫！ 類型（“JournalEntry”而不是“ journalEntry”）和對象名稱（“Abacus”而不是“abacus”）。 您也可以將對象拖放到某些位置以新增鏈接。

對於Actor，Item和其他 @<type>[<name>] 方式的引用，將引用特定對象的ID。 如果以後刪除該對象，即使使用相同名稱創建了一個新對象，現有鏈接也不會引用該新對象。還要注意，如果特定名稱存在多個相同選擇，則將選擇“第一個”。 如果該“第一個”碰巧是玩家由於權限而看不到的那個，那麼即使該玩家可以看見“第二個”，點擊引用鏈接也會給他們一個「權限錯誤」。

例子:

    這是一些隨機文本，這是我的技能檢查，Macro按鈕 @Macro[Initiative]也是我要鏈接到 @JournalEntry[Kryx SRD] 的日記條目

Displays this:

![示範](https://paper-attachments.dropbox.com/s_18A9487B0EE61A81F393FA91A13C89DE192362383B6581DB8EF3234F6BDD2D71_1587943331481_image.png)

### HTML

您可以使用HTML格式化Macro,日誌等（基本上所有帶有文本框的內容）。

### **學習HTML**

HTML非常簡單。 如果您在文本周圍加上標記，就已經是在使用HTML了！ 如果您想學習一些基本的HTML，則到[freecodecamp](https://www.freecodecamp.org/learn/)完成前幾節課，這不超過20分鐘。

**使用內建HTML編輯器**
您不必完全學習任何HTML就能使您的格式美觀。 在任何描述框中（在您的角色，日誌，道具上），都應該有一個可以使用的文本編輯器。 只需將骰子和引用及按鈕結合使用，即可在那裡進行格式化，從而使事情變得更加漂亮。

**使用外部HTML編輯器**
對於Macro，您將需要使用外部HTML編輯器。 您可以使用[htmleditor](http://htmleditor.in/index.html)。 但是，在粘貼之前，請確保將其通過[壓縮器](https://www.textfixer.com/html/compress-html-compression.php)。

## 嵌入網頁

您可以將大多數網頁嵌入到Foundry中，只要有文本編輯器即可。打開文本編輯器後，單擊源按鈕（<>）並粘貼以下代碼，將YOUR_LINK替換為所需網站的鏈接（包括鏈接中的https://）：

```html
<div style="overflow: hidden; position: relative; min-height: 100%;"><iframe style="border: 0; height: 100%; left: 0; position: absolute; top: 0; width: 100%;" src="YOUR_LINK" width="100%" height="100%" allowfullscreen="true;">
</iframe></div>
```

這是嵌入規則引用，SRD，影片甚至角色卡的有用方法。 由於網站的私有設置，某些網站將不會接受使用嵌入，而某些網站（例如YouTube）則要求您使用特殊的嵌入鏈接。

## 角色卡

如果您使用的是Simple Worldbuilding System，則可以在角色卡的“描述”框中放置可單擊的擲骰按鈕，除了骰子語法，引用和格式外，什麼也不用改變。 在這裡有一個示範：
![示範](https://paper-attachments.dropbox.com/s_18A9487B0EE61A81F393FA91A13C89DE192362383B6581DB8EF3234F6BDD2D71_1587945338468_image.png)
