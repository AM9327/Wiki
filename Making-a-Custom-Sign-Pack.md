**NOTE:** This page is still under construction and its contents may change at any time as we finalize features for v1.0.0 of Traffic Control.

***
## Introduction
Starting with version 1.0.0, Traffic Control supports custom Sign Packs. Making one may seem a tad complex, but we will go over every aspect of making a Sign Pack in this wiki page.

**What you'll need**
* An advanced text editor such as Notepad++ or Visual Studio Code.
   * You can also use Windows Notepad, but advanced text editors have better formatting support for JSON.
* A JSON Validator. We recommend [JSON Lint](https://jsonlint.com/)
* A UUID Generator. We recommend [UUID Generator](https://www.uuidgenerator.net/)

If you feel like this may be too much, we have an example Sign Pack available for download [here.](https://github.com/RavenholmZombie/trafficcontrol/releases/tag/ESP)

***
## Phase One - File Structure
To get started, you will need to make a .ZIP folder for your Sign Pack's files to reside inside.

Make a working folder somewhere on your computer. Inside this folder, make a folder for each shape of sign you want to add. Also make an empty `signs.json` file. It'll be necessary for Phase Two.

**Default Types:**
* `circle`
* `diamond`
* `misc`
* `rectangle`
* `square`
* `triangle`

An example Sign Pack would be laid out like this:

```
└── My Totally Awesome Custom Signs Pack.zip/
    ├── rectangle
    ├── square
    ├── circle
    ├── misc
    └── signs.json
```

### Making Your Sign Textures

Using any image editing software (Photoshop, GIMP, Paint.NET, or even Windows Paint), create your sign textures and save them as .PNG files to the folders inside of your working directory based on their overall shape (square signs go in the `square` folder, etc), or put them into a `custom` folder for a custom shape.

**Minecraft's symmetrical texture resolution rule applies, so ensure your textures have a symmetrical resolution (16x16, 32x32, etc) or else the game will refuse to load them!**


***

## Phase Two - Taming the JSON Beast
While it may seem complicated at first, creating your first `signs.json` file is actually pretty straightforward!

You can download an example `signs.json` file from Pastebin [here](https://pastebin.com/raw/eWpVftkt).

For the sake of ease, we'll explain how the `signs.json` file works in parts.

**Part 1 - Introduction and Definitions**
```
	"name": "My Custom Sign Pack",
	"author": "RavenholmZombie",
	"pack_id": "a7823a4d-6634-47c8-bdb5-5f39acb52a87",
	"types": 
	{
		"state": "US State Signs"
	},
```

| Property | Required | Data Type | Default Value | Notes |
| -------- | :------: | --------- | ------------- | ----- |
| `name`   | Yes      | string    |               | What your Sign Pack will be called. This variable will be shown in-game. |
| `pack_id` | Yes     | string (must be UUID) |   | Randomly generated UUID. You can use the UUID generator [we linked above](https://github.com/RavenholmZombie/trafficcontrol/wiki/Making-a-Custom-Sign-Pack#introduction) to create a UUID. <br /> * NOTE: It is recommended that `pack_id` be given a new UUID for each update you make to your Sign Pack. |
| `signs`  | Yes      | array of objects (see part 2) |     | This is where all of your sign data will be placed |
| `author` | No       | string    | *None*        |       |
| `types`    | No       | object (see below) | *No custom types will be loaded* | List of custom sign types you want your pack to use. |

**`types` Object Information**

This is a key/value list of properties where the key is the type used in your sign objects (see below) and the value is the friendly name to be shown in game.

**Part 2 - Defining your Signs**
```
	"signs": 
	[
		{
			"id": "0322a20d-b2df-45d3-b43a-c753273aa8ab",
			"name": "WI Sign",
			"type": "state",
			"front": "Pixel Wisconsin Route Shield.png",
			"back": "wiback.png",
                        "variant": "2",
                        "tooltip": "WI Highway Route Sign",
                        "note": "Use this sign at regular intervals and after major intersections to indicate route",
                        "halfheight": true,
			"textlines":
			[
				{
					"label": "Route Number",
					"x": 8,
					"y": 8,
					"width": 15.5,
					"maxlength": 3,
					"xscale": 10,
					"yscale": 10,
					"halign": "center",
					"valign": "center",
					"color": 0
				}
			]
		},

```

| Property | Required | Data Type | Default Value | Notes |
| -------- | :------: | --------- | ------------- | ----- |
| `id`     | Yes      | string (must be UUID) |   | UUID for this particular Sign. Again, use a randomly generated UUID for this. |
| `name`   | Yes      | string    |               | Name of your sign. This is what will be presented in-game. |
| `type`   | Yes      | string    |               | Refers to the shape of this sign. [See above for default shapes](https://github.com/RavenholmZombie/trafficcontrol/wiki/Making-a-Custom-Sign-Pack#phase-one---file-structure) or use one defined in your `types` object above |
| `front`  | Yes      | string    |               | Refers to the texture name for the *front* of this sign. |
| `back`   | No       | string    | `back.png`    | Refers to the texture name for the *back* of this sign. |
| `variant` | No      | string (must be number) | *None* | ***DEPRECATED!*** Provides legacy support for existing worlds. **DO NOT** use this for new signpacks. Only utilize this property if you modified Traffic Control's `signs.json` file prior to the v1.0.0 update and you don't want to break existing worlds. |
| `tooltip` | No      | string    | *None*        | Text to be displayed when the mouse hovers over the sign in the selector in game |
| `note`   | No       | string    | *None*        | Text to be displayed below the sign in the sign GUI |
| `halfheight` | No   | boolean   | `false`       | If true, only renders the lower half of the pole for the sign |
| `textLines` | No    | array of object (see below) | *None* | Defines where editable text fields are on the sign for players |

**`textLines` Object Information**

Text Lines are based on the concept that the sign is broken into a 16x16 grid. You can place text on this grid using (x,y) coordinates. There are also many other options you can utilize - see the table below.

| Property | Required | Data Type | Default Value | Notes |
| -------- | :------: | --------- | ------------- | ----- |
| `label`  | Yes      | string    |               | This text will display above the text field to tell the user what the purpose of the text field is |
| `x`      | Yes      | double    |               | The "x" coordinate of the sign to display your text |
| `y`      | Yes      | double    |               | The "y" coordinate of the sign to display your text |
| `width`  | Yes      | double    |               | Determines the width of the text area on the sign |
| `color`  | Yes      | int       |               | The color of the text on the sign. To get the int color, convert hex to int. <br /> Example: 0xFF0000 (red) = 16711680 |
| `maxlength` | No    | int       | -1 (no max length) | Determines the maximum amount of characters for this text line |
| `xScale` | No       | double    | 1             | How to scale the text on the x-axis. 1 is no scaling, < 1 is scale in, > 1 is scale out. |
| `yScale` | No       | double    | 1             | How to scale the text on the y-axis. 1 is no scaling, < 1 is scale in, > 1 is scale out. |
| `halign` | No       | string    | `left`        | Horizontal text alignment. Options are: `left`, `center`, `right` |
| `valign` | No       | string    | `top`         | Vertical text alignment. Options are: `top`, `center`, `bottom` |

**Each custom sign in your pack will need to have its own dedicated block in `signs.json`**


**An example signs.json file**
```
{
	"name": "My Custom Sign Pack",
	"author": "CSX8600",
	"pack_id": "a7823a4d-6634-47c8-bdb5-5f39acb52a87",
	"types": 
	{
		"custom": "Custom Type"
	},
	"signs": 
	[
		{
			"id": "0322a20d-b2df-45d3-b43a-c753273aa8ab",
			"name": "WI Sign",
			"type": "custom",
			"front": "Pixel Wisconsin Route Shield.png",
			"textlines":
			[
				{
					"label": "Route Number",
					"x": 8,
					"y": 8,
					"width": 15.5,
					"maxlength": 3,
					"xscale": 10,
					"yscale": 10,
					"halign": "center",
					"valign": "center",
					"color": 0
				}
			]
		},
		{
			"id": "0322a20d-b2df-45d3-b43a-c753273aa8ac",
			"name": "TX Sign",
			"type": "custom",
			"front": "texas shield.png",
			"textlines":
			[
				{
					"label": "Route Number",
					"x": 8,
					"y": 6,
					"width": 12.5,
					"maxlength": 20,
					"xscale": 6,
					"yscale": 6,
					"halign": "center",
					"valign": "center",
					"color": 0
				},
				{
					"label": "Test",
					"x": 15,
					"y": 15,
					"width": 16,
					"maxlength": 20,
					"halign": "right",
					"valign": "right",
					"color": 16711680
				}
			]
		}
	]
}

```

***
## Phase 3 - Loading and Testing
Once you've gotten your .zip file created, it is now time to test it! To load your Sign Pack into Minecraft, you will need to place it in the `tc_signpacks` folder, which can be found in your `.minecraft` folder. If `tc_signpacks` is missing, create it.

Then you load up the game and, once in a world, place down a Sign block and begin looking through it for your custom signs. If they show up, then congratulations! If your signs do not appear, check your Minecraft log for any errors that may have occurred with your signs.

**TIP:** You can hot reload all Sign Packs without having to restart Minecraft by pressing `F3 + ]`