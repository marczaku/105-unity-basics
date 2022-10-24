# Exercises 2 - Scripting

## 1.1 - Components

### Goal
We'll make a small Jenga Tower Game.

### Slides
Need Help? [Here's The Slides!](slides/README.md#1-components)

### Set Up

- Create a new Scene (`Basic 3D (Built-In)`) and name it Jenga
- Create a GameObject -> 3D Object -> Plane
  - Place it at Position (0, 0, 0)
  - Change its scale to (0.6, 1, 0.6)
  - Name it Ground
- Create a GameObject -> 3D Object -> Cube
  - Place it at Position (0, 0.2, 0)
  - Change its scale to (3, 0.4, 1)
  - Name it Brick
- Create a Prefab from this Brick
- Now, instantiate this prefab 2 more times
  - Place the first instance at (0, 0.2, -1)
  - Place the third instance at (0, 0.2, 1)
- We have one row of Jenga Bricks now
- Build 10 more rows of Bricks by changing the y-position and by rotating each even row by 90 degrees on the Y-Axis

### Instructions

We want to be able to remove Jenga Bricks by clicking on them.
- Add a Physics RayCaster Component to the Main Camera Game Object
  - This component will track for mouse input on objects with 3D Colliders
- Create a new GameObject > UI > Event System
  - This class is needed to actually track mouse events like moving, clicking, hovering etc.
- Open the Brick Prefab. Then in the Prefab:
  - Add a Event Trigger Component
  - Select Add New Event Type
  - Then Pointer Click
  - Set the Brick GameObject itself as a Target (it says `None (Object)`)
  - Set the Function to GameObject > SetActive (it says `No Function`)
    - The flag is set to `false` automatically
    - This will disable the GameObject when it's clicked on
- Test it! Leave Prefab Editing Mode and start Play Mode!

Next, we want our Bricks to fall when they're not resting on other bricks.
- Open the Brick Prefab. Then in the Prefab:
  - Add a Rigid Body Component
- Test it! Leave Prefab Editing Mode and start Play Mode!

## 1.2 Custom Component

### Goal
We'll add a counter to Jenga which detects, how many bricks we have removed so far

### Slides
Need Help? [Here's The Slides!](slides/README.md#1-components)

### Set Up
- Create a GameObject > Create Empty
  - Name it Brick Counter

### Instructions
- In the Project View, Create -> C# Script
  - Name it BrickCounter
  - No WhiteSpaces in the name!
  - Open the Script by Double-Clicking
  - Rider should open up now
- In Rider, we'll get to Code:
  - You'll find a class named `BrickCounter` which inherits from `MonoBehaviour`
  - Remove both methods and comments that you find in the class
    - `Start` and `Update`
  - Add a private field of type `int` named `brickCount`
  - Add a public method
    - return type `void`
    - no parameters
    - named `CountBrick`
  - in the method's body:
    - increment `brickCount`
    - Use the static method `Log` in class `Debug` and pass the interpolated string `$"Total Bricks Removed: {brickCount}` as an argument.
- Next, we need to add the script to our Game Scene:
  - Drag&Drop the `BrickCounter`-Script onto the `Brick Counter` GameObject in the scene
- And now, we need to make sure that the `CountBrick`-Method is invoked when a brick is clicked:
  - You can't reference the `Brick Counter` directly to the `Brick` Prefab though
    - That is, because in a Prefab we can't reference objects which are part of a scene
    - That is, because the prefab could be used in entirely different scenes
  - Instead, we need to reference it in the actual instances of the `Brick` in our Game Scene
  - Select all instances of the `Brick` GameObject and on the Event Trigger Component:
    - Click the small + Button under Pointer Click
    - Drag&Drop the `Brick Counter` GameObject as a Target
    - Select BrickCounter > CountBrick as the Function to Call
- Start the Play Mode, make sure the Console Window is open and observe the Console output when clicking on Bricks! :)


## 2 - Event Functions

### Goal
Implement Movement forward and backward and rotation to the left and right.

### Slides
Need Help? [Here's The Slides!](slides/README.md#1-components)

### Set Up

<img width="156" alt="image" src="https://user-images.githubusercontent.com/7360266/137805960-04b2da02-5785-4fdd-aa49-09cdcf195053.png">     <img width="90" alt="image" src="https://user-images.githubusercontent.com/7360266/137805943-bf9c3476-21bb-4d3c-a551-e1fc7c1e0e22.png">   

- Add a Directional Light to the Scene
- Create a GameObject for the Player
  - I have built one using an Empty Parent GameObject named Player
  - A 3D-Cube as a child that's scaled on the X-Axis, named Body
  - A 3D-Sphere as a child that's scaled on all Axes and slightly moved up on the Y-Axis, named Head
- Create a new C# Script named `PlayerMovement`
- Attach the Script to the Player-GameObject
- Start the Play Mode and notice, that nothing is happening, yet

### Move Forward
- Modify the `PlayerMovement`-Class's `Update`-Method to look like this:

```cs
void Update() {
    // only, if the W-Key is currently pressed...
    if (Input.GetKey(KeyCode.W)) {
        // translate the player's transform-component on the y-axis (which points up)
        transform.Translate(0f, 0.01f, 0f);
    }
}
```

- `Update` is called every Frame. Which means that on good computers, it might be called 54 times per second (54 FPS = 54 Frames per Second) and on worse ones only 31 times per second.
- `Input` is a `class` Unity provides for Common Input-Handling functionality. It is technically not `static`, but all Fields, Properties and Methods are, so it kind of is, too.
- `static bool GetKey(KeyCode keyCode)` is a static method that returns `true`, if the `KeyCode`, which is passed as an argument, is currently pressed.
- `transform` is a `Property` on every `MonoBehaviour`-Class that accesses the `Transform`-Component from the same `GameObject` that the `MonoBehaviour` is attched to.
- `void Translate(float x, float y, float z)` is the easiest method for moving a `Transform` into any direction. It resembles teleporting the `Transform`, which can lead to weird, glitchy behavior like moving through a very thin wall, if used carelessly.

- Start the Game to see, that the `Player` now walks up towards the top of the Screen.

### Backwards Movement
Make the Player walk backwards, when `S` is pressed
        
### Turn Left
Make the Player turn left, when `A` is pressed

`transform.Rotate(float xAngle, float yAngle, float zAngle);`

### Turn Right
Make the Player turn right, when `D` is pressed
        
### Update-Problem
What problem exists again with `Update`, Movement and Frame-Rate (FPS)?\
Solve it!
        
### Using the Input Manager
Use `float Input.GetAxis(string name);` instead of `bool Input.GetKey(KeyCode keyCode);`

---

IGNORE THE EXERCISES BELOW, THEY WILL BE MOVED TO LATER!

## 2 - Event Functions

### Slides
Need Help? [Here's The Slides!](slides/README.md#2-event-functions)

You might also need: [Console Intermediate: Enums](https://github.com/marczaku/csharp-intermediate/blob/main/slides/003.6-console-intermediate-1-enums.md)

### Set Up

- Create a GameObject for the Car
  - I have built one using an Empty Parent GameObject named Car
  - A 3D-Cube as a child that's scaled on the X-Axis, named Body
  - 4 3D-Cubes as children, named Wheel
- Create a new C# Script named `CarMovement`
- Attach the Script to the Car-GameObject
- Add the same Code from the `PlayerMovement` to the Car
  - This is not realistic Car Physics, but it'll do the job for now!
- Start the Play Mode and notice, that both the Player and the Car move at the same Time now
- Disable the `CarMovement`-Component on the Car-GameObject
- Start the Play Mode and notice, that now, only the Player moves

### Enter and Leave Car (The Cheap Way)
- Create a new C# Script named `Vehicle` and add it to the `Car`-GameObject.
- Add a `public` field of type `GameObject` named `player` to the Script. Reference the `Player`-GameObject in the Scene.
- Add a `public` field of type `CarMovement` named `carMovement` to the Script. Reference the `CarMovement`-GameObject in the Scene.
- In `Update`, check, whether the `F` key is being Pressed.
- If so, check, whether the `player`-GameObject's `isActiveInHierarchy`-Property is `true`
  - If `true`, then the `player` is not in the Vehicle and we should let him "Enter":
    - Set the `player`-GameObject inactive by invoking its `SetActive` method with the value `false` to Hide the Player.
    - Set the `carMovement`-Field's `enabled`-Property to `true` to enable Car Controls.
  - If `false`, then the `player` is currently in the Vehicle and we should let him "Exit":
    - Assign the `Vehicle`-Script's own `transform`-Property's `position`-Property's value to the `player`-GameObject's `transform`-Property's `position`-Property to the.
    - Set the `player`-GameObject active by invoking its `SetActive` method with the value `true` to Show the Player.
    - Set the `carMovement`-Field's `enabled`-Property to `false` to disable Car Controls.
- Test your code, whether it works. If not, then you probably did one of the steps wrong.

### Clean-Up
- Move the Logic for Checking, whether the Player is in a Car, for Entering and Leaving the Car to new Methods, to clean up your Code.
- Update should look like this:
```cs
void Update() {
   if(PlayerIsInCar()) {
      LeaveCar();
   } else {
      EnterCar();
   }
}
```

### Distance-Check
- Check, whether the Player is actually close to the car before letting him Enter.
- Check the distance using `float Vector3.Distance(Vector3 a, Vector3 b);` and comparing the result using `<`
- Only call `EnterCar()`, if that distance is not bigger than a threshold that you pick yourself

### Use the Input Manager
- Configure a new Input named "Interact-Vehicle", check for `Input.GetButton("Interact-Vehicle")` instead of `Input.GetKey(KeyCode.F)`

### BONUS: Clean Up Responsibilities between Player and Car:
- The Player should have a Script to Try to Enter Vehicles, maybe name it `Driver`?
- The Vehicle should not check for the Player Entering the Car on `Update`, but instead have a public `Enter` and `Exit` Method.
- The Car needs to check for leaving the Vehicle in the `CarMovement`-Script.
- This one's a bit tricky and vague, but we'll look at it tomorrow! :)


## Done?
Return to the [Overview](../../../#3-serialization)