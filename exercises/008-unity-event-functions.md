# Exercises 008 Unity Event Functions

## Preparation

Read the slides on [Unity Event Functions](../slides/008-unity-event-functions.md) and [Console Intermediate: Enums](https://github.com/marczaku/csharp-intermediate/blob/main/slides/003.6-console-intermediate-1-enums.md)

## Set Up

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

## Enter and Leave Car (The Cheap Way)
- Create a new C# Script named `Vehicle`
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

## Clean-Up
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

## Distance-Check
- Check, whether the Player is actually close to the car before letting him Enter.
- Check the distance using `float Vector3.Distance(Vector3 a, Vector3 b);` and comparing the result using `<`
- Only call `EnterCar()`, if that distance is not bigger than a threshold that you pick yourself

## Use the Input Manager
- Configure a new Input named "Interact-Vehicle", check for `Input.GetButton("Interact-Vehicle")` instead of `Input.GetKey(KeyCode.F)`

## BONUS: Clean Up Responsibilities between Player and Car:
- The Player should have a Script to Try to Enter Vehicles, maybe name it `Driver`?
- The Vehicle should not check for the Player Entering the Car on `Update`, but instead have a public `Enter` and `Exit` Method.
- The Car needs to check for leaving the Vehicle in the `CarMovement`-Script.
- This one's a bit tricky and vague, but we'll look at it tomorrow! :)
