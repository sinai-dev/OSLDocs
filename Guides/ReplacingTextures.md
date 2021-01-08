# Replacing Textures

Replacing textures is probably the easiest part of using the SideLoader. All you have to do is place the texture .png file in the `Texture2D\` folder of your [SL Pack](SL-Packs).

Importantly, your textures must use <b>the exact same name</b> that the game uses. This is how the SideLoader identifies which textures to replace.

## Items and Status Effects

For Items and Status Effects, you can replace these items and textures using a Custom Item or Custom Status template.

See the related articles for more details.

## Finding Textures

To get textures to replace as well as the names of those textures, use [uTiny Ripper](https://sourceforge.net/projects/utinyripper/files).

1. Download uTiny Ripper and run the "uTinyRipper.exe" file. 
2. Drag a file (such as `Outward_Data\StreamingAssets\item_visuals` file) or the entire "Outward/" folder onto the uTiny Ripper GUI.
3. Wait for uTiny to finish reading the assets.
4. Unpack and Save the Assets to a folder of your choice.

This may take a while. When it's done, open the generated folder and look in the "Texture2D" folder. Most of the game's textures can be found inside here, simply copy+paste them into your Texture2D folder and make changes as desired.

You can also use AssetStudio GUI to unpack individual textures, although I can't guarantee it will export them with the correct names, make sure the file name matches what you see in the GUI.