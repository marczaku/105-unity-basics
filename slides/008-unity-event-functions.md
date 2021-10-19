## 1. Event Functions

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

---

## 2. Awake

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


---

## 3. Start / OnDestroy

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


---

## 4. OnEnable / OnDisable

- `OnEnable` is called everytime, when the Script or its GameObject is enabled
- Also, when already enabled from Start
- `OnDisable` is called everytime, when the Script or its GameObject is disabled
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

---

## 5. Update

  
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

---

## 6. FixedUpdate

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


---

## 7. LateUpdate


- `LateUpdate` is called after all other `Update` functions
- It‘s great for when you have to wait for other `Update` to complete
- E.g. check for the `WinCondition` here, so all other Scripts (like Damages, Heals, etc.) are evaluated before.


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

---

## 8. Collision (3D)

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


---

## 9. Trigger (3D)

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


---

## 10. Same For 2D

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

---

## 11. Mouse Input

  
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

---

## 12. Other Useful Ones


- These all have different use cases.
- `OnApplicationQuit´ is great for saving data.
- `OnApplicationPause´ is great for pausing the game and opening the pause menu on return.

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

---
