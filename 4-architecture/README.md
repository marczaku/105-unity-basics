# Exercises 009 Communicating Between GameObjects

## Small Theft Auto Code Improvements

Go through your Small Theft Auto Project and see, if you can replace a few of those `public` Component Reference Fields.

e.g.: Does the `Player` need a Reference to the `Car`? Or vice versa? Probably not! I'm sure that you can get these References in a smarter way using `GetComponent`, and `FindObjectOfType` and a bunch of the others.

The less we need to set up manually, the better, because the less we can forget to set up!

One example:\
Have the Player have a `Driver` Script.
- In `Update`, you check if `F` is Pressed Down.
- If so, you use `FindObjectsOfType<T>` to search for all `Vehicle`-Scripts in your Surroundings.
- BASIC: You check for the first one (`[0]`) and whether it's close enough, and hope that it's always just one car.
- BONUS: You find the one of all `Vehicle`, that's the closest to you and check, whether that one's close enough.
- If it is, you call `Enter` on that `Vehicle`, passing the `Driver` into the Method.

The `Enter`-Method:
- Stores the `Driver` to a `private` field named `driver`.
- Disables the `Driver`'s GameObject.
- Enables the `VehicleMovement`-Component on its own GameObject using `GetComponent<T>`

The `Vehicle`:
- In `Update` also checks, if `F` is Pressed Down.
- If so, you check, whether the `driver` field is null or not.
- If not, then you call `Leave`

The `Leave`-Method:
- Moves the `driver`'s GameObject to the Vehicle's position.
- Enables the `driver`'s GameObject.
- Sets the `driver` field to `null` (to make it clear, that there is no more driver in this car)
- Disables the `VehicleMovement`-Component on its own GameObject using `GetComponent<T>`
