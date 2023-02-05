---
title: Würfelwurf in einem Makro
description: Beispiel eines Würfelwurfes als Makro mit Chatausgabe
published: true
date: 2023-02-05T12:10:50.895Z
tags: 
editor: markdown
dateCreated: 2023-02-05T12:10:50.895Z
---

# DSA 3W20 Wurf (simple Version)
```js
//Erzeuge einen Würfelformel mit Zusatztext. Warte auf das Ergebnis der Auswertung der Würfelformel außerdem wird das asynchron ausgeführt. Das heißt andere Prozesse können zwischendurch abgearbeitet werden.
// Notiz: Statt "evaluate" --> kann "roll" verwendet werden beides scheint gleichwertig
// Das ganze kann mit Dice so Nice genutzt werden

let dies = await new Roll("1d20[black]+1d20[red]+1d20[yellow]").evaluate({async: true});

// In dies liegt nun das Ergebnis des Wüfelwurfes bzw. genauer der verarbeiteten Würfelformel

// Erzeueg nun eine html basierte Chat Nachricht welche ausgegeben wird.
let ChatData={
// Die Nachricht wird dem aktiven Sprecher/Actor zugewiesenet. Falls es den Sprecher nicht gibt oder er gelöscht wurde so wird stattdesen der Spielername verwendet.
speaker: ChatMessage.getSpeaker({token: actor}),
// Diese Eintrag wird von Dice so Nice verwendet. Bisher unklar was er genau tut
type: CONST.CHAT_MESSAGE_TYPES.ROLL,
// Hier kommt der eigentliche Wurf. Es wird die Zufallszahl in der Würfelformel erzeugt und die Würfelformel wird ausgewertet. Falls mehr als eine Würfelformel/ein Würfel verarbeitet werden solle so sind die Würfelformeln/Würfel als array von objekten zu übergeben --> [r1,r2,r3]
rolls: [dies],
// Diese Eintrag wird von Dice so Nice verwendet. Bisher unklar was er genau tut
rollMode: game.settings.get("core", "rollMode"),
// Mit content wird die HTML Vorlage definiert die für die Ausgabe der Chat Nachricht verwendet werden soll. Jeder Würfel in der Würfelformel ist ein Objekt eines Arrays also müssen wir dieses Objekt auch separat ansprechen mit dies.dice[x]. Erinnerung: In Java Script starten Array mit einem index von 0
content:`
<div class="dice-roll">
  <div class="dice-result">
   <div class="dice-tooltip" style="display: block;">
    <section class="tooltip-part">
        <div class="dice">
              <ol class="dice-rolls">
                <li class="roll die d20" style="transform: scale(1.1);margin-right: 4px">${dies.dice[0].results[0].result}</li>
                <li class="roll die d20 min" style="transform: scale(1.1);margin-right: 4px">${dies.dice[1].results[0].result}</li>
                <li class="roll d20" style="transform: scale(1.1);margin-right: 4px; color: yellow; filter: sepia(0.5) saturate(6)contrast(0.8) brightness(1.1)">${dies.dice[2].results[0].result}</li>
            </ol>

        </div>
    </section>
  </div>
     <h4 class="dice-total">${dies.dice[0].results[0].result} / ${dies.dice[1].results[0].result} / ${dies.dice[2].results[0].result} </h4>
 </div>
</div> `
}
// Hier wird die Nachricht nun ausgegeben
ChatMessage.create(ChatData);
```

