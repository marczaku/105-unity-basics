# Exercises 010 Unity UI System

## Preparation

Read the slides on [010 - Unity UI System](../slides/010-unity-ui-system.md)

## Timer

Add a Timer to our Game:
- You need a GameObject with `Text`-Component, place it centrally at the top of the Screen.
- Make sure that there is a Canvas-Component on one of that Text-Component's parents.
- Add a new Script `Timer` to the same GameObject as the `Text`.
- Have the Script count the time that has passed. Save it to a field of type `float` named `timePassed`. Use `time.DeltaTime` and `Update`
- And then set it on the `Text`'s `text` Property using the Format `"0.00s"`. You can pass the Format to `float`'s `ToString()`-Method.

See, if everything works! :)

BONUS 1: Use a nice TextMeshPro Text!
BONUS 2: Make sure, that the `Timer` is still on the top of the Screen for different Resolutions. You can change your Resolution in the Game-View.
BONUS 3: Make the Timer count down and restart the Level when it's at 0.
BONUS 4: Make a Timer, that counts in Minutes and Seconds: It should show for example instead of 160.22s "2 min 40s"

## Floating Text

Whenever the Player enters a car, spawn a Text at the Player's Position, which then floats upwards, while fading out. The Text says "Nice Car!"
- Use a World-Canvas for this.
- Spawn a Premade Prefab for the text when the Player enters a Car.
- Spawn it at the Player's Position.
- On the Prefab, put a Text that says "Nice Car!" 
- On the Prefab, put a new Script named `FloatingText`, which decreases the `Text`'s `color`'s `a` field (alpha) and increases its Y-position every `Update`

BONUS: Destroy the FloatingText as soon as it's fully invisible (alpha <= 0)
