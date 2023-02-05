---
title: Initiative ohne Token und Actor zum combat tracker hinzufügen
description: 
published: true
date: 2023-02-05T14:23:52.836Z
tags: 
editor: markdown
dateCreated: 2023-02-05T14:23:18.336Z
---

# Initiative-Script

Die Initiative für einen Actor kann mittels Script ermittelt werden. Dazu wird das combat-Objekt des globalen game-Objekt benötigt. 

Das combat-Objekt existiert nur, wenn auch ein aktiver combat tacker existiert. 

Die Initiative muss einem Combatant zugewiesen werden. Falls dieser noch nciht existiert mit er vorher erschaffen werden. 

Beispielcode für die Initiative Bestimmung für einen Actor unabhängig von einem Token.

```js	
let newInitiative; // the initiative value for the combatant

// ...
// The intnitiative value is calculated here
// ...

let combatant = game.combat.getCombatantByActor(actor._id);
if (!combatant) {
  await game.combat.createEmbeddedDocuments('Combatant', [{actorId: actor._id}]);
  combatant = game.combat.getCombatantByActor(actor._id);
}

const initiatives = {
  _id: combatant._id,
  initiative: newInitiative
};

await game.combat.updateEmbeddedDocuments('Combatant', [initiatives]);
```

Dieser kann z.B. im actor-sheet mittels click-event getriggert werden.