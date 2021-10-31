# Exercises 011 Unity GameObject Lifetime

## Preparation

Read the slides on [011 - Unity GameObject Lifetime](../slides/011-unity-gameobject-lifetime.md)

## Spawn a Car

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

## Add a Main Menu
- Create a new Scene named `MainMenu`
- Open that Scene
- Add a Button with the Label `Start Game`
- Create a Script named `StartGame.cs`
  - Give it a `public` Method with return type `void` named `LoadGame`, without Parameters.
  - In that Method, Load your real Game Scene (the one with Player, Cars, etc.)
  - Call that Method in the Button's OnClick-Event (using the Inspector)
Start the Game and Validate, that after clicking `Start Game`, your Main Menu disappears and instead you see your Game.

## Add a Game Restart Button
- In your Game-Scene, add a "Main Menu" Button to the Top right of the Screen.
- If clicked, Activate another GameObject named `MainMenu` that contains two Buttons as Children:
  - Retart Game Button, which will also call `StartGame`-Script's `LoadGame`-Method.
  - Close Button, which will Deactivate the `MainMenu`-GameObject again.
Test, that you can restart the game by opening the Main Menu and then clicking Restart Game.\
- BONUS: Make it possible to open and close the MainMenu using the Escape-Button, too.

## BONUS: Respawn a Car
- Implement in your `ParkingSpot`:
- If there has been no car on this spot for more than 10 seconds, a new Car spawns.
