# Replacing Audio

Replacing Audio with SideLoader is fairly simple, the only tedious part is finding the correct name for replacement.

All you have to do is use the same name that the game uses (listed below) and SideLoader will find and replace the original audio with your file.

Your .wav files must be placed in the `AudioClip\` sub-folder of your [SL Pack](Basics/SLPacks).

For example, `Outward\BepInEx\plugins\MyPack\SideLoader\AudioClip\BGM_GeneralTitleScreen.wav` would replace the title screen music.

## Audio Clip Names

You can see a full list of audio clip names [here](API/Enums/Sounds.md).

The names start with a prefix which is used as a category for the sound, you can use this to find the type of sound you are interested in:
* `BGM_` is background music
* `ENV_` is environment sounds
* `SFX_` is sound effects
* `FT_` is foot (walking) sounds
* `LOC_` is for localization (dialogue / etc)
* `CS_` is combat sounds
* `UI_` is for user interface sounds

## Example

1. Pick any song you want to use and make sure it is a `.wav` file (there are many free online converters for this if you need one).
2. Create a Test SL Pack (`Outward\BepInEx\plugins\Test\SideLoader\`), and create a `AudioClip\` sub-folder inside it. Put your `.wav` file inside the `AudioClip\` sub-folder (for example, `Test\AudioClip\MySong.wav`)
3. Rename your audio clip so that it matches the name of an existing audio clip. For this example, we will use the title screen music. Rename your audio clip to `BGM_GeneralTitleScreen.wav`.
4. Start the game, and you should now hear your audio clip.