![Nolimits 2 Sounds](https://i.imgur.com/oy2bPhU.png)

This is a free-to-use and open-source collection of scripted sounds for coasters,
including custom lift sounds and various roars. &mdash; **[Download](https://github.com/Emonadeo/Nolimits2Sounds/releases/latest)**

## Sound List

| Name | Description | Source | Contributor |
| ---- | ----------- | ------ | ----------- |
| `bm_lift` | B&M Lift Sound | Silver Star @ Europapark | |
| `bm_roar`| B&M Roar Sound | | [TheCodeMaster](https://nolimitscentral.com/account/thecodemaster) |
| `gci_lift` | GCI Lift Sound | Wood Coaster @ Knight Valley | |
| `rmc_lift` | RMC Lift Sound | Twisted Timbers @ Kings Island | |
| `jet_funky_lift`| Funky Lift Sound for sketchy Jet Co**r**sters | Chupy Coaster @ Washuzan Highland | [Jetcoasterfan](https://nolimitscentral.com/account/jetcoasterfan) |
| `jet_roar` | Jet Coaster Roar Sound | Hurry Coaster @ Kobe Fruit and Flower Park | [Jetcoasterfan](https://nolimitscentral.com/account/jetcoasterfan) |
| `misc_roar` | Roar suitable for most steel coaster types | | [TheCodeMaster](https://nolimitscentral.com/account/thecodemaster) |
| `misc_clunky_lift` | Clunky Sound for old and slow Lifts | Speed @ Oakwood | |
| `misc_old_lift` | Vintage Sound for normal and fast Lifts. Similar to the existing Sound | Speed @ Oakwood | |
| `misc_lsm` | General Purpose LSM Sound | | [bestdani](https://nolimitscentral.com/account/bestdani) |

# Usage Guide

## Setup

1. **[Download](https://github.com/Emonadeo/Nolimits2Sounds/releases/latest) the latest release and unpack it into your park directory.**

2. **Add the Script to your coaster.**
    > **Note:** It is not needed to change the coaster's operation mode to scripted.

![Guide](https://i.imgur.com/WnXCoxe.png)

## Assigning Sounds

![Guide](https://i.imgur.com/WRDD4jL.png)

1. **Select the section you want to add a sound to** (Can be a lift, station or block brake)

2. **Open Type Settings** (Or double-click the section directly)

3. **Rename the section to the sound's name** *(See example below)*  
    > **Multiple Sections:** You can't give two sections the same name. If you want to use the same sound on multiple sections, append the name by an underscore (`_`) and a number counting upwards starting from 1.  
    **Roar Sounds**: If you want to add a roar sound, you can name any unused section *(i.e. station, lift or block brake)*. Roar sounds are always played and not limited to that section.

4. **Done**

## Controlling Sounds

Currently a sound keeps playing until the very last car of a train exits the section.
By adding a **trigger** to your track, you can define when a sound should cut off as a train enters the trigger.
Some sounds even play an additional audio clip, e.g. the chain dog detaching from the chain.

![Guide](https://i.imgur.com/LuGlHVQ.png)

1. **Select the _"Add Trigger"_ tool**

2. **Add a trigger to the desired cutoff position**
    > **Note:** The sound ends as soon as the first car enters the trigger.
    Also consider that some sounds play an additional audio clip instead of being cut off immediatly.
    
3. **Name the trigger identical to the corresponding section + `_end`**

## Example

![Example](https://i.imgur.com/hpNbTk6.png)


# Creating your own Sounds

> **Note:** Creating sounds requires basic knowledge of scripting

See Instructions on how to make your own Sounds on the [Wiki](https://github.com/Emonadeo/Nolimits2Sounds/wiki)
