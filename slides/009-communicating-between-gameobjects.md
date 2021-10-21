# 009 Communicating between GameObjects

As our Game grows, our Components need to be able to interact...
- The Mobs in our Tower Defense need to find towers.
- The Towers need to target and attack Mobs.
- The player must be able to enter Cars.
- The Gems in a Match-3 Game must be able to Match and Pop, if matched.

To enable this communication between Components, Unity has a few Tools up its sleeve:

## 1. Direct Referencing

The most manual, yet most reliable way of accessing other Components is directly referencing them through a Field:

```cs
public class SuperThrusterInput : MonoBehaviour {
  public Thruster mainThruster; // Needs to be Assigned in the Inspector

  void Start() {
    this.mainThruster.enabled = true;
  }
}
```
<img width="416" alt="image" src="https://user-images.githubusercontent.com/7360266/137977123-1ebdec44-ba01-42d9-bfc3-d9311cee4d3a.png">

- **Pro:** All-round solution
- **Contra:** Does not work for objects instantiated dynamically during run-time (like a new monster that spawned)
- **Pro:** Can be set up specifically
- **Contra:** Needs to be set up manually

---

## 2. GetComponent<T>()

If a Component that you need is on the same GameObject as the Script that needs it, we can use `GetComponent<T>` to get it during Runtime:

```cs
void BrakeCar() {
  // Reduce Rigidbody's Velocity by 5%:
  GetComponent<Rigidbody2D>().velocity *= 0.95f;
}
```
- **Condition:** Component is on same GameObject
- **Pro:** No configuration required
- **Contra:** Costs some extra performance
- **Pro:** Can react to changes during runtime

=> What, if it is not on the same GameObject?

---

## 3. GetComponentInParent<T>()

If the Component is not on our own GameObject, or it is at least not guaranteed to be there, but we know that it is on one of our Parent GameObjects, we can use `GetComponentInParent`:
  
```cs
void OnCollisionEnter2D(Collision2D other) {
  // The rigid body of a gameObject that collided with us could be on the GameObject itself
  // Or on one of its parents (since child GameObjects can have their own Colliders)
  Rigidbody2D otherRigidBody = other.gameObject.GetComponentInParent<Rigidbody2D>();
  otherRigidBody.angularVelocity = 3f,
}
```
  
 <img width="431" alt="image" src="https://user-images.githubusercontent.com/7360266/137978554-25438b42-4e83-458f-91a9-054d95f51115.png">
  
- This will look on the same `GameObject`, but also on all parent `GameObjects`
- And return the first component, that it finds

---

## 4. GetComponentInChildren<T>()

If the GameObject is not on one of the GameObject itself, but on one of its Children, you can use `GetComponentInChildren`:
  
```cs
void OnCollisionEnter2D(Collision2D other) {
  // The Thruster of the space ship that collided with us could be on one of the Child GameObjects:
  Thruster otherThruster = other.gameObject.GetComponentInChildren<Thruster>();
  Destroy(otherThruster);
}
```
  
<img width="298" alt="image" src="https://user-images.githubusercontent.com/7360266/137978897-510fdc9b-5f0d-4973-b3ba-273dc3f4cf4e.png">

- This will look on the same `GameObject`, but also on all child `GameObjects` and their child `GameObjects` and so on…
- And return the first component, that it finds

---

## 5. GetComponents<T>()

Sometimes one is not enough, but you just want them all:

```cs
void Start() {
  this.mainThruster.enabled = true;
  // This finds all Thrusters that are parented under ourselves:
  Thruster[] thrusters = GetComponentsInChildren<Thruster>();
  // HINT: Use foreach-loop, if you already know, what that is!
  for(int i = 0; i < thrusters.Length; i++) {
    Thruster thruster = thrusters[i];
    thruster.enabled = true;
    if (thuster.transform.localPosition.x > 0) {
        this.rightTrusters.Add(thruster);
    }
  }
}
```
- Whenever one is not enough!
- This will return all Components that can be found given the Constraints.
- You can use 
  - `GetComponents` to get all Components on the same GameObject
  - `GetComponentsInParent` to get all on the same GameObject and its Parents
  - `GetComponentsInChildren` to get all on the same GameObject and its Children

---

## 6. FindObjectOfType<T>()
  
- This can be used to find the component ANYWHERE in the scene
- Useful to find things like HUD, GameManager, Player, or other things you expect ONCE in the scene

```cs
void Start() {
  Hud hud = FindObjectOfType<Hud>();
  hud.playerName.text = this.name;
}
```
<img width="332" alt="image" src="https://user-images.githubusercontent.com/7360266/137979581-0a703ad6-bd43-4426-b57a-4689f7c5c876.png">


---
  
## 7. FindObjectsOfType<T>()
 
- This can be used to find all components of one kind ANYWHERE in the scene
- Useful to find things like all Enemies, all Traps, all Checkpoints, …

```cs
void BaitAllEnemies() {
  Enemy[] enemies = FindObjectsOfType<Enemy>();
  // HINT: Use foreach-loop, if you already know, what that is!
  for(int i = 0; i < enemies.Length; i++) {
    Enemy enemy = enemies[i];
    enemy.target = this;
    enemy.isAngry = true;
  }
}
```
---
  
## 8. Bonus: SendMessage

SendMessage can be used whenever you want to send an Event / a Messsage to a GameObject, but you don't know, what Component exactly is supposed to receive it.

If a Script like this exists and it is attached to a GameObject.
  
```cs
public class Player : MonoBehaviour {
  public int health = 5;
  void OnTrap() {
    this.health--;
    if (this.health == 0) {
        Destroy(this.gameObject);
    }
  }
}
```

Then another Script could send a Message to the GameObject that it collided with.\
If that other GameObject has the `Player`-Component attached, or any other Component that has a `public` `OnTrap`-Method.\
Then that Method will be invoked by the Message!

```cs
public class Trap : MonoBehaviour {
  void OnTriggerEnter2D(Collider2D other) {
     other.gameObject.SendMessage("OnTrap");
  }
}
```
  
- Pro: A very cool concept!
- Pro: Strong architecture! (Classes don't need to know about each other)
- Con: Performance is not great
- Con: Type-Safety gets lost (we pass the Method Name as a `string`? What about Spelling Errors?)
- Con: Limited in use, can not „Consume“ events (like in every other language / framework)
  - Imagine, there is a `Damage`-Message and you have `Health` and `Armor` Components that can receive the Message. Right now, they would both Receive the Message, so the `Armor` would not be able to "protect" the `Health`-Component.
  ---
