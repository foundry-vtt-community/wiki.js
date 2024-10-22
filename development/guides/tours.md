---
title: Tours
description: Systems and Modules sometimes are not intuitiv to new players or even experienced players or GMs. To hint them getting the point early you may want to create a tour through your meachanics.
published: true
date: 2024-10-22T14:31:51.597Z
tags: tours
editor: markdown
dateCreated: 2024-10-21T17:36:21.736Z
---

# Tour Management

![Up to date as of 11](https://img.shields.io/badge/FoundryVTT-v11-informational)

As you like to show the mechanics of your module or system to your new users, you may do it by tours.

#### Chat usage Tour (Example)

In this example, you can see how to

-   register your tours from multiple JSON files
-   add multiple steps to a clean JSON file
-   bound the position of a steps window to an element's ID (#chat-form)
-   run code before entering a step of a tour (e.g. writing a chat message)
-   switching the active tab to the right one due to be able to address the elements ID (#chat-form is in tab “chat” and not “settings” where you star the tour)
-	manage access for GM only

#### myTours.js

```javascript
Hooks.once("setup", async function () {
	registerMyTours();
}
// necessary to toggle tab from settings to chat before you are able to target #chat
Hooks.on("closeToursManagement", function (event) {
  ui.sidebar.activateTab("chat");
});
class MyTour extends Tour {
	async _preStep() {
    	await super._preStep();
    	const currentStep = this.currentStep;
    	if(currentStep.id == "chat") {
      	ChatMessage.create({
        	content: "<h2>Demo MyTour</h2>",
        	speaker: ChatMessage.getSpeaker({alias: "MyTour", color: "#ff0000"})
      	});
    	} else {
      	console.log("STA Stardate | Tours _preStep: ",currentStep.id);
    	}
  	}
}
async function registerMyTours() {
  try {
    game.tours.register(moduleName, 'format', await MyTour.fromJSON('/modules/'+moduleName+'/tours/chat.json'));
    if(game.user.isGM) {
      game.tours.register(moduleName, 'settings', await MyTour.fromJSON('/modules/'+moduleName+'/tours/settings.json'));
    }
  } catch (error) {
    console.error("MyTour | Error registering tours: ",error);
  }
}
```

#### Chat.json

```javascript
{
    "title": "Chat Guide",
    "description": "Howto use the chat",
    "canBeResumed": false,
    "display": true,
    "steps": [
        {
            "id": "chat",
            "selector": "#chat-form",
            "title": "Chat",
            "content": "Here one can write messages to the other players.",
            "tab": {
                "parent": "sidebar",
                "id": "chat"
            }
           
        },
        {
            "id": "stepTwo",
        	// ...
        }
	]
}
```

#### Tour API

If you like to create more complex tours using interaction and training for your users, you want to take a look at the API-Documentation of the [Tours Class](https://foundryvtt.com/api/classes/client.Tour.html).