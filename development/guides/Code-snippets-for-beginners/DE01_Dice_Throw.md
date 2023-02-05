---
title: Würfelwurf in einem Makro
description: Beispiel eines Würfelwurfes als Makro mit Chatausgabe
published: true
date: 2023-02-05T14:16:26.478Z
tags: 
editor: markdown
dateCreated: 2023-02-05T12:10:50.895Z
---

# 3W20 Wurf

### 3W20 Wurf (simple Version)

Es sollen 3W20 in einem Wurf geworfen werden und im Chatfenster soll das Ergebnis ausgegeben werden. Der Code kann so wie er ist als Makro Code eingefügt und als Makro verwendet werden.

Das ganze funktioniert auch mit Dice so Nice so dass die Würfel animiert ausgegeben werden.
```js
//Erzeuge einen Würfelformel mit Zusatztext. Warte auf das Ergebnis der Auswertung der Würfelformel außerdem wird das asynchron ausgeführt. Das heißt andere Prozesse können zwischendurch abgearbeitet werden.
// Notiz: Statt "evaluate" --> kann "roll" verwendet werden beides scheint gleichwertig
// Das ganze kann mit Dice so Nice genutzt werden. Die Farben werden als Text in der Würfelformel übergeben und von Dice so Nice ausgewertet.

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


### 3W20 Wurf (komplexere Version)

Bei diesem Beispiel werden drei einzelne Würfel bzw. aus Sicht von von Foundry drei Würfelformeln verwendet. Auch ein einzelner Würfel stellt in Foundry erst mal eine eigene Würfelformel dar. Außerdem wird Dice so Nice verwendet um die Farbe der Würfel diesmal direkt über Dice so Nice zu steuern und nicht über den Text der Würfelformel.

```js
// Erzeuge ein Würfelformel (term) für jeden einzelnen Würfel als eigenständiges Objekt. Statt "roll" kann auch "evaluate" benutzt werden.
let r1 = await new Roll("1d20").roll({async:true});
let r2 = await new Roll("1d20").roll({async:true});
let r3 = await new Roll("1d20").roll({async:true});

// Wähle wie der Würfel aussehen soll. Das colorset muss in dice so nice existieren. Außerdem muss der englische Namen von Dice so Nice verwendet werden. {colorset: "Color"} fügt den Eigenschaften "Aussehen" eine neue bzw. andere Eigenschaft hinzu.
// Die Aussehen=appearance property (Eigenschaft) enthält bzw. definiert das Aussehen der Würfel! "Flavor" ist Zusatztext der ausgegeben wird und kann in der oben stehenden simplen Version z.B. dazu verwendet werden die Farbe zu definieren. Der unten stehende Code erlaubt es den Zusatztext für andere Ausgaben zu nutzen als z.B. die Farbe zu bestimmen. Man könnte den Zusatztext also nutzen um "Aua das tat aber Weh!" auszugeben.

r1.dice[0].options.appearance = {colorset:"fire"};
r2.dice[0].options.appearance = {colorset:"red"};
r3.dice[0].options.appearance = {colorset:"yellow"};

