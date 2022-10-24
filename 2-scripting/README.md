# Exercises 2 - Scripting

## 1.1 - Components

### Goal
We'll make a small Jenga Tower Game.

### Slides
Need Help? [Here's The Slides!](slides/README.md#1-components)

### Set Up

- Create a new Scene (`Basic 3D (Built-In)`)
  - Now, Save the Scene (Ctrl+S)
  - Put it in `Assets/Scenes`
  - And name it `Jenga`
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
  - Then `Pointer Click`
  - Click on the Small `+` Button to connect a new event target
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
- Let's validate, that Rider is configured as the IDE of your choice:
  - Open Edit > Preferences
  - Open the External Tools Tab
  - Instead of `Open by File Extension`, select `Rider`
  - This ensures, that Auto-Complete etc. works

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
  - Select each instance of the `Brick` GameObject and on the Event Trigger Component and for each:
    - Click the small + Button under Pointer Click
    - Drag&Drop the `Brick Counter` GameObject as a Target
    - Select BrickCounter > CountBrick as the Function to Call
- Start the Play Mode, make sure the Console Window is open and observe the Console output when clicking on Bricks! :)

## 2.1 - Event Functions (Collision)

### Goal
Right now, the game can't detect when the tower has crashed (which means that the player has lost). Let's fix that!

We'll add a Dead Zone underneath the plane which holds the tower and make it a lot larger than the plane.

If anything collides with that Dead Zone, it must mean, that a Brick crashed.

### Slides
Need Help? [Here's The Slides!](slides/README.md#2-event-functions)

### Set Up
- Create a GameObject -> 3D Object -> Plane
  - Place it at Position (0, -0.1, 0)
    - This is slightly below the ground plane
  - Change its scale to (3, 1, 3)
  - Name it Dead Zone

### Instructions
- In the Project View, Create -> C# Script
  - Name it DeadZone
  - No WhiteSpaces in the name!
  - Open the Script
- Implement the Collision Detection Unity Event Method on your Script
  - Find the correct signature (return type, method name, parameters) on the slides for On Collision Enter (not 2D!)
  - Within the Method body, which only gets invoked as soon as any Object as collided with the Dead Zone:
    - Invoke `Debug.Log` and pass `"The tower collapsed. You lose."`
    - Add the following expression for restarting the Scene: `SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);`
- Don't forget to attach the `DeadZone` Script to our `Dead Zone` GameObject!
- Test it!

## 2.2 - Event Functions (Start)

### Goal
That tower is way too tidy! Let's make a script that makes our bricks less perfectly shaped and less perfectly positioned :)

### Slides
Need Help? [Here's The Slides!](slides/README.md#2-event-functions)

### Instructions
- In the Project View, Create -> C# Script
  - Name it Brick
  - Open the Script
- Implement the Start Unity Event Method in your Script
  - Find the correct signature (return type, method name, parameters) on the slides for Start
  - Within the Method body, which only gets invoked once when the Game Starts:
    - The Property `Transform transform {get;}` allows us to access our `Transform` Component, which holds our position.
    - `Transform` has a Property `Vector3 position {get;set;}` with which we can change the `x`, `y` and `z` coordinates of our position.
    - We can use the `+=` operator to change the position in a certain direction.
    - `Transform` also has a Property `Vector3 right {get;}` which returns the direction of the Transform's right side (which is always different depending on the GameObject's rotation)
    - If we would add the whole `right` Vector to our `position`, each brick would move a whole Unity Unit in distance (remember, a Brick is 3 Units long and 1 wide, so that'd be way too much)
    - We also want them all to have some random offsets, sometimes in one, sometimes in the other direction
    - So we can multiply the `right` Vector with a random `float`:
    - `transform.right * Random.Range(-.1f,.1f)`
    - This way, each Brick will receive a random offset. Sometimes .1 to the left side, sometimes .1 to the right side, sometimes 0.
- Don't forget to attach the `Brick` Script to your `Brick` Prefab. Not on one or each of the Game Objects in the Scene, but on the Prefab in your Project Window.
- Test it!


## 2.3 - Event Functions (Drag)

### Instructions

- Make the `Brick` class implement the interfaces
  - `IBeginDragHandler`: When the user starts Dragging, we will tell the RigidBody attached to our GameObject to stop being simulated and be kinematic instead, since we want to control the physics from our code.
  - `IDragHandler`: When the user continues Dragging, we will update our position depending on the user's drag direction.
  - `IEndDragHandler`: When the user stops Dragging, we will tell the RigidBody to stop being kinematic and be simulated again instead.

The coding part is a little too technical and mathematical for my taste at this point, so here's the working code:

```cs
    public void OnDrag(PointerEventData eventData)
    {
        Plane plane = new Plane(Vector3.up, Vector3.up * eventData.pointerPressRaycast.worldPosition.y);
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        if (plane.Raycast(ray, out float distance))
        {
            transform.position = ray.GetPoint(distance);
        }
    }

    public void OnBeginDrag(PointerEventData eventData)
    {
        GetComponent<Rigidbody>().isKinematic = true;
    }

    public void OnEndDrag(PointerEventData eventData)
    {
        GetComponent<Rigidbody>().isKinematic = false;
    }
```

Remove the `EventTrigger` Component from the `Brick` Prefab in the Project View, so the user can't click on the Bricks to remove them anymore.

Test it! :)

Note: What Feature of the game broke now? Can you fix it? Do you face additional problems?

## 2.4 - Event Functions (Update)

### Goal
We will begin with a Small Theft Auto Prototype and implement Movement forward and backward and rotation to the left and right.

### SetUp

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

IGNORE THE EXERCISE BELOW, THEY WILL BE MOVED TO LATER!

## Done?
Return to the [Overview](../../../#3-serialization)