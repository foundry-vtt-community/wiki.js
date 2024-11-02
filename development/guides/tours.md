---
title: Tours
description: Systems and Modules sometimes are not intuitiv to new players or even experienced players or GMs. To hint them getting the point early you may want to create a tour through your meachanics.
published: true
date: 2024-11-02T16:40:24.135Z
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
      	console.log("MyTours | Tours _preStep: ",currentStep.id);
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
#### Type of Tours

There are common tours one can start manually but also special tours types inherit from *Tours* Class as:
- CanvasTour
- SetupTour
- SidebarTour

Each type of tour has specific methods to interact with their domain.
(Todo: explain by example)

#### Fields

The JSON for a tour may contain several fields one can use for
- progress flow
- visibility in tour lists
- resumable

```javascript
{
	"title": "MYMODULE.title", // see below at "localization"
  "description": "...",
  "canBeResumed": false, // once started resume at last step
  "display": true // visible in tour list
  "closeWindows": false, // default closes all on tour.start()  
  "steps": [ // array of objects each with possible fields
    {
    "id" "nameOfStep",
    "selector": "#htmlTagID", // htmlElementTyp[parameter-name=\"name\"]
    "title": "...",
    "content": "...",
    "sidebarTab": "chat", // {"chat", "combat", "scenes", "actors", "items", "journal", "tables", "cards", "playlists", "compedium", "settings"}
		"tooltipDirection": "RIGHT" // {"UP", "CENTER", "RIGHT", "LEFT", ...}
    }
  ],
  "suggestedNextTours": [
    "myModule.tourName" // tourName.json / find names by typing 'game.tours' in browser-console
  ],
  "localization": { // this should be done in language/en.json 
  	"MYMODULE.title": "bla bla",
    "MYMODULE.description": "foobar"
  }
}
```
#### Conditional next Tour
The field suggestedNextTour**s** is not able to take more then one tour (by Tours Class ![](https://img.shields.io/badge/FoundryVTT-v12-informational)). At this point also the following workaround for **conditional suggestedNextTour** could help out:

```javascript
class MyTour extends Tour {
  async _preStep() {
      await super._preStep();
      const currentStep = this.currentStep;
      if(currentStep.id == "chat") {
        switch(game.settings.get(moduleName, "chatMode")) {
          case "shutUpAndListenTheGM":
            this.config.suggestedNextTours = [moduleName+".strongGMwordsMSG"];
            break;
          case "haveFun":
            this.config.suggestedNextTours = [moduleName+".funGMgreatings"];
            break;
          case "headCinemaTeamwork":
            this.config.suggestedNextTours = [moduleName+".cinemaWishings"];
            break;
          default:
            break
        }
      } 
    }
}
```


#### Selectors

To get faster finding the right selector here you can find some examples.
```javascript
{
    "title": "Configuration Settings",
    "description": "Howto mark the Button Configuration Settings for Tours",
    "canBeResumed": false,
    "display": true,
    "steps": [
        {
            "id": "configSettings",
            "selector": "button[data-action=\"configure\"]", // html element "button" with [parameter] data-action="configure"
            "title": "Config Tour",
            "content": "MYTRANSLATION.tours.configSettings",
            "sidebarTab" "settings" // {"chat", "combat", "scenes", "actors", "items", "journal", "tables", "cards", "playlists", "compedium", "settings"}
           
        },
        {
            "id": "stepTwo",
        	// ...
        }
	]
}
```


```javascript
{
    "title": "..",
    "description": "...",
    "display": true,
    "steps": [
        {
            "id": "configSettings",
            "selector": ".tabs>a[data-tab='actors']", // .class "tabs" with chield > element "a" and [parameter] "data-tab"
            //...
        }
	]
}
```

#### Settings Tour

To be able guiding your users through the settings one have to know how to trigger this:
1) Hooks renderFormApplication (maybe check if it is settings)
2) closeWindow: false (tour.start will close settings instead)
3) tour.start() on click event

```javascript
Hooks.on("renderFormApplication", (app, html, data) => {
  if(html[0].id == "client-settings") { // else Tour-Management Window also will get the listener
    html.find('#client-settings').prevObject[0].querySelectorAll("[data-tab='MyModuleSettingName']").forEach(element => {
      element.addEventListener('click', (event) => {
        const tour = game.tours.get("myModuleName.tourName"); // find by console.log(game.tours)
        tour.start();
      });
    });
  }
});
```

#### Start Tour at first login

If you like to start the tour once module is activated and players log in, you can do this by checking the status of the tour. Have in mind you also can check if the user isGM if it is a settings tour.

```javascript
Hooks.once("ready", async function () {
	if(game.tours.get("myModuleName.tourName").status == "unstarted") // {"unstarted", "in-progress", "completed"} 
  {
    game.tours.get("myModuleName.tourName").start(); 
  }
});
```
**Tipp:** If you open a window like client-settings you should call tour.start() within a timeout function
```javascript
setTimeout(function(){ 
  game.tours.get("myModuleName.tourName").start();
}, 1000);
```

#### Tour API

If you like to create more complex tours using interaction and training for your users, you want to take a look at the API-Documentation of the [Tours Class](https://foundryvtt.com/api/classes/client.Tour.html).