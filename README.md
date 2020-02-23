# Handmade Hero CMake / Visual-Studio setup

A fully-featured CMake build setup for Handmade Hero (No MSBuild and no solution file).<br>
This is for people who have the source code but want to use CMake and/or Visual Studio (instead of a text editor).<br>
There's no changes needed to the codebase whatsoever and it doesnt modify anything so you can still have it in a repo and just pull each day there's an update.<br>

Instructions for how to set-up the repo for building are in the source code's folder in the readme.txt file.<br>
If you have linked your git-hub account to the original repo you can also see them here:
https://github.com/HandmadeHero/cpp

Once that's all set-up, all you need on top of that are these 3 files:

- CMakeLists.txt
- CMakeSettings.json
- launch.vs.json

Note: 
Only the first file is actually needed (the others are just the result of doing a few more steps in VS).<br>
Because it's just a CMake file it can be used from a command-line without ever opening visual studio at all.<br>
It can also be extended to be cross-platform and usable outside of VS to compile in Linux or MacOS.

Put the files in the top-most directory.<br>
It is the one containing the \\build directory. <br>
In Casey's example it would be just w:\\

Then, just open the folder itself in visual studio (2019 or above).<br>
There are multiple ways of doing this:<br>
1. Just right-click in an empty area within the folder in File Explorer and choode "Open in Visual Studio".
2. Open a new Visual Studio instance, and in the startup window click on "Open a Local Folder", then choose the directory.
3. In an already open Visual Studio instance just go to: "File > Open > Folder" then choose the directory.

Wait a few seconds for Visual Studio to identify the files and generate a CMake cache and CMake targets from them.<br>
Once it's done, you should already be able to run/debug any target.<br>
You should be able to see all the targets in the solution explorer by clicking on "Switch View" and selecting "CMake Targets View".

![alt text](https://github.com/ArnonMarcus/HandmadeCMakeVS/blob/master/HandmadeVisualStudioTargets.png?raw=true)

If you haven't built it already, you can just do a full build by either going to: "Build > Build All" <br>
or by right-clicking on the "HandmadeHero Project" in the solution explorer and selecting "Build All".

![alt text](https://github.com/ArnonMarcus/HandmadeCMakeVS/blob/master/HandmadeVisualStudioBuild.png?raw=true)

You should then choose a Startup target, either by selecting "Game" from the "Select Startup Item" dropdown <br>
or by right-clicking the "Game" target in the solution explorer and choosing "Set As Startup Item".

You can then launch the game by clicking on the startup button (which sould be set to "Game"). <br>
It launches in debug mode so you can already put break-points etc.

To debug any other target just select it in the "Select Startup Item" drop-down and then click it. <br>
Break points should work as us usal in any target.

nJoy :)