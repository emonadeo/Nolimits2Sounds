![Nolimits 2 Sounds](https://i.imgur.com/51oxkSC.png)

This is an OpenSource Project for rollercoaster related Sounds to add to the realism of NoLimits 2

## How to use

1. [Download](https://github.com/Emonadeo/Nolimits2Sounds/releases/latest) the latest release and copy it into your park directory.

2. Add the Script to a coaster. **Note:** It is not needed to change the coaster's operation mode to scripted.
> *Coaster Properties* `->` *Add from File* `->` *Sounds.nl2script*

3. You can now add any sound by naming sections. To support having a sound on multiple sections every section-name is appended by an underscore and a number starting at 1 going up with each additional section: `<section_name>_<number>`<br><br>
Let's say we have a Mine Train coaster that has 3 Lifthills and we want each lift-hill to have the `misc_old_lift` sound. In this case we name the first lift-hill `misc_old_lift_1`, the second `misc_old_lift_2` and so on.<br><br>
**Note:** Global sounds like `bm_roar` also have to be appended by `_1` due to the way this script works, even though you'd only add it once per coaster anyways.

4. Switch to Play Mode and enjoy :)

## Soundlist

| Name               | Description                                                                | Global | Source                       | Contributor |
| ------------------ | -------------------------------------------------------------------------- | ------ | ---------------------------- | ----------- |
| `bm_lift`          | B&M Lifthill Sound                                                         | No     | Silver Star @ Europapark     | |
| `bm_roar`          | B&M Roar Sound                                                             | Yes    |                              | [TheCodeMaster](https://nolimitscentral.com/account/thecodemaster) |
| `gci_lift`         | GCI Lifthill Sound                                                         | No     | Wood Coaster @ Knight Valley | |
| `gci_lift`         | GCI Lifthill Sound                                                         | No     | Wood Coaster @ Knight Valley | |
| `rmc_lift`         | RMC Lifthill Sound                                                         | No     | Twisted Timbers @ Kings Island | |
| `jet_funky_lift`   | Funky Lifthill Sound for sketchy Jet Co**r**sters                          | No     | Chupy Coaster @ Washuzan Highland | [Jetcoasterfan](https://nolimitscentral.com/account/jetcoasterfan) |
| `jet_roar`         | Jet Coaster Roar Sound                                                     | Yes    | Hurry Coaster @ Kobe Fruit and Flower Park | [Jetcoasterfan](https://nolimitscentral.com/account/jetcoasterfan) |
| `misc_roar`        | Roar suitable for most steel coaster types                                 | Yes    |                              | [TheCodeMaster](https://nolimitscentral.com/account/thecodemaster) |
| `misc_clunky_lift` | Clunky Sound for slow Lifthills                                            | No     | Speed @ Oakwood              | |
| `misc_old_lift`    | Vintage Sound for normal and fast Lifthills. Similar to the existing Sound | No     | Speed @ Oakwood              | |
| `misc_lsm`         | General Purpose LSM Sound as found on Premier Rides Launch Coasters        | No     |                              | [bestdani](https://nolimitscentral.com/account/bestdani) |

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

#### 2. NoLimits Central

> This is the new place after Nolimits-Exchange shut down

There is alternatively a discussion thread on NoLimits Central ([Link](https://nolimitscentral.com/forum/topic/49)). Simply post a new comment providing the information named above and we will be sure to check it out.

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

#### Using `MultiSection`

Since Section Names in NoLimits 2 are unique you still might want to have your sound be applied to multiple sections. For this Reason I've provided a **MultiSection** object which your Sound-Class automatically implements. Access it any time by calling `this.multisection`. Currently the MultiSection-Class has the following functions:

| Function                                            | Returns               | Description |
| --------------------------------------------------- | --------------------- | ----------- |
| `isTrainOnMultiSection(Train train)`                | bool                  | Returns true if the specified train is on any of the Sections the Sound is bound to |
| `getSections()`                                     | Section[]             | Returns all Sections the Sound is bound to in an Array |
| `getFromCoaster(Coaster coaster, String sectionId)` | *static* MultiSection | Returns a MultiSection that includes all Sections of a coaster named after `sectionId` |

#### Registering the Sound-Class

This happens inside `src/SoundHelper.nlvm`. You just need to call your constructor inside an if clause that checks if there is a Section on the coaster that is named after your sound. Additionally set the **soundFound** boolean to true so the Script knows that a sound got activated.

```
if(hasSound(coaster, Sections.BM_ROAR))
{
	BMRoar roar = new BMRoar(sim, coaster);
	soundFound = true;
}
```

#### Done!

Your sound should now be in the game. To reload the Script ingame you don't have to reload the Park. Resetting the coaster inside the Control Panel **(F4)** also reloads the coaster.

Once your done create a **Pull Request** and I will look into it and most likely add it to the library.
