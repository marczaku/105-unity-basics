# Slides 2 - Scripting

Components are used to define a GameObject's Behaviours. You can think of Components as Lego Bricks and GameObjects being the Object that holds them all together.

For example, a Creep in a Tower Defense might have Components for
- Rendering (Displaying) a Monster on the Screen
- Following a Path
- Holding Health Logic
- Sound Effects
- An Input Area to allow the Player to Click on it

## 1. Components

- `GameObjects` in Unity have one or more Components
- Components Give a `GameObject` meaning
- A `GameObject` is only something with 
  - a name, 
  - a position 
  - optionally a Parent and/or a Child (or more)

<img src="https://user-images.githubusercontent.com/7360266/137622846-6ab6d44e-494e-4aeb-ab4c-411b15dcefc2.png" width="400" height="220">

- `GameObjects` **ALWAYS** have a `Transform`-Component
  - Sometimes, it‘s a `RectTransform`
- Each Component represents one Behaviour of this `GameObject`
- In other Frameworks, `GameObjects` are called `Entities`
  
<img src="https://user-images.githubusercontent.com/7360266/137622851-5ba41a62-a267-48e3-88e5-a8b74ca42814.png" width="400" height="200">

### 1.1. Adding Components

You can add more components using the Add Component Button in the Inspector

<img src="https://user-images.githubusercontent.com/7360266/137623173-425211b4-f46c-4dee-ad97-1873dd46ab6a.png" width="400" height="250">

Or by Drag-Dropping a Script from the Project-Window

<img src="https://user-images.githubusercontent.com/7360266/137623183-0c25a99d-fd89-4364-9ad3-50fcc4a5c2d7.png" width="400" height="250">

### 1.2. (Re)-Moving Components
You can Remove and Move components using the `ContextMenu` (by Rightclicking or using the dots in the Top-Right of a Component)

<img src="https://user-images.githubusercontent.com/7360266/137623263-5cd3ca6f-5d00-4c35-9f41-e15debf4be80.png" width="400" height="250">


### 1.3. Configuring Components

You can configure components using the Inspector

<img src="https://user-images.githubusercontent.com/7360266/137623350-43f8d49f-5d57-4b18-a9aa-1da4a71b344d.png" width="400" height="250">

- You can only change variables that are:
  - Either Public
  - Or have a [SerializeField]-Attribute
- Private variables without attributes can not be seen / edited.

<img src="https://user-images.githubusercontent.com/7360266/137623355-1aae4a5e-b12a-4004-99ab-bd71cd0ae8ad.png" width="500" height="250">

### 1.4. Custom Components

