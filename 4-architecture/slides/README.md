# Slides 4 - Architecture

# 5. Connecting GameObjects

As our Game grows, our Components need to be able to interact...
- The Mobs in our Tower Defense need to find towers.
- The Towers need to target and attack Mobs.
- The player must be able to enter Cars.
- The Gems in a Match-3 Game must be able to Match and Pop, if matched.

To enable this communication between Components, Unity has a few Tools up its sleeve:

## Direct Referencing

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

- Condition: Target is an Asset, or both Source and Target are part of the same Scene.
- Pro: All-round solution
- Pro: Can be set up specifically
- Con: Does not work for objects instantiated dynamically during run-time (like a new monster that spawned)
- Con: Needs to be set up manually

---

## GetComponent\<T\>()

If a Component that you need is on the same GameObject as the Script that needs it, we can use `GetComponent<T>` to get it during Runtime:

```cs
void BrakeCar() {
  // Reduce Rigidbody's Velocity by 5%:
  GetComponent<Rigidbody2D>().velocity *= 0.95f;
}
```
- Condition: Component is on same GameObject
- Pro: No configuration required
- Pro: Can react to changes during runtime
- Con: Costs some extra performance

The little performance impact can be mitigated by caching the component after startup:

```cs
public class Car : MonoBehaviour {
  private Rigidbody2d rigidBody2D;

  void Awake() {
    rigidBody2D = GetComponent<RigidBody2D>();
  }

  void BrakeCar() {
    rigidBody2D.velocity *= 0.95f;
  }
}
```


=> What, if it is not on the same GameObject?

## GetComponentInParent\<T\>()

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

## GetComponentInChildren\<T\>()

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

## GetComponents\<T\>()

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

## FindObjectOfType\<T\>(bool)
  
- This can be used to find the component ANYWHERE in the scene
- Useful to find things like HUD, GameManager, Player, or other things you expect ONCE in the scene

```cs
void Start() {
  Hud hud = FindObjectOfType<Hud>();
  hud.playerName.text = this.name;
}
```

<img width="332" alt="image" src="https://user-images.githubusercontent.com/7360266/137979581-0a703ad6-bd43-4426-b57a-4689f7c5c876.png">


If you pass in `true` as an argument for the Parameter `bool includeInactive`, the Script can even find scripts on disabled GameObjects!

```cs
Hud hud = FindObjectOfType<Hud>(true);
```
  
## FindObjectsOfType\<T\>()
 
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

## Singleton

Another way of connecting Scripts is using static references:

```cs
public class Game : MonoBehaviour {
  public static Game Instance {get; private set;}

  public int starsCollected;

  void Awake() {
    Instance = this;
  }
}
```

Other scripts can now access the class through the static Property:

```cs
public class Star : MonoBehaviour {
  void OnTriggerEnter(Collider collider) {
    Game.Instance.starsCollected++;
    Destroy(gameObject);
  }
}
```
  
## Bonus: SendMessage

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

# 6. Scriptable Objects

## Serialized Classes

Serializable Classes which are Fields of Components...

```cs
[System.Serializable]
public class Level {
   public int level;
   public int experience;
}
```

```cs
public class Hero : MonoBehaviour {
   public Level level;
}
 ```
 
 ...become a part of the Component that they are part of:

<img width="382" alt="image" src="https://user-images.githubusercontent.com/7360266/139946588-1e981919-a08c-4c58-8b98-109bc29b0808.png">

<img width="382" alt="image" src="https://user-images.githubusercontent.com/7360266/139946601-bfc61709-1c03-46f2-8aeb-dee28ffd5f26.png">

---

## Serialized UnityEngine.Object

Classes inheriting from `UnityEngine.Object`...

```cs
public class Item : UnityEngine.Object {
   public int price;
   public int armor;
}
```

```cs
public class Enemy : MonoBehaviour {
   public Enemy enemy;
   public Item item;
```

... are treated as an object of their own!

<img width="389" alt="image" src="https://user-images.githubusercontent.com/7360266/139946685-074d45a8-c0bf-4835-be75-b9c9f773d132.png">


And referenced through their File ID

<img width="308" alt="image" src="https://user-images.githubusercontent.com/7360266/199338743-0fb3b2fd-ca31-4a05-8b9b-906d2a96e7ab.png">

### Functions of UnityEngine.Object

UnityEngine.Object is not very useful by itself:

<img width="389" alt="image" src="https://user-images.githubusercontent.com/7360266/139946991-503a814e-62fa-416c-9218-e3ab6dd06f91.png">

It contains some basic static Methods for resource management by Unity (`Destroy`, `Instantiate`, ...)
- It has a `name` 
  - (and `hideFlags` for the Editor)
