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

### Window Title

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
<img width="400" alt="image" src="https://media1.giphy.com/media/jNI34zkXlcOmLHVZsM/giphy.gif"> | Freely moved around by clicking on the title, keeping the mouse button down, and then dragging the Window anywhere you want.
--------------- | -----------------
<img width="400" alt="image" src="https://media0.giphy.com/media/LVAtcP3iwrRPuEmApG/giphy.gif"> | Maximized, if you need a little extra space, by rightclicking the window title and selecting Maximize, or by simply double-clicking the title. This can be undone the same way.
<img width="400" alt="image" src="https://media0.giphy.com/media/378qi05vuZvXn8KJyQ/giphy.gif"> | Closed and also Opened again using the Rightclick Context-Menu.
<img width="400" alt="image" src="https://media3.giphy.com/media/vMIMQZyejtB7zVuffc/giphy.gif"> | Undocked and Docked again. If you Undock  Window, it comes a freely floating Window that you can place anywhere on your Screen(s).
<img width="400" alt="image" src="https://media1.giphy.com/media/7Zi3dvgM6RukyTOKms/giphy.gif"> | Resized by Dragging the Window Frames.
<img width="400" alt="image" src="https://media3.giphy.com/media/vMIMQZyejtB7zVuffc/giphy.gif"> | Undocked and Docked again. If you Undock  Window, it comes a freely floating Window that you can place anywhere on your Screen(s).
<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137455112-82a9b967-3697-43c4-83d3-f5541ef5437a.png"> | You can find more Windows and more Window Functions in Unity's Menu-Bar's Window-Menu.
<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137456577-1dbba24b-96a8-48e8-8dcc-2bca0a6013ad.png"> | Very Useful: You can Use the Menu Window > Layouts to Save and Restore your Favorite Layouts. This is especially useful, if you have a few Layouts for different purposes (like Level-Editing, Debugging or Creating UIs), or if you messed up your Layout by Accident and want to reset it.

Next, we will take a look at the most important Windows and how they contribute to our Unity Project.

## Project

The Project Window is the heart of our Project. It contains an overview of all Assets and Packages.

<img width="585" alt="image" src="https://user-images.githubusercontent.com/7360266/137457495-74a349a6-42cc-473f-a8aa-66cf084b731b.png">

### Looks

First things first, I highly recommend making the Project-Window into its own docking field and switching it to One-Column-Layout. The reasons:
- Programmers are more interested in File-Names and File-Types
  - than in the looks of a File. 
- Large Icons cut off File Names
  - `Enemy_Zombie_Attack_Animation_1.png` becomes `Enem...on_1.png`
- There is new Space which will allow us to always keep an Eye on the Console.
  - The Console shows us Warnings and Errors, which we need to notice and fix ASAP!

If you ever find yourself searching for an Image whose File Name you don't know, you may temporarily switch to a Two-Column Layout again. But as a Programmer during most of your tasks, you won't need Large Icons, really.

### Assets

To explain, what the Assets-Folder shows, let's compare the Assets-Window of a small project:

<img width="471" alt="image" src="https://user-images.githubusercontent.com/7360266/137536146-43b9652e-d9c6-43e0-9eb4-9ea20b10aa8b.png">

With the contents of our Unity's Project-Folder. To find our Unity-Project-Folder, we can simply right-click on `Assets` and select `Reveal in Finder` (`Show in Explorer` on Windows) to open the Directory in our File-Explorer.

<img width="629" alt="image" src="https://user-images.githubusercontent.com/7360266/137536528-e358911f-b2cb-4304-bf64-1552df34f170.png">

Now, in this folder, you'll find exactly the same Folders and Files as in the Project-Window's Assets-Browser. Additionally, you'll find a lot of `.meta`-Files, which we will get to later.\
So basically, the Project-View shows us all the Files, named Assets, that are part of our Project. It Reflects the files that are on our Hard Drive as well.\
Therefore, this will be the Heart of any Project. Any Character, Environment, Soundtrack, Level and even Code will be found here!

### Packages

<img width="466" alt="image" src="https://user-images.githubusercontent.com/7360266/137536830-67b4731f-f794-4090-8312-1fa33a95e63b.png">

If you take a look at the Packages in the Project-Window, then you will find a lot of Content here. Unity has designed the Package Manager around the idea of being able to seamlessly integrate Plugins into the Engine without requiring complicated Installation Instructions.\
We will learn more about it at a later point, but if you're curios, you may take a look at the `Package Manager` that you find under the Window-Menu.

### What's Next?
If you look at what's inside your Assets in a new, fresh Project, you will only find one file in the Scenes-Folder: `SampleScene.unity`. That's a `Scene-Asset` and it can be opened through double-click. As we have found out under [Window Title](#window-title), the `SampleScene` is already open, so double-clicking won't really do anything right now.

But let's change that!

<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137539606-8aef4739-fb4f-408c-bd54-657187cb4dbc.png"> | Choose File > New Scene
------ | ------
<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137539691-e9ab66f8-2a92-4697-909f-fb24648a356b.png"> | Select `Basic 3D (Built-In)` and then click the `Create`-Button
<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137539876-9b023edb-d24f-4fbe-bbc8-053171b14b68.png"> | Now, your Window Title should start with `Unitled - SmallTheftAuto`. Let's change that! Use the Save-ShortCut (Mac: ⌘S, Windows: Ctrl+S), or Use the Menu: File > Save
<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137540191-e588163e-3f4b-464a-ba18-12160c1a4eac.png"> | Make sure to select the `Scenes`-Folder within in your `Assets`-Folder to keep all your Scenes in one place. And then give your Scene a name, like `SanAndreas`. Confirm by clicking the `Save`-Button.
<img width="400" alt="image" src="https://user-images.githubusercontent.com/7360266/137540391-c8a9514d-6cff-4870-aadb-c69fadb82756.png"> | If you did everything correctly, then your `Assets`-Folder should contain a `Scene`-Folder with two Scenes: `SampleScene` and `SanAndreas`
