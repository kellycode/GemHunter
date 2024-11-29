20241127: Fixed the layout problems but after testing for a while I simply don't find this game interesting.  Interactions are clumsy
# GEM HUNTER TEST
source: https://assetstore.unity.com/packages/essentials/tutorial-projects/gem-hunter-match-2d-sample-project-278941#description

Doesn't Scale Properly in WebGL (Fixed - see below)

These are the steps

Loaded the project directly into Unity from the asset store page.

Changed player screen size to 1920 x 1080

Note: If I change the player screen size to a standard phone portrait aspect ratio, it works.  In landscape it doesn't

Changed project build profile to WebGL

Build and Run

This is the result in WebGL (Same result with config.matchWebGLToCanvasSize true or false)

![WebGL Result](https://github.com/kellycode/GemHunter/blob/main/GemHunter_WebGL.jpg)

Same result in Chrome, Edge and Opera browsers

In the Unity Editor it also doesn't scale properly running in the Game Tab and this is also what it looks like as a Windows 11 exe build

![Editor Result](https://github.com/kellycode/GemHunter/blob/main/GemHunter_Editor.jpg)


I found a fix:

In GameManager.cs, in ComputeCamera() it makes an assumption about screen size...

line ~204

(hard coded assumption of sizes)  
//On Wide screen, we fit vertically  
halfSize = (bounds.size.y + 3) * 0.5f + LevelData.Instance.BorderMargin;

I replaced that with a reverse of the portrait mode calculation above...

float screenRatio = Screen.width / (float)Screen.height;  
halfSize = ((bounds.size.x + 1) * 0.5f + LevelData.Instance.BorderMargin) * screenRatio;

That fixed the layout on both my windows exe and WebGL requirements