- The Equality Operator `==` has been overloaded to check, if a `UnityEngine.Object` was destroyed

### Null Comparison

Here, we have a class, that inherits from `UnityEngine.Object`:

```cs
public class Item : Object {
   public int price;
}
```

If we have a Script with a reference to an `Object`:

```cs
public class Enemy : MonoBehaviour {
   public Item item;
   void Start() {
      // this is, where the following code samples will be placed
   }
```

We can Clone it using Unity's `Object.Instantiate`-Method:

```cs
Object newItem = Instantiate(this.item);
```

Usually, it should be enough, to assign `null` to `newItem` and the GarbageCollector takes care of the rest:

```cs
newItem = null;
```

In reality, though, any class inheriting from `UnityEngine.Object` needs to be explicitly destroyed using `Object.Destroy`.

```cs
Destroy(newItem);
```

The funny part now is, that the variable still points to the C#-class.
- even though, the Object got destroyed
- there will be no `NullReferenceException` thrown!

```cs
Debug.Log(newItem.price); // 25
```

Even more funny, is though, that Unity has overloaded the Equality-Operator `==` and will still return `true` when comparing to `null`:

```cs
Debug.Log(newItem == null); // True
```

Unfortunately, that feature does not work for modern C#'s null-coalescing operators `?.` `??` and `??=`:

```cs
Debug.Log(newItem?.price); // 25
```

Above code sample should theoretically be the same as:

```cs
Debug.Log(newItem != null ? newItem.price : null); // (null)
```

And here, it will not instantiate a new Item, even though the old one has been destroyed:

```cs
newItem ??= Instantiate(this.item);
```

## Object Lifetime

### `ObjectFactory.CreateInstance`

<img width="440" alt="image" src="https://user-images.githubusercontent.com/7360266/139948820-231e1e5c-e170-43bb-8b0d-6001d53230b0.png">

Used to create a new instance of `UnityEngine.Object`
- C#: A new instance of the class is created
- C++: A C++ Object with instanceId and Pointer is Created

<img width="418" alt="image" src="https://user-images.githubusercontent.com/7360266/139951820-5ebe602b-3bd6-4257-bdbb-88e1aa97596c.png">

### `Instantiate`

<img width="440" alt="image" src="https://user-images.githubusercontent.com/7360266/139948820-231e1e5c-e170-43bb-8b0d-6001d53230b0.png">

An existing Object gets cloned
- C#: New instance of the class + copy values
- C++: A C++ Object with instanceId and Pointer is Created

<img width="418" alt="image" src="https://user-images.githubusercontent.com/7360266/139951820-5ebe602b-3bd6-4257-bdbb-88e1aa97596c.png">

### `Destroy`

<img width="436" alt="image" src="https://user-images.githubusercontent.com/7360266/139948936-92a408a5-1276-446a-87b1-a6390c8eef18.png">

An existing Object gets destroyed
- C#: --
- C++: The C++ Object gets destroyed, the InstanceId and Pointer are Freed

<img width="409" alt="image" src="https://user-images.githubusercontent.com/7360266/139948970-c1e273f1-e0c6-4fb1-8f42-b673fa5c249b.png">

### Assigning `null`

<img width="157" alt="image" src="https://user-images.githubusercontent.com/7360266/140583902-70057ca9-12ed-4344-a42a-440380910e47.png">

The reference to the C# class is lost
- C#: The Garbage Collector cleans up the class
- C++: --

<img width="446" alt="image" src="https://user-images.githubusercontent.com/7360266/140583961-95e70077-f68f-41cb-917c-8e4b8830c6b6.png">


## `UnityEngine.Object` useless?

> Unity has a c++ engine component, and a c# scripting side. Everything that is a UnityEngine.Object needs to exist on both the c++ and the c# side for the engine to work with them.
> 
> If your MyClass derives from `UnityEngine.Object`, 
> 
> If you use your own constructor, the c++ side will not be informed of the thing being created, which will cause problems. The `==` operator has been `overriden` for `UnityEngine.Object`, and checks if the `object` exists on the c++ side when you compare with `null`. If it doesn't, `x == null` returns true even if `x is not null`.
> 
> `UnityEngine.Object` should no be derived from by users. But, since the engine internally needs to inherit from it, and we want to be able to reference a `UnityEngine.Object` by it's type, it can't be `sealed` or `internal`. If the C# language allowed for a class to be inheritable only from within it's own assembly, `UnityEngine.Object` would have that setting.

---

## `UnityEngine.Object` Inheritors

