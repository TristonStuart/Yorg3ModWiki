
# YORG.io 3 Modding Wiki
## Example Mods
[Bigger Map Mod](https://github.com/TristonStuart/Yorg3-Mod-Tools/tree/master/Example%20Mods/Bigger%20Map%20Mod "Bigger Map Mod") <br>
[No Fog Mod](https://github.com/TristonStuart/Yorg3-Mod-Tools/tree/master/Example%20Mods/No%20Fog%20Mod "No Fog Mod") <br>
[Better Zombie Spawning](https://github.com/TristonStuart/Yorg3-Mod-Tools/tree/master/Example%20Mods/Better%20Zombie%20Spawning "Better Zombie Spawning") <br>
[More Zoom Mod](https://github.com/TristonStuart/Yorg3-Mod-Tools/tree/master/Example%20Mods/More%20Zoom "More Zoom Mod")

*These mods are all currently in use or are part of other mods, tobspr will not publish any of these to the mod gallery.*
## Getting Started
Getting started can be tricky!

The first thing you will need is the mod developer version of the game. <br> To get the mod dev version of the game use this link :
https://beta.yorg3.io/?xdev_modDeveloper=1

This version is needed to install custom mods. <br> It will check for a mod file at http://localhost:8000/mod.js

You are probably wondering how to host your mod file at that address. <br> You can either make a file hosting server or use one of the ones that have already been made (Check below this).
## File Hosting Servers
Node.js File Server : https://github.com/TristonStuart/Yorg3-Mod-Tools/tree/master/Mod.js%20File%20Server%20(nodejs)/ (Recomended)

Python File Server : https://github.com/tobspr/yorg.io-3-modding-docs/blob/master/sample_mod/mod_testing_server.py
## [Mod Builder](https://github.com/TristonStuart/AdvancedModLoader/tree/master "Advanced Mod Loader (AML)")
**[AML](https://github.com/TristonStuart/AdvancedModLoader/tree/master "AML") is a custom mod loader for the mod developers.**

**It can :**
* Inject Multiple Mods At Once
* Load Custom APIs
* Includes Default APIs To Help With Mod Creation
* Gives Debugging Tools For Mods
* Log Mod And Game Data
* Communicate With Other Mods (with included api)
* Better Visual Representation Of Mods
* Compatibility Checker For Mods
* Combine Multiple Mods Into One File
* Compile Mods And APIs Into Publishable Mod

AML is specifically designed for mod developers, to help make the creation of mods easier. <br> AML has many features to help with easier creation of large and small mods but also has tools that allow 
## Code Snippets / Examples
### Getting app
The app var is important for editing anything outside of the game variables. <br> Getting it is different across released and beta (as of 10/25/19), so 2 methods are shown. <br> The api variable must be accessible.
```javascript
function getApp(api){return api.app;}
var app = api.app;
```
```javascript
function getAppOld(api){return api.exportedVariables.Loader.app;}
var app = api.exportedVariables.Loader.app;
```
### Getting API (Register Mod)
The api variable is needed as it is the mods connection point to the game, without it your mod can do nothing. <br> You will only need to register one mod function per mod. <br> The api variable is used for registering your mod implementation (check below) and also is your link to everything outside the game environment (aka main menu, settings, user details, and more).
```javascript
function myMod(api){
  console.log('I got the api!');
  console.log(api);
}
window.registerMod(myMod);
```
### Register Mod Implementation
Unlike when registering your mod to get the api (check above), your mod implementation will run everytime a game is started and will pass you the game variables through "root". <br> Any mod that wants to interact with the game will need to register a mod implementation.
```javascript
function myMod(api){
  console.log('myMod is registered!');
  function modImplementation(root){
    console.log('Game has started! Got root!');
  }
  api.registerModImplementation(modImplementation);
}
window.registerMod(myMod);
```
### Signals (Hook Functions To Game Events)
Signals are an important tool to hook certain functions to ingame events and processes. <br> Signals allow you to execute a function to draw something on screen (for example) everytime the game wants to draw a frame.

There are many signals : 
**`aboutToDestruct
consumerPrioManuallyChanged
damageDispatched
dayNightChanged
entityAdded
entityDestroyed
entityGotNewComponent
entityQueuedForDestroy
fullGameResync
gameOver
gameRejectedFromServer
gameRestored
gameSaved
gameSyncedWithServer
mapThemeLoaded
modDrawScreenSpace
modDrawWorldSpace
modUpdateTick
newlyUnlockedBuildingsChanged
performAsync
postLoadHook
readyToRender
requireRoutingUpdate
resized
skillsChanged
streetDestroyed
streetPlaced
structureDestroyed
structureEnhanced
structurePlaced
structureUpgraded`**

Example of how to add a hook to a signal :
```javascript
root.signals.postLoadHook.add(function(){
  console.log('Game has fully loaded!');
});
```
### About The Signals :
#### aboutToDestruct
Ran : Before playerBase is destroyed <br> Args : None
#### consumerPrioManuallyChanged
Ran : When building resource priority is changed <br> Args : [BuildingMeta]
#### damageDispatched
Ran : When Zombie Attacks Something <br> Args : [SpecificData (instaKill info), EntityMeta]
#### dayNightChanged
Ran : When day / night changes <br> Args : None
#### entityAdded
Ran : When an entity is created <br> Args : [EntityMeta]
#### entityDestroyed
Ran : When an entity is destroyed <br> Args : [EntityMeta]
#### entityGotNewComponent
Ran : When an entity gets a new component <br> Args : [EntityMeta]
#### entityQueuedForDestroy
Ran : Before an entity is destroyed <br> Args : [EntityMeta]
#### fullGameResync
Ran : (Not known - Might only be used in MP) <br> Args : Unknown
#### gameOver
Ran : When game ends <br> Args : None
#### gameRejectedFromServer
Ran : (Not known - Invalid sync token / anticheat or mods) <br> Args : Unknown
#### gameRestored
Ran : When game is restored from sync <br> Args : None
#### gameSaved
Ran : (Not known) <br> Args : Unknown
#### gameSyncedWithServer
Ran : When game is synced with server <br> Args : None
#### mapThemeLoaded
Ran : When map theme is fully loaded <br> Args : None
#### modDrawScreenSpace
Ran : Every frame <br> Args : [DrawParameters (Canvas - Mod Ui, Root, Zoom)]
#### modDrawWorldSpace
Ran : When rendering at world space <br> Args : [DrawParameters (Canvas, Root, Zoom)]
#### modUpdateTick
Ran : Every tick <br> Args : None
#### newlyUnlockedBuildingsChanged
Ran : When new building is unlocked <br> Args : [SpecificDataArray (Building Name)]
#### performAsync
Ran : When the code needs to use async <br> Args : [CallBackFunction?, Name]
#### postLoadHook
Ran : After game is loaded <br> Args : None
#### readyToRender
Ran : When game is ready to render <br> Args : None
#### requireRoutingUpdate
Ran : When game needs to make a routing update <br> Args : None
#### resized
Ran : When window is resized <br> Args : [Width, Height]
#### skillsChanged
Ran : When the skills are changed <br> Args : None
#### streetDestroyed
Ran : Never (There is nothing using this signal and it is never called) <br> Args : Unknown
#### streetPlaced
Ran : (Not known) <br> Args : Unknown
#### structureDestroyed
Ran : When a structure is destroyed <br> Args : [BuildingMeta]
#### structureEnhanced
Ran : When a structure is enhanced (not upgraded) <br> Args : [EntityMeta]
#### structurePlaced
Ran : When a structure is placed <br> Args : [BuildingMeta]
#### structureUpgraded
Ran : When a structure is upgraded <br> Args : [BuildingMeta, UpgradeMeta]

