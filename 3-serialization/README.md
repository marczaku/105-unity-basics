# Exercises 3 - Serialization

## 3. - Serialization

### Goal

We want to have the ability to leave and enter a car. Therefore, we will:
- Create a Car GameObject in the Scene.
- Add a Script to that Car.

### Slides
Need Help? [Here's The Slides!](slides/README.md#3-serialization)

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
- In `Update`, check, whether the `F` key has just been pressed.
  - There's a function similar to `Input.GetKey()` for this.
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
   if(EnterCarButtonPressed()){
        if(PlayerIsInCar()) {
            LeaveCar();
        } else {
            EnterCar();
        }
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

---

## 4. GameObject Lifetime

### Slides
Need Help? [Here's The Slides!](slides/README.md#4-gameobject-lifetime)

### Spawn a Car

Add an empty ParkingSpot to the Game:\
Create a GameObject with some Visual (2D or 3D) for the Parking Spot. Name it `Parking Spot`\
Create the ParkingSpot-Script:
- `Parkingspot.cs` should inherit from `UnityEngine.MonoBehaviour`
- It has a `public` field of type `bool` named `hasCar`
- It has a `public` field of type `GameObject` named `carPrefab`
- This value can be set in the Inspector.

Implement the Mechanic that will spawn a car on Game Start, if the Parking Spot has a Car assigned:
- On Game Start, if `hasCar` is true:
  - Instantiate a new Instance of `carPrefab`
  - Set that Instance's position to the same position as the `ParkingSpot`
Add the `ParkingSpot`-Script to your GameObject named `Parking Spot`\
Keep the value of `Has Car` to `false`\
Start the Game and validate, that the Parking Spot will still be empty after starting the Game.

Now, add a parked ParkingSpot to the Game:\
Make a Duplicate of the `Parking Spot`-GameObject. Name it `Parking Spot (Full)`.\
Move the Duplicate somewhere different from the first Parking Spot.\
On The Duplicate's `ParkingSpot`-Script, set:
- `Has Car` to true
- `Car Prefab` to a Prefab of the Car in your scene:
  - Drag and Drop the `Car`-GameObject to your Project-View to create a new Prefab.
  - Then, Drag the `Car`-Prefab from your Project-View into the `Car Prefab` Field.]
  - Then, Delete the old `Car`, which was already in the Scene (Hierarchy). it is not needed anymore.
Start the Game and validate, that the Duplicate now contains a Copy of the `Car`-Prefab.

Validate, that you can Enter and Leave the Car properly. If not, then Read up on Communicating Between GameObjects again.\
Add a second Full Parking Spot to the Game (Duplicate the existing one). Can you Enter / Leave each Car properly? If not: Fix the Bugs\
Make the ParkingSpot into a Prefab.\
Make ParkingSpot (Full) a Prefab Variant of ParkingSpot.\
It's always best to have Prefab Templates available for your Game's Elements :)

### Add a Main Menu
- Create a new Scene named `MainMenu`
- Open that Scene
- Add a Button with the Label `Start Game`
- Create a Script named `StartGame.cs`
  - Give it a `public` Method with return type `void` named `LoadGame`, without Parameters.
  - In that Method, Load your real Game Scene (the one with Player, Cars, etc.)
  - Call that Method in the Button's OnClick-Event (using the Inspector)
Start the Game and Validate, that after clicking `Start Game`, your Main Menu disappears and instead you see your Game.

### Add a Game Restart Button
- In your Game-Scene, add a "Main Menu" Button to the Top right of the Screen.
- If clicked, Activate another GameObject named `MainMenu` that contains two Buttons as Children:
  - Retart Game Button, which will also call `StartGame`-Script's `LoadGame`-Method.
  - Close Button, which will Deactivate the `MainMenu`-GameObject again.
Test, that you can restart the game by opening the Main Menu and then clicking Restart Game.\
- BONUS: Make it possible to open and close the MainMenu using the Escape-Button, too.

### BONUS: Respawn a Car
- Implement in your `ParkingSpot`:
- If there has been no car on this spot for more than 10 seconds, a new Car spawns.