<img width="339" alt="Screenshot 2021-11-02 at 21 48 27" src="https://user-images.githubusercontent.com/7360266/139949437-784f552e-ea2d-4aab-b2e8-06948b7b0169.png">

Many Assets Inherit from `UnityEngine.Object` directly:

```cs
public class GameObject : Object {}
```

```cs
public class Texture : Object {}
```

```cs
public class SceneAsset : Object {}
```

But they require Custom Code (C++ and C#) to be provided by Unity to be usable.
- They can all exist as an Asset in the ProjectView

## `MonoBehaviour`

A `Component` is something that can be attached to a `GameObject`

```cs
public class Component : Object{}
```

A `Behaviour` is a `Component` that can be enabled and disabled

```cs
public class Behaviour : Component{}
```

A `MonoBehaviour` is the base class for all our C# Scripts and brings some features like Invoke and Coroutines

```cs
public class MonoBehaviour : Behaviour{}
```

We can not inherit from `Component` or `Behaviour` Directly
- Because only `MonoBehaviours` can be serialized properly, referencing our Scripts
- Without having to write C++ Code

<img width="633" alt="image" src="https://user-images.githubusercontent.com/7360266/139949713-2b645efb-29c7-453b-ab60-397f4d4162db.png">

Unity Components like `Transform` inherit from `Component` or `Behaviour`
- To improve performance
- By avoiding the extra `MonoBehaviour` Overhead

```cs
public class Transform : Component {}
```

<img width="223" alt="image" src="https://user-images.githubusercontent.com/7360266/139949771-247ba7a6-3be0-4fa1-b7e7-432a4a4b8742.png">

## Overview: Our options so far

Scenes & Assets: Are part of a project

<img width="227" alt="image" src="https://user-images.githubusercontent.com/7360266/139949951-314b40e3-fba5-4042-a141-d65d2b297179.png">

GameObjects: Are part of a scene or prefab

<img width="175" alt="image" src="https://user-images.githubusercontent.com/7360266/139949956-3f21f2a4-f9d1-44e3-a18f-b1456d7fe19a.png">

Components: Are part of GameObjects

<img width="393" alt="image" src="https://user-images.githubusercontent.com/7360266/139949965-bc91fd7a-def4-452a-bbf2-0f2140d3b593.png">

- Custom Class: 
  - Can be used by components
  - Can be used by code using new()
  - Can be serialized, if [System.Serializable]
- Custom `MonoBehaviour`:
  - Can be used like any other component (Add to GameObject in Editor or through Script)- 

<img width="392" alt="image" src="https://user-images.githubusercontent.com/7360266/139949979-59085fd3-110f-480e-8694-433d41b92767.png">

## What we can Customize

|Class|Customization|
|-----|-------------|
|Scene|---|
|Asset|Coming Next|
|GameObjects|---|
|Component|Inherit from `MonoBehaviour`|
|Class|Create Class, make it `[Serializable]`|
|Editor|Coming Soon|
|EditorWindow|Coming Soon|

## Scriptable Objects

Make a class Inherit from `UnityEngine.ScriptableObject`
- Give it a `[CreateAssetMenu]` Attribute

```cs
using UnityEngine;

[CreateAssetMenu]
public class Item : ScriptableObject {
  public int price;
}
```

You can now rightclick wihin Project View and Create a new Instance of that Class

![image](https://user-images.githubusercontent.com/7360266/139950409-51048af1-97c6-4e1d-88ec-4879a5dcf47b.png)

The Instance will exist as an Asset in your Project View

<img width="219" alt="image" src="https://user-images.githubusercontent.com/7360266/139950413-b8f50c20-891a-4e98-95aa-aa7fd78c06ca.png">

You can select it to view and edit its values in the Inspector

<img width="390" alt="image" src="https://user-images.githubusercontent.com/7360266/139950419-0b2052e7-5798-4984-99bc-5b04eade1582.png">

## Scriptable Object Serialization
A new `.asset` File is created
- As well as a matching `.asset.meta` File

<img width="294" alt="image" src="https://user-images.githubusercontent.com/7360266/139950534-fe37612d-b25c-4d2e-adc8-052bf88a6fb8.png">

The `.asset` File contains the serialized instance of your class
- Does this remind you of something?

![image](https://user-images.githubusercontent.com/7360266/139950541-df3e3326-eb40-4927-916f-5a866f3b4b86.png)

It looks exactly like a MonoBehaviour that's serialized as part of a Prefab or Scene:

![image](https://user-images.githubusercontent.com/7360266/139950631-ad13f917-50a3-4886-b331-a8d946600dd1.png)

## What‘s the difference to MonoBehaviour?

A Scriptable Object does not need a `GameObject` to be attached to

<img width="392" alt="image" src="https://user-images.githubusercontent.com/7360266/139950833-464590c0-e4be-4e28-a491-b0c217201e16.png">

And it cannot be attached to a `GameObject`, even if you wanted to
- It therefore has less overhead.
- That‘s it!

<img width="393" alt="image" src="https://user-images.githubusercontent.com/7360266/139950845-118319ea-4097-4e63-b36e-5e0f80d23815.png">

## Why not use a Prefab instead?

Valid question! Only reason:
- Less overhead:
  - No `GameObject`
  - No `Transform`
- People can't misuse it:
  - Cannot be instantiated into a Scene
  - No other Components can be added
  - Cannot be disabled
- Clear Intention!
  - It is always an Asset
  - A GameObject can be in a Scene or a Prefab

## Why not use a Plain Class instead?

Classes have even slightly less overhead than ScriptableObjects!

```cs
[Serializable]
public class Item {
  public int price;
}
```

Classes can be part of MonoBehaviours

```cs
public class TreasureBox : MonoBehaviour {
  public Item item;
}
```

But class instances can not be shared between multiple MonoBehaviours

ScriptableObjects can be created as an asset in the project view
- And then drag‘n‘dropped onto multiple MonoBehaviour-Fields

There is the option of serializing the classes manually (e.g. using Json or Yaml Serializer)
- And then referencing the TextAssets in your Game
- And then deserializing the TextAssets during Runtime

But that's a lot of work just to replicate what Unity already provides for us
- Also, it would add the overhead that we just saved back to our Game

## Common use cases

### Configuration

You can create a simple Immutable Scriptable Object class that you use for configuration.

Your Designer can now create multiple instances of this class for different types, e.g.

The lazy way:

```cs
public class CookieProducer {
  public int production;
  public float productionRate;
  public Color color;
  public Sprite image;
}
```

The clean, immutable way:

```cs
public class CookieProducer {

  public int Production => production;
  public float ProductionRate => productionRate;
  public float Color => color;
  public Sprite Image => image;

  [SerializeField] private int production;
  [SerializeField] private float productionRate;
  [SerializeField] private Color color;
  [SerializeField] private Sprite image;
}
```

You can now reference these in your Game where ever you want to use a Cookie Producer in your Game.

### Sharing Variables

You can share Variables across multiple Objects or even multiple Scenes!

<img width="527" alt="image" src="https://user-images.githubusercontent.com/7360266/139951199-ec871a16-d1bc-4317-aece-11ab2d393709.png">

- PlayerPrefab Updates PlayerHP ScriptableObject Instance
- Music System checks PlayerHP SO and changes music to sound dramatic when HP low
- User Interface checks PlayerHP SO and updates the HealthBar and Label
- Enemy AI checks PlayerHP SO and becomes less shy when the HP is low

### Sharing Events across Objects / Scenes

<img width="571" alt="image" src="https://user-images.githubusercontent.com/7360266/140585673-895fb16a-ba1a-44ab-b601-988488733319.png">


- PlayerPrefab Invokes OnPlayerDied ScriptableObject Instance
- Music System subscribed to the Event and plays Sad Game Over Music
- Game Over UI subscribed to the Event and displays itself on the screen
- Enemy subscribed to the Event and starts dancing and celebrating

### Sharing Code across Objects / Scenes

> Scriptable Objects don’t have to be just data. Take any system you implement in a MonoBehaviour and see if you can move the implementation into a ScriptableObject instead. Instead of having an InventoryManager on a DontDestroyOnLoad MonoBehaviour, try putting it on a ScriptableObject.
> 
> Since it is not tied to the scene, it does not have a Transform and does not get the Update functions but it will maintain state between scene loads without any special initialization. Instead of a singleton, use a public reference to your inventory system object when you need a script to access the inventory. This makes it easy to swap in a test inventory or a tutorial inventory than if you were using a singleton.
> 
> Here you can imagine a Player script taking a reference to the Inventory system. When the player spawns, it can ask the Inventory for all owned objects and spawn any equipment. The equip UI can also reference the Inventory and loop through the items to determine what to draw. 

## Amazing Features For Free

Scriptable Objects come with a lot of features for free:

<img width="258" alt="Screenshot 2021-11-02 at 22 04 06" src="https://user-images.githubusercontent.com/7360266/139951424-37793b0d-e664-494c-a4f1-f8f3303a99f3.png">

- Asset Management: Create, Duplicate, Delete within Unity
- Serialization: Save, Redo, Undo
- Inspector: Edit, Toggle, TextField, Slider, Lists, Color Pickers, …
- Referencing: Meta-File-ID System for safe referencing, even when renamed
- Drag & Drop utility