# Advanced Merge

## Script interface
I'm providing a simpler script version you can run as a script in case you find the [Advanced Merge](Merge-Advanced.md#advanced-merge) dialog too cumbersome or you'd like to integrate it with your automated [command-line interface](https://www.aseprite.org/docs/cli/#script) (CLI) process. Setting it up with your own settings should be fairly accessible for anyone no matter their skill level.

The only requirement is to have the TopDownTools extensions enabled and installed and have the `AdvancedMergeScript.lua` script file copied to Aseprite's `scripts` [directory](https://www.aseprite.org/docs/scripting/). The reason the extension is required is that most of the processes and functions in this "light weight" script are called from the extension's own lengthy internal scripts.

This bare bones script can be accessed like any other script, via the `File -> Scripts` menu in the UI, or by adding a `--script` parameter to your batch / bash command.

## Instructions:

 1. Install and enable the TopDownTools extension.
 2. Navigate to the extension directory.
 3. Open the `script-interface` subfolder.
 4. Copy the `.lua` script inside.
 5. Navigate to Aseprite's scripts folder (or open Aseprite and go to `File -> Scripts -> Open Scripts Folder`).
 6. Paste the `.lua` script.
 7. Edit the file with your desired settings. 
 8. *Optional:* Run the script from `File -> Scripts -> AdvancedMergeScript` (you might need to rescan scripts, just once so it pops in the scripts list).
 9. *Optional:* Run the script via CLI.
 10. You're done!

If you have troubles installing the script you can check [this](https://community.aseprite.org/t/aseprite-scripts-collection/3599) thread.

## Command-Line Interface parameters

A few CLI parameters can be set on your batch script, but most of the script's settings should be set up in the script's `.lua` file.  These parameters will just override the relevant settings on the script.

>If changing the `lua` file is too much of a hassle for you, you're free to modify the script to accept as many parameters from the command-line as you see fit. You can read a little about it [here](https://github.com/aseprite/api/blob/main/api/app.md#appparams).  The purpose of the relevant variables is described in this page and most of the source code (though not particularly efficient nor elegant ;') is documented extensively.

### `script-param` arguments
+ `saveto=file`: `file` should a path plus a filename. If the path is not specified, the script will revert to the path set in the `.lua` file. If that directory does not exist, it will attempt to save the new sprite in the directory of one of the source sprites. It will print out the location of the saved file in case you have trouble finding it.
+ `forcerw=false`: If **true** it will overwrite the file if it already exists at the specified location. If **false**, it will create a copy as mentioned below. Note that if the path is not valid or cannot be created, it will disable this overwrite just to make sure no files are accidentally overwritten.

### Example .bat:
```bash
aseprite.exe -b "H:\Game\Sprites\Attack1.ase" "H:\Game\Sprites\Attack2.ase" --script-param saveto="H:\Game\Sprites\Attack Merged.ase" --script-param forcerw=true --script "C:\Users\USERNAME\AppData\Roaming\Aseprite\scripts\AdvancedMergeScript.lua" 
```

## Script parameters
These settings need to be set up in the `AdvancedMergeScript.lua` file. Keep a backup in case the script breaks (though it should not affect the extension in any way).

+ `saveNewSprite`:  `true` or `false`
	+  if **true**, it will try to save the new sprite created in the merge process
	+ if **false**, it will leave the new sprite open and you manually save it later

>**NOTE:** If the file already exists at the specified location, the script will prompt you to overwrite or save a copy. The saved copy will have the specified filename and 5 random numbers (Ex: `Slash 68209.ase`).

+ `closeAll`:  `true` or `false`
	+ if **true**, it will close all the open sprites after the process is finished, the new sprite included.
+ `path`: the file path where the new sprite should be saved.
	+ Example: `"H:\\GameDev\\Sprites\\"` (on Windows)
>Note the use of the double backslash `\\`. The backlash `\` is a special character in Lua so the path needs to be written that way. Alternatively, you may change this symbol to a single forward slash `/`.
+ `filename`: a `string` with the desired filename, **without the `.ase` or .`aseprite` extension**.
	+ Example: `"NPC Priest ALL DIRECTIONS"` (this will be saved as `NPC Priest ALL DIRECTIONS.ase` in the above path)

>**IMPORTANT:** The specified path should exist but in case it doesn't, the script will try to create the necessary folders. Keep in mind that this feature is not supported in all of Aseprite's versions. Particularly, the methods invoked to create this folder were included in Aseprite version `1.2.26`. If your Aseprite version is older and the path does not exist, the script will attempt to save the new sprite at one of the source sprite's location with the filename you specified.

### Merge settings
+ `stackedMode`: `true` or `false`
	+ if **true**, it will import the source sprites as a [stack of layers/groups](Merge-Advanced.md#stacked-layers-fold-sprites). 
	+ set it to **false** if you want to [import as a sequence](Merge-Advanced.md#sequence-fanned-out).

+ `singleLayer`: `true` or `false`
	+ if **true**, it will import every source sprite into a [single layer](Merge-Advanced.md#import-into-a-single-layer).
	+ if **false**, it will import each sprite to its own layer or group of layers.

+ `copyFlattened`: `true` or `false`
	+  if **true**, it will copy each source sprite or its layers [as a flattened image](Merge-Advanced.md#flatten-sprite-layers). 
	+ set it to **false** to [import the layer structure](Merge-Advanced.md#import-layer-structure).

+ `onlyVisible`:  `true` or `false`
	+ Mostly relevant when importing the layer structure (i.e., `copyFlattened = false`)

+ `ignoreBackground`:  `true` or `false`
	+ Mostly relevant when importing the layer structure (i.e., `copyFlattened = false`)
	
+ `createGroups`: `true` or `false`. Same as [Group merged sprites](Merge-Advanced.md#group-merged-sprites).
	+ ***Must be* `true`** ***when importing the layer structure*** (i.e., `copyFlattened = false`)

+ `canvas`: `string` variable. Applies if importing sprites with [different canvas dimensions](Merge-Advanced.md#canvas-size-settings). Only accepts these 3 strings:
	+ `"offset"`: default, same as **Center with offset**.
	+ `"center"`: same as **Absolute center** (only works like you'd expect when importing layer structure)
	+ `"doNothing"`: same as **Do Nothing**.

+ `copyFrameRate`: `true` or `false`. Same as [Copy frame rate](Merge-Advanced.md#copy-framerate).

+ `sortFilenames`: `true` or `false`. Same as [Sort sprites by filenames](Merge-Advanced.md#sort-sprites-by-filename).

+ `sortByDirection`: `true` or `false`. Same as [Sort by sprite directions](Merge-Advanced.md#sort-sprites-by-directions).
	+ Only relevant if you care to use a [direction preset](Merge-DirectionPresets.md#direction-name-presets).

+ `addNewTags`: `true` or `false`. Same as [Add delimiting tags](Merge-Advanced.md#add-delimiting-tags).
	+ **NOTE:** Will not work if `stackedMode` is **true**.
 
+ `importTags`: `true` or `false`. Same as [Import tags](Merge-Advanced.md#import-tags).

### Naming settings
These settings are stored inside the `naming` table in the Merge script. Some combinations are valid (like custom names and direction names) while others are not. The console will show you some error if these parameters are set up incorrectly.

[**Name layers with sprite filenames**](Merge-Advanced.md#name-layerstags-with-sprite-filenames):
+ `naming.useFilenames`: `true` or `false`
+ `naming.removeFrameNumbers`: `true` or `false`

[**Name layers/tags with a custom name**](Merge-Advanced.md#name-layerstags-with-a-custom-name):
+ `naming.useCustomName`: `true` or `false`
+ `naming.customString`: the desired custom name as a `string`
	+ Example: `naming.customString = "Some Name"`

[**Name layers/tags with a direction name**](Merge-Advanced.md#name-layerstags-with-a-direction-name):
+ `naming.useDirectionNames`: `true` or `false`
+ `naming.cachedPresetName`: the [preset name](Merge-DirectionPresets.md#direction-name-presets) as a `string`. Must be the exact name without the file extension.
	+ Example: `naming.cachedPresetName = "Relative 8 Mixed D-Down"` 
+ `naming.cachedIsDefaultPreset`: `true` or `false` 
	+ it should be **true** for a [default preset](Merge-DirectionPresets.md#default-direction-presets)
	+ or **false** if it's a [custom preset](Merge-DirectionPresets.md#custom-direction-presets).

+ `naming.stringSeparator`: a `string` or character to separate a prefix/suffix
	+ Example: `naming.stringSeparator = "_"`

### Extra Features

Your workflow may require that you work with sprites that are not stacked or have as few duplicate frames as possible, so these additional settings might help you save time instead of opening each dialog manually.

[**Fan Out Layers**](Layers-FanOut.md#fan-out-layers)
>**NOTE:** Will only work if `stackedMode` is `false`. Otherwise you'd be spreading out the layers twice, potentially making a mess of empty frames and displaced layer groups.
+ `fanOut` : `true` or `false`
	+ if **true**, apply the Fan Out process to the layers in the new sprite.
+ `extendTags`: `true` or `false`. Same as [Extend tags from base stack](Layers-FanOut.md#fan-out-settings). Consider at what point you're importing tags because you might not need to do it twice.
+ `flattenAll`: `true` or `false`. Same as [Flatten layers](Layers-FanOut.md#fan-out-settings).
+ `flattenVisible`: `true` or `false`. Same as [Flatten visible layers](Layers-FanOut.md#fan-out-settings).

[**Re-Link Cels**](Relink-RelinkCels.md#re-link-all-cels)
+ `relinkCels`: `true` or `false`
	+ if **true**, re-link cels in the new sprite.
+ `onlyContinuous`: `true` or `false`. Same as [link only in continuous layers](Relink-RelinkCels.md#link-settings). Mostly relevant if importing layer structure. Otherwise, you may want to set it to **false** so it tries to link all identical cels no matter their layer settings.

## Troubleshooting
If you receive the error below when using a direction name preset, make sure its name is correct and that the table can be loaded properly.
```
C:\Users\USERNAME\AppData\Roaming\Aseprite\extensions\topdown-tools\shared\TopDown_PresetsHelper.lua:180: attempt to index a nil value (local 'preset')
```

## Default Parameters (Template)

```lua
--automatically saves the file to the path and filename below
local saveNewSprite = true
--close every sprite, the merged sprite as well
local closeAll = true

--[[NOTE: if using CLI, the 'path' and 'filename' variables will be overwritten by script-param(s)
If you're on Windows, the backslash "\"" is a special character in Lua, so you'll have to duplicate them 
like in the example below or replace the slashes with forward slashes: /
--Ex: H:\GameDev\Sprites\ -> "H:\\GameDev\\Sprites\\"	
-]]
--Ex: H:\GameDev\Sprites\ -> "H:\\GameDev\\Sprites\\"
local path = "H:\\GameDev\\Sprites\\Player\\"
--A filename without the extension
local filename = "Attack1 Merged"

--Merge settings (the first tab in the Advanced Merge dialog)
--+++++++++++++++++++++++++++++++++++++--
local stackedMode = false
local singleLayer = false
local copyFlattened = true
local onlyVisible = true
local ignoreBackground = true
local createGroups = false
--canvas  should be filled with "offset", "center" or "doNothing" (capitalization is important!)
local canvas = "offset"
local copyFrameRate = true
local sortFilenames = false
local sortByDirection = true
local addNewTags = true
local importTags = true

--Naming settings (second tab in the Advanced Merge dialog)
--+++++++++++++++++++++++++++++++++++++--
local naming = {}
--using filenames for layer/tag names:
naming.useFilenames = true
naming.removeFrameNumbers = false

--using a custom name, it cannot be true if useFilenames is also true
naming.useCustomName = false
naming.customString = ""

--use direction names
naming.useDirectionNames = false
--should be the preset's filename without the extension
naming.cachedPresetName = "Relative 4 Long Down"
naming.cachedIsDefaultPreset = true

--the separator for prefix/suffix
naming.stringSeparator = ""

--Optional fan out process
--+++++++++++++++++++++++++++++++++++++--
--IMPORTANT: This will be skipped if stackedMode = false (or it would fan out the sprites twice, making a mess)
local fanOut = false
local extendTags = true
local flattenAll = false
local flattenVisible = false

--Optional re link cels process
--+++++++++++++++++++++++++++++++++++++--
local relinkCels = false
local onlyContinuous = false
```