- You can create your own Components by using the `Create`>`C# Script` Context-Menu within the Project View.
- The Component name has to match the File-Name!!
- The Component has to be a public class and inherit from MonoBehaviour
- If your Component meets these Criteria, you can add them to GameObjects using any of the methods described in [Adding Components](#adding-compponents)
<img src="https://user-images.githubusercontent.com/7360266/137623409-dd6d61d7-13ee-416f-b20c-324a32341776.png" width="400" height="250">

<img src="https://user-images.githubusercontent.com/7360266/137623411-db0bc147-2921-457a-8a21-c34ca52339e8.png" width="400" height="250">


## 2. Event Functions

- Event Functions are functions that you can use in your Components / MonoBehaviours
- They will be called by Unity automatically
- They have to have the correct parameters

```cs
using UnityEngine;
public class NewBehaviourScript : MonoBehaviour {
  // start is called before the first frame update
  
  void Start() {
  }
  
  // update is called once per frame 
  
  void Update() {
  }
}
```

### 2.2. Awake

- `Awake` is called ONCE for each script only
- Only, when it‘s Enabled for the first time
- It‘s called before any other function
- Put early Initialization Code here
- Use Script Execution Order in Project Settings if you need more control than that
- Use `Awake` whenever you can (if you do not depend on other scripts to `Awake` first)

```cs
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
  void Awake() {
    Debug.Log("I'm Awake!");
  }
}
```

### 2.3. Start / OnDestroy

- `Start` is called ONCE for each script
- When it‘s enabled the first time
- Great for Initialization
- You should use `Awake` instead if you can, though
- But if you depend on another Script to `Awake` first, then use `Start` here

- `OnDestroy` is called ONCE for each script
- When it‘s destroyed, unloaded, or the Game is being quit
- Great for Clean-Up

```cs
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
  void Start() {
    Debug.Log("Start is called once" +
              "for this script only. Put logic" +
              "here, that needs to happen once" +
              "after game start");
  }
  
  void OnDestroy() {
    Debug.Log("OnDestroy is the counter-" + 
              "part and called when this object" +
              "gets destroyed. Even, if it's because"+ 
              "of the scene being unloaded, or the" +
              "game being quit.");
  }
}
```

### 2.4. OnEnable / OnDisable

- `OnEnable` is called every time, when the Script or its GameObject is enabled
- Also, when already enabled from Start
- `OnDisable` is called every time, when the Script or its GameObject is disabled
- Also, right before the script is Destroyed

```cs
using UnityEngine;
public class NewBehaviourScript : Monobehaviour {
  void OnEnable() {
    Debug.Log("OnEnable is called every" +
              "time, when this script is enabled.");
  }
  
  void OnDisable() {
    Debug.Log("OnDisable is called every" +
              "time, when this script is disabled.");
  }
}
```

### 2.5. Update

  
- `Update` is called every frame
- When your script is enabled, only
- Put logic here that needs to execute regularly
- Like Input, Visual Updates, …

```cs
using UnityEngine;
public class NewBehaviourScript : Monobehaviour {
  void Update() {
    Debug.Log("Update is called every frame.");
  }
}
```

### 2.6. FixedUpdate

- `FixedUpdate` is called a fixed amount of times per second
- Put logic here that‘s supposed to be framerate-independent
- Like physics, AI, heavy game-logic…

```cs
using UnityEngine;
public class NewBehaviourScript : Monobehaviour {
  void FixedUpdate() {
    Debug.Log("FixedUpdate is called a fixed" +
              "amount of times per second. Default is" +
              "50 times per second.");
  }
}
```

- Amount of times per second can be configured in Project Settings > Time > Fixed Timestep

<img width="410" alt="image" src="https://user-images.githubusercontent.com/7360266/137853344-1db6e6a5-2291-4371-9fb1-7628124ca7a6.png">


### 2.7. LateUpdate


- `LateUpdate` is called after all other `Update` functions
- It‘s great for when you have to wait for other `Update` to complete
- E.g. check for the `WinCondition` here, so all other Scripts (like Damages, Heals, etc.) are evaluated before.
- Or move your Healthbars to the player's position
  - This makes sure, that the player first moves (in `Update`)
  - And THEN, your Healthbar gets moved above the Player
  - If it was to happen in reverse, your Healthbar might end up where the player HAS BEEN a moment ago ^^


```cs
using UnityEngine;
public class NewBehaviourScript : Monobehaviour {
  void LateUpdate() {
    Debug.Log("I'm late to the party," +
              "but I'm called once per frame" +
              "nevertheless.");
  }
}
```


### 2.7.1 Overview

Abbreviations:
- GO: GameObject
- MB: MonoBehaviour

Happens when a Script become Active:

|  Method |     Condition             |Frequency|Priority|
|---------|---------------------------|---------|--------|
|  Awake  |       GO Active           |  ONCE   |    1   |
|  Start  |   GO Active & MB Enabled  |  ONCE   |    3   |
|OnEnable |   GO Active & MB Enabled  |RECURRING|    2   |

Happens when a Script becomes inactive:

|  Method |     Condition             |Frequency|Priority|
|---------|---------------------------|---------|--------|
|OnDisable| GO Inactive | MB Disabled |RECURRING|    1   |
|OnDestroy| GO Destroyed & MB Started |  ONCE   |    2   |

Happens once per Frame:

|   Method  |     Condition             |Frequency|Priority|
|-----------|---------------------------|---------|--------|
| Update    |  GO Active & MB Enabled   |RECURRING|    1   |
| LateUpdate|  GO Active & MB Enabled   |RECURRING|    2   |

Happens only when `fixedDeltaTime` has passed.
- happens multiple times, if more than 1x `fixedDeltaTime` has passed.

|   Method  |     Condition             |Frequency|Priority|
|-----------|---------------------------|---------|--------|
|FixedUpdate|  GO Active & MB Enabled   |RECURRING|    1   |

### 2.8. Collision (3D)

- These three functions are called for collisions for Objects with 3D-Colliders
- Only, if the other object‘s collider is not a Trigger
- Only, if at least one of both objects has a RigidBody
- You will find all details about the collision in the Collision-Object


```cs
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
  void OnCollisionEnter(Collision other) {
    Debug.Log("Collision started");
  }
  
  void OnCollisionStay(Collision other) {
    Debug.Log("Still colliding in this Frame");
  }
 
 void OnCollisionExit(Collision other) {
    Debug.Log("Collision ended");
  }
}
```

### 2.9. Trigger (3D)

- These three functions are called for collisions for 3D-Objects
- Only, if the other object‘s collider is a Trigger
- Only, if one of both objects has a RigidBody
- You will find all details about the collider in the Collider-Object

```cs
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
  void OnTriggerEnter(Collider other) {
    Debug.Log("Trigger overlap started");
  }
  
  void OnTriggerStay(Collider other) {
    Debug.Log("Still Overlapping Trigger in this Frame");
  }
 
 void OnTriggerExit(Collider other) {
    Debug.Log("Trigger Overlap ended");
  }
}
```

### 2.10. Same For 2D

```cs
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
  void OnCollisionEnter2D(Collision2D other) {
    Debug.Log("Collision started");
  }
  
  void OnCollisionStay2D(Collision2D other) {
    Debug.Log("Still colliding");
  }
 
 void OnCollisionExit2D(Collision2D other) {
    Debug.Log("Collision ended");
  }
}
```

```cs
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
  void OnTriggerEnter2D(Collider2D other) {
    Debug.Log("Trigger overlap started");
  }
  
  void OnTriggerStay2D(Collider2D other) {
    Debug.Log("Still Overlap Trigger");
  }
 
 void OnTriggerExit2D(Collider2D other) {
    Debug.Log("Trigger Overlap ended");
  }
}
```

### 2.11. Mouse Input

  
- There‘s a lot of useful mouse input events
- They are only triggered on GameObjects with Colliders
- There‘s a better method of using `IPointerClickHandler` and similar interfaces in combination with `PhysicsRayCaster`

```cs
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
  void OnMouseDown() {
    Debug.Log("Click started");
  }
  
  void OnMouseDrag() {
    Debug.Log("I'm being dragged");
  }
  
  void OnMouseUp() {
    Debug.Log("Click ended");
  }

  void OnMouseEnter() {
    Debug.Log("Hover started");
  }
  
  void OnMouseExit() {
    Debug.Log("Hover ended");
  }
}
```

### 2.12. Other Useful Ones

- These all have different use cases.
- `OnApplicationQuit´ is great for saving data before the application shuts down.
- `OnApplicationPause´ is great for pausing the game when the game is minimized and opening the pause menu on return.

```cs
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
  void OnApplicationQuit() {
    Debug.Log("Called when you quit the game.");
  }
  
    void OnApplicationPause(bool pauseStatus) {
    Debug.Log("Called when pausing (true)" +
              "or un-pausing (false) the game.");
  }
  
    void OnGUI() {
    Debug.Log("Great for quickly adding debug" +
             "information for your game. Bad for performance.");
  }
  
    void OnValidate() {
    Debug.Log("Called on game start and whenever" +
             "a value is changes in the inspector." +
             "Great for validating the input.");
  }
}
```

