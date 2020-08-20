# Troubleshooting

If you have trouble getting SL mods to work, this page should help you diagnose most problems.

## Check these things first

Before you dive too far into troubleshooting, you should make sure that:
1. You are on the [latest release of SideLoader](https://github.com/sinai-dev/Outward-SideLoader/releases)
2. You have the latest version of Outward, and BepInEx.
3. You have checked [Outward Wiki: Troubleshooting Mods](https://outward.gamepedia.com/Installing_Mods#Troubleshooting) as well.

## Error logging

SideLoader has extensive logging, and you should be able to tell if something major has gone wrong from the log file.

The log file can be found at `Outward\output_log.txt`, assuming you are on BepInEx 5.2 or later.

In the log file, Ctrl+F for "SideLoader" and start from there. Look for your mod and see if SideLoader said anything about it, which give you a clue as to what went wrong.

If you don't see anything that looks bad in the SideLoader setup but you find your custom asset is still bugged, you can reproduce the bug in-game and then immediately quit the game, then check the log file. At the very bottom of the file, you may find some logs related to the error.

## My Item/Skill isn't in the F1/F3 menu!

If you're making a custom Item or Skill and you want to spawn it from the related debug menu, but can't see it anywhere:

* For items, make sure `IsPickable` is `true`
* For skills, make sure the `New_ItemID` is <b>not</b> between `8300000` and `8399999` (anything above or below is fine).

Other than that, if you can't see your item or skill then it probably had an error on setup.

## There's no error logging but something isn't right!

Contact me in the [Outward Discord](https://discord.gg/outward) and let me know what's going on.