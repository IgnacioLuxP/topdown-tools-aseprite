# Import Slices
Import the [slices](https://www.aseprite.org/docs/slices/) from another open sprite or from a specific file. 

It can be accessed through `Sprite -> Import Slices [Experimental]`.

## Warning
This feature's current state can be described as **Experimental** at best, so use with care. The reason is that many internal functions and attributes of slices are not exposed to Aseprite's [API](https://github.com/aseprite/api) which greatly limits this feature's capabilities.  

For example, slices can be animated (i.e., a slice can have one position/size in frame `1` and different values in frame `2`). This information is stored in the file and can be [exported to a JSON file](https://www.aseprite.org/docs/slices/#exporting-slices). However, the frame information of each slice is not readable in the API which may cause different issues, like exporting ghost or duplicate slice data.

These issues seem to be almost non existent when working with sprites that have a single frame in them, because each slice has information only for that single frame. Therefore, this tool might be useful when working with tilemaps or UI graphic elements.

Additionally, you might want to check out this [SliceList script](https://github.com/jsmars/DevTools#slicelistlua) by [jsmars](https://github.com/jsmars) in case you encounter some weird issues. It lists potentially corrupt or hidden slices and lets you delete them in case they are not listed in the UI.

### Demo

Watch a demo of this tool [here](./demos/Demo-Extras-ImportSlices.md#import-slices).

## Instructions

### Import from another open sprite:
1. Open a sprite where you want to import the slices. The extension will warn you if this sprite has pre-existing slices.
2. Open the sprite that has the slices you want to import.
3. Switch to the first sprite's tab.
4. Go to `Sprite -> Import Slices [Experimental]`. The script will use the current tab as the **destination sprite**.
6. Switch to the second sprite's tab.
7. Click `Select current tab` to set it as the **source sprite**.
8. Click `Import Slices`.
9. You're done!

### Import from a file:
1. Open a sprite where you want to import the slices. The extension will warn you if this sprite has pre-existing slices.
2. Go to `Sprite -> Import Slices [Experimental]`. 
3. The script will use the current tab as the **destination sprite**.
4. Click `Select file` and pick a sprite that has the slices you want to import. This will be your **source sprite**.
5. Click `Import Slices`.
6. You're done!

## Additional Settings
+ **Add sprite name to slices:** Adds the sprite's filename to the beginning of the slice name.
	+ Example: Importing a slice named `Button` from sprite `UI-green.aseprite`  with this option enabled will rename the imported slice to **"UI-green: Button"**.