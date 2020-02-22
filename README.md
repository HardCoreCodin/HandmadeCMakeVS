# Handmade Hero Visual Studio setup

A fully-featured Visual Studio setup for Handmade Hero using CMake (No MSBuild and no solution file) 

After following the instructions here: https://github.com/HandmadeHero/cpp

All you need are these 3 files:

- CMakeLists.txt
- CMakeSettings.json
- launch.vs.json

Put them in the top-most directory.
It is the one containing the "\build" directory. 
In Casey's example it would be just "w:\"

Then, just open the folder itself in visual studio (2019 or above).
There are multiple ways of doing this:
1. Just right-click in an empy area within the folder in File Explorer, and choode "Open in Visual Studio".
2. Open a new Visual Studio instance, and in the startup window click on "Open a Local Folder", then choose the directory.
3. In an already open Visual Studio instance, just go to: "File > Open > Folder", then choose the directory.

Wait a few seconds for Visual Studio to identify the files and generate a CMake cache and CMake targets from them.

Once it's done, you should already be able to run/debug any target.

You should be able to see all the targets in the solution explorer by clicking on "Switch View" and selecting "CMake Targets View".

If you haven't built it already, you can just do a full build by either going to: "Build > Build All", or by right-clicking on the "HandmadeHero Project" in the solution explorer and selecting "Build All"

You should then choose a Startup target, either by selecting "Game" from the "Select Startup Item" dropdown, or by right-clicking the "Game" target in the solution explorer and choosing "Set As Startup Item".

You can then launch the game by clicking on the startup button (which sould be set to "Game"). It launches in debug mode, so you can already put break-points etc.

To debug any other target, just select it in the "Select Startup Item" drop-down, and then click it. Break points should work as ususal in any target.

nJoy :)