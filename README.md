![Nolimits 2 Sounds](https://i.imgur.com/51oxkSC.png)

This is an OpenSource Project for rollercoaster related Sounds to add to the realism of NoLimits 2

## How to use

1. [Download](https://github.com/Emonadeo/Nolimits2Sounds/releases/latest) the latest release and copy it inside your park directory.

2. Add the Script to a coaster. **Note:** It is not needed to change the coaster's operation mode to scripted.
> *Add from File* `->` *Sounds.nl2script*

3. You can now add any sound by naming sections. Some sounds are only applied to a single section, others apply globally.
For example the sound `bm_lift` which applies the B&M Lift sound will only play on the section it is defined to. However when naming a section `bm_roar` the B&M Roar will be played on the entire coaster. **Note:** The B&M Roar is not yet implemented, I just used it as an example.

4. Switch to Play Mode and enjoy :)

## Soundlist

| Name               | Description                                                                | Global | Source                       |
| ------------------ | -------------------------------------------------------------------------- | ------ | ---------------------------- |
| `bm_lift`          | B&M Lifthill Sound                                                         | No     | Silver Star @ Europapark     |
| `gci_lift`         | GCI Lifthill Sound                                                         | No     | Wood Coaster @ Knight Valley |
| `misc_clunky_lift` | Clunky Sound for slow Lifthills                                            | No     | Speed @ Oakwood              |
| `misc_old_lift`    | Vintage Sound for normal and fast Lifthills. Similar to the existing Sound | No     | Speed @ Oakwood              |

# Contributing

You found a sound or recorded one yourself which you want to add? See below how to get it into the game.

## Players

Don't worry if you have no experience in **scripting** &ndash; you can still submit sounds, it'll just take some time until someone implements it.

The following information is recommended to add:

- A working link to the downloadable Sound-File
- **Optional:** Source of the Sound (Which coaster did you take it from)
- **Optional:** Description if the sound isn't clearly distinguishable

**There are two ways to submit a sound:**

#### 1. Opening an Issue

This is the fastest way but you have to have a GitHub account to create an Issue.
Click [here](https://github.com/Emonadeo/Nolimits2Sounds/issues/new) and select the *sound submission* label. After you provided the information named above in the description send it.

Done :)

#### 2. Nolimits-Exchange Forum

There is alternatively a discussion thread on the nolimits-exchange forum ([Link]()). Simply post a new comment providing the information named above and we will be sure to check it out.

## Developers

Adding a sound yourself isn't hard either as we provide a small API. There are just some things to keep in mind.

For this tutorial I will use a **B&M Roar** sound as an example.

#### Setup

First thing you have to do is to **fork** and **clone** this repository. And that's it already.

#### Adding the sound file

All sounds go into the `assets` folder. Normally we distinguish between sounds by manufacturer which also have there own folder inside the `assets` folder. If you're sound is not clearly bound to a manufacturer you can put it in the `misc` (miscallenous) folder.

In my case the file would go into `assets/bm/roar.ogg` (Can't use `&` in a folder name so just leave it out if you have the same problem)

##### Registering it

In order to use it in the script later on we have to assign a **resource id** to the sound. We do this inside the `Sounds.nl2script` file located in the root folder. For our example it will look like this:

`<resource id="bm_roar">assets/bm/roar.ogg</resource>`

The id is unique, so watch out if the sound you're trying to add already exists.

Furthermore take a look into the `src/lib/Sections.nlvm` file. Every sound also has it's id registered in there too so we prevent typos when accessing the resource several times programmatically later on. As for yours add a new **String** which puts the id you also registered in the `sounds.nl2script` into a variable. Make sure the String is **public**, **static** and **final** is we can access it globally. In our example it'll look like this:

`public static final String BM_ROAR = "bm_roar";`

#### Adding the Sound-Class

Now we get into the actual Scripting! Hooray!

Each sound has it's own **.nlvm** file. These are located inside `src/sounds`. As a result create a new class after the following naming scheme: `<manufacturer><name>`. So for our example it would be `BMRoar.nlvm`.

To make the implementation significantly easier I provided a **TrackedSound** class which provides 4 methods:

```
package sounds;

import com.nolimitscoaster.*;

import api.*;
import lib.*;

public class BMRoar extends TrackedSound
{
	public BMRoar(Simulator sim, Coaster coaster) // The constructor of your Sound-Class
	{
		super(sim, coaster, Sections.BM_ROAR); // Resource ID goes in here
	}

	public bool shouldPlay(Coaster coaster, Train train)
	{
		// Mandatory. The sound will only play as long as this method returns true.
	}

	public void onInit(StaticSound sound)
	{
		// Configure the sound in here. Stuff like Distance Parameters go in here.
	}

	public void onFrame(StaticSound sound, Train train)
	{
		// This gets called each frame. If your sound pitch depends on the speed you can configure stuff in here.
	}

	public void play(StaticSound sound)
	{
		// This gets called when your sound starts playing. If you want your sound to fade in you can do that in here.
	}

	public void stop(StaticSound sound)
	{
		// This gets called when your sound stops playing. Fading out would go in here.
	}
}
```

#### Registering the Sound-Class

This happens inside `src/SoundHelper.nlvm`. You just need to call you're constructor inside an if clause that checks if there is a Section on the coaster that is named after your sound. Additionally set the **soundFound** boolean to true so the Script knows that a sound got activated.

```
if(coaster.getSection(Sections.BM_LIFT) != null)
{
	BMRoar roar = new BMRoar(sim, coaster);
	soundFound = true;
}
```

#### Done!

Your sound should now be in the game. To reload the Script ingame you don't have to reload the Park. Resetting the coaster inside the Control Panel **(F4)** also reloads the coaster.

Once your done create a **Pull Request** and I will look into it and most likely add it to the library.
