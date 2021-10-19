# Assignments 007 Unity Components

## Preparation

Read the slides on [Unity Components](../slides/007-unity-components.md)

## Set Up

<img width="156" alt="image" src="https://user-images.githubusercontent.com/7360266/137805960-04b2da02-5785-4fdd-aa49-09cdcf195053.png">     <img width="90" alt="image" src="https://user-images.githubusercontent.com/7360266/137805943-bf9c3476-21bb-4d3c-a551-e1fc7c1e0e22.png">   

- Add a Directional Light to the Scene
- Create a GameObject for the Player
  - I have built one using an Emoty Parent GameObject named Player
  - A 3D-Cube as a child that's scaled on the X-Axis, named Body
  - A 3D-Sphere as a child that's scaled on all Axes and slightly moved up on the Y-Axis, named Head
- Create a new C# Script named `PlayerMovement`
- Attach the Script to the Player-GameObject
- Start the Play Mode and notice, that nothing is happening, yet

## Move Forward
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

## Backwards Movement
Make the Player walk backwards, when `S` is pressed
        
## Turn Left
Make the Player turn left, when `A` is pressed

`transform.Rotate(float xAngle, float yAngle, float zAngle);`

## Turn Right
Make the Player turn right, when `D` is pressed
        
## Update-Problem
What problem exists again with `Update`, Movement and Frame-Rate (FPS)?\
Solve it!
        
## Using the Input Manager
Use `float Input.GetAxis(string name);` instead of `bool Input.GetKey(KeyCode keyCode);`
