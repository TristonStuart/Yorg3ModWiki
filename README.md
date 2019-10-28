
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
## [ModBuild](https://github.com/dengr1065/modbuild "ModBuild")
**[ModBuild](https://github.com/dengr1065/modbuild "ModBuild") is a custom mod loader for mod developers.**

**It can :**
* Merge multiple mod files into one
* Supports plugins
* Supports AML api
* Push mod projects to publishable mod

ModBuild is specifically designed for mod developers, to help make the creation of mods easier.
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
Ran : Before Game Exits <br> Args : None
#### consumerPrioManuallyChanged
Ran : When building resource priority is changed <br> Args : [Building]
#### damageDispatched
Ran : When Zombie Attacks Something <br> Args : [DamageForm, Entity]
#### dayNightChanged
Ran : When day / night changes <br> Args : None
#### entityAdded
Ran : When an entity is created <br> Args : [Entity]
#### entityDestroyed
Ran : When an entity is destroyed <br> Args : [Entity]
#### entityGotNewComponent
Ran : When an entity gets a new component <br> Args : [Entity]
#### entityQueuedForDestroy
Ran : Before an entity is destroyed <br> Args : [Entity]
#### fullGameResync
Ran : In multiplayer when the server detected a simulation state mismatch, this resets the game to a previous point in time <br> Args : None
#### gameOver
Ran : When game ends <br> Args : None
#### gameRejectedFromServer
Ran : When the server does not accept the savegame (Invalid sync token / anticheat or mods) <br> Args : None
#### gameRestored
Ran : When game is restored (Called at the beginning when a savegame has been loaded)  <br> Args : None
#### gameSaved
Ran : Unused <br> Args : None
#### gameSyncedWithServer
Ran : When game was successfully synced with server <br> Args : None
#### mapThemeLoaded
Ran : When map theme is fully loaded <br> Args : None
#### modDrawScreenSpace
Ran : Every frame <br> Args : [DrawParameters (contains e.g. the Canvas, Root and Zoom)]
#### modDrawWorldSpace
Ran : When rendering at world space <br> Args : [DrawParameters (contains e.g. the Canvas, Root and Zoom)]
#### modUpdateTick
Ran : Every tick <br> Args : None
#### newlyUnlockedBuildingsChanged
Ran : When new buildings are unlocked <br> Args : [Array<buildingId: string>]
#### performAsync
Ran : When the code needs to use async methods, those will be executed after the current logic step <br> Args : [callback: function() : void, name?: string]
#### postLoadHook
Ran : After game is loaded <br> Args : None
#### readyToRender
Ran : When game is ready to render <br> Args : None
#### requireRoutingUpdate
Ran : When game needs to make a routing update <br> Args : None
#### resized
Ran : When window is resized <br> Args : [width: number, height:number]
#### skillsChanged
Ran : When the skills are changed <br> Args : None
#### streetDestroyed
Ran : Unused <br> Args : [x, number, y:number]
#### streetPlaced
Ran : When a new street was placed <br> Args : [x: number, y:number]
#### structureDestroyed
Ran : When a structure is destroyed <br> Args : [Building]
#### structureEnhanced
Ran : When a structure is enhanced (not upgraded) <br> Args : [Entity]
#### structurePlaced
Ran : When a structure is placed <br> Args : [Entity]
#### structureUpgraded
Ran : When a structure is upgraded <br> Args : [Entity, Upgrade]