// Vorbereiten des Chat Data Formulars
let ChatData={
// Nciht sicher was passiert wenn es kein toekn für den Actor gibt. Im Zweifelsfall spricht der würfelnde Spieler.
speaker: ChatMessage.getSpeaker({token: actor}),
// Diese Eintrag wird von Dice so Nice verwendet. Bisher unklar was er genau tut wird von der dice so nice API benötigt
type: CONST.CHAT_MESSAGE_TYPES.ROLL,
// Ein Arry von Würfelformeln/Würfeln welche von dice so nice gewürfel bzw. verwendet werden
rolls: [r1,r2,r3],
// Diese Eintrag wird von Dice so Nice verwendet. Bisher unklar was er genau tut wird von der dice so nice API benötigt
rollMode: game.settings.get("core", "rollMode"),
// html Nachricht für den Sprachchat von FoundryVTT
content:`
<div class="dice-roll">
  <div class="dice-result">
   <div class="dice-tooltip" style="display: block;">
    <section class="tooltip-part">
        <div class="dice">
              <ol class="dice-rolls">
                <li class="roll die d20" style="transform: scale(1.1);margin-right: 4px">${r1.dice[0].results[0].result}</li>
                <li class="roll die d20 min" style="transform: scale(1.1);margin-right: 4px">${r2.dice[0].results[0].result}</li>
                <li class="roll d20" style="transform: scale(1.1);margin-right: 4px; color: yellow; filter: sepia(0.5) saturate(6)contrast(0.8) brightness(1.1)">${r3.dice[0].results[0].result}</li>
            </ol>

        </div>
    </section>
  </div>
     <h4 class="dice-total">${r1.dice[0].results[0].result} / ${r2.dice[0].results[0].result} / ${r3.dice[0].results[0].result} </h4>
 </div>
</div> `
}

ChatMessage.create(ChatData);
```
### 3W20 Wurf (Aussehen: Optionen) aus der Standard API
```js
let r = await new Roll("1d20").evaluate();
r.dice[0].options.appearance = {colorset:"fire"};
r.toMessage();
```

#### Aussehen Eigenschaften des Würfels// Properties for dice appearance
```js
let r = await new Roll("d20").evaluate();
// properties possible for dice appearance
r.dice[0].options.appearance = {
    colorset: "custom",
    foreground: "#FFFFFF",
    background: "#FF0000",
    outline: "#000000",
    edge: "#000000",
    texture: "fire",
    material: "metal",
    font: "Arial Black",
    system: "standard"
};
r.toMessage();
```

#### RollType Beispiel

publicroll, gmroll, blindroll, selfroll --> Folgendes in der Console Eingeben zum testen --> game.settings.get("core", "rollMode")
Es wird ein String/Zeichenkett zurückgegeben 

Siehe auch Codebeispiel mit den Zeilen
--> let RollTypeBe = game.settings.get("core", "rollMode");
--> rollMode: RollTypeBe,

Es lässt sich über den rollMode steuern als welche Art von Würfelwurf gmroll, blindroll etc der Würfelwurf ausgeführt wird.
--> let RollTypeBe = "gmroll";
sollte einen Wurf des Spielleiters erzeugen der für Spieler nicht sichtbar ist.

```JS
//Create a roll term with flavor text and execute the roll. Wait for the result and make the roll async.
// Note: Instead of "evaluate" --> "roll" can be used they seem identical 
let dies = await new Roll("1d20[black]+1d20[red]+1d20[yellow]").evaluate({async: true});

// Create a html based Chat message which will be outputed.

// publicroll, gmroll, blindroll, selfroll --> just paste game.settings.get("core", "rollMode") in console. String is returned 
let RollTypeBe = game.settings.get("core", "rollMode");
// possible examples
// let RollTypeBe = "gmroll";
// let RollTypeBe = "blindroll";
// let RollTypeBe = "selfroll";
// let RollTypeBe = "publicroll";

let ChatData={
// let the roll take part as a roll of the actor. If speaker ist deleted then the roll takes place as the player.
speaker: ChatMessage.getSpeaker({token: actor}),
// This was asked for by the Dice so Nice API. Currently not clear what it exactly does
type: CONST.CHAT_MESSAGE_TYPES.ROLL,
// Rolls a pool of dice/diceterms. If more than one diceterm/dices are rolled then an array of objects needs to be defined --> [r1,r2,r3]
rolls: [dies],
// This was asked for by the Dice so Nice API. Currently not clear what it exactly does
rollMode: RollTypeBe,
// Create HTML template for Chat message output. Each Dice in the dice term is an object in an array thus we need to adress it with dies.dice[x]. Remember: arrays start with an index of 0 in java script
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
// Create the Chat message
ChatMessage.create(ChatData);
```


