# 004 Unity Introduction

Unity is a modular, cross-platform 3D Engine with integrations for Ads, Shops, Physics, Multiplayer, Analytics, Visual Scripting, AR, VR nd much more. In this course, you'll learn to make your first steps in the 3D Engine which can be a bit overwhelming in the beginning.

## Opening a Downloaded Unity Project

<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137399139-655f50d2-3f9e-4692-aae7-56ddd00d743f.png"> | After cloning your repository, you will find your Unity project under `projects/SmallTheftAuto`
------|--------
<img width="241" alt="image" src="https://user-images.githubusercontent.com/7360266/137400625-80de8120-b249-4818-8f51-5b82216713d8.png"> | You should Add this Project to your `Unity Hub` by first making sure, that the Projects-Tab is Selected.
<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137399650-db3f9ad2-c0ab-4927-8539-2d46ea5b58ce.png"> | And then using the Open Button.
<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137399682-026c873e-da9d-4fac-ba71-d81297ae67fd.png"> | And then navigating to the Unity-Project's Directory. Make sure to actually select the folder named `SmallTheftAuto` and not a folder inside or outside. Then click on the Open-Button.
<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137400484-caf3667c-1ec8-4b39-8131-e8cb6ce8918b.png"> | Now, you should find the `SmallTheftAuto`-Project in your Unity-Hub
<img width="59" alt="image" src="https://user-images.githubusercontent.com/7360266/137400758-e51a51fa-c94c-4af0-b5a3-5ef809ba35a2.png"> | It might show you a warning icon next to the Game. It should not be a big problem. Click on the Icon.
<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137400827-083508d4-d9e5-4e66-ad02-205661011c66.png"> | It will tell you, that you have not the exact same Editor Version installed as me. You can just click on `Choose another Editor version`
<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137400938-a7d04589-0162-4e32-af86-8d3754795bdc.png"> | And then choose an Editor that resembles the original Editor Version as much as possible. It should be at least `2021.1`, but anything further from that should be backwards-compatible.
<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137401214-57280991-54f0-4908-9267-cd12b2b050c7.png"> | You are ready to click on the Project-Name `SmallTheftAuto` now, in order to open the project in Unity.

## Editor Overview

You will be greated by this beautiful monster:

<img width="1523" alt="image" src="https://user-images.githubusercontent.com/7360266/137401428-2f181d28-ca4b-4870-9e3d-2ac7109f9bf7.png">

- Where to begin?

Actually, the title of the Window is a good start! Let's have a look:

<img width="741" alt="image" src="https://user-images.githubusercontent.com/7360266/137401573-99c5de93-95e6-4e1c-b9c1-3eb5d6067471.png">

That's a lot of information, but what does it mean?
- `SampleScene`: this is the name of the `Scene` that is currently opened. We will learn later, what a scene is, but for now, let's just say that this could be a Level or Menu of our Game. `SampleScene` is an empty `Scene` with a `Camera` that comes with every new Unity-Project.
- `SmallTheftAuto`: This is our Project's Name, or, to be more specific, it's just our Project's Directory-Name, really, but these two are usually the same.
- `PC, Mac & Linux Standalone`: This is the platform that the Editor is currently simulating. Unity is a Cross-Platform Engine and other possible platforms include `iOS`, `Android`, `Nintendo Switch` and many more.
- `Unity 2021.1.24f1`: That's the Unity Editor Version that we are currently using.
  - `2021`: is the Major Version, it is updated every year and comes often with great new Technologies, sometimes at the cost of old technologies, which need to then to be replaced in our projects. That's why companies usually hesitate too upgrade their existing projects to a new Major Version.
  - `1`: is the Minor Version, it is usually updated 2-3 times per Major Version and shows that some big new Features have been introduced and that Upgrading should be throroughly considered and carefully tested.
  - `24f1` is the Patch Version. Patches are released for all Editor Versions regularly and it is usually recommended to regularly integrate them. They come with Bugfixes and Performance Improvements as well as new Compatibilities for e.g. new Phones or OS Versions.
- `Personal (Personal)`: This means, that I have activated a (free) Personal License due to not using Unity in a professional capacity on this Work Station. You can purchase Subscriptions with Unity to activate additional features like being able to remove the "Made with Unity"-Splashscreen from your Application when publishing.
- `<Metal>` is Unity's Default Graphics API for Apple Devices and selected here due to me working on Mac OS.

## Dockable Window System

Unity's Editor comes with a Dockable Window System. Every Window can be:
<img width="400" alt="image" src="https://media1.giphy.com/media/jNI34zkXlcOmLHVZsM/giphy.gif?cid=790b76116c86f582f2ade70a35fd080a30471793a8bd1866&rid=giphy.gif&ct=g"> | Freely moved around by clicking on the title, keeping the mouse button down, and then dragging the Window anywhere you want.
--------------- | -----------------
<img width="400" alt="image" src="https://media0.giphy.com/media/LVAtcP3iwrRPuEmApG/giphy.gif?cid=790b76113f1e0c7cf3cd871025dcfc7cce39417a4573dfff&rid=giphy.gif&ct=g"> | Maximized, if you need a little extra space, by rightclicking the window title and selecting Maximize, or by simply double-clicking the title. This can be undone the same way.
