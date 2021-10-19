## 1. Event Functions

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

- Event Functions are functions that you can use in your Components / MonoBehaviours
- They will be called by Unity automatically
- They have to have the correct parameters

---

## 2. Awake

```cs
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
  void Awake() {
    Debug.Log("Awake is called as" +
              "the very first function." +
              "Put everything here, which" +
              "MUST be executed before anything" +
              "else.");
  }
}
```

- Awake is called ONCE for each script only
- Only, when it‘s Enabled for the first time
- It‘s called before any other function
- Put early initialization Code here
- Use Script Execution Order in Project Settings if you need more control than that
- Use Awake whenever you can (if you do not depend on other scripts to Awake())

---

## 3. Start / Ondestroy

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

- Start is called ONCE for each script
- When it‘s enabled the first time
- Great for Initialization
- OnDestroy is called ONCE for each script
- When it‘s destroyed, unloaded, or the Game is being quit
- Great for Clean-Up

---

## 4. Onenable / Ondisable

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

- `OnEnable` is called everytime, when the Script is enabled
- Also, when already enabled from Start
- `OnDisable is called everytime, when the Script is disabled
- Also, right before the script is Destroyed

---

## 5. Update

```cs
using UnityEngine;
public class NewBehaviourScript : Monobehaviour {
  void Update() {
    Debug.Log("Update is called every frame.");
  }
}
  ```
  
- Update is called every frame
- When your script is enabled, only
- Put logic here that needs to execute regularly
- Like Input, Visual Updates, …

---

## 6. FixedUpdate

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

<img width="410" alt="image" src="https://user-images.githubusercontent.com/7360266/137853344-1db6e6a5-2291-4371-9fb1-7628124ca7a6.png">

- `FixedUpdate` is called a fixed amount of times per second
- Put logic here that‘s supposed to be framerate-independent
- Like physics, AI, heavy game-logic…
- Amount of times per second can be configured in Project Settings > Time > Fixed Timestep

---

## 7. LateUpdate

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

- `LateUpdate` is called after all other Update functions
- It‘s great for when you have to wait for other Updates to complete
- E.g. check for the `WinCondition` here, so all other Scripts (like Damages, Heals, etc.) are evaluated before.

---

## 8. Collision (3D)

```cs
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
  void OnCollisionEnter(Collision other) {
    Debug.Log("Collision started");
  }
  
  void OnCollisionStay(Collision other) {
    Debug.Log("Still colliding");
  }
 
 void OnCollisionExit(Collision other) {
    Debug.Log("Collision ended");
  }
}
```

- These three functions are called for collisions for Objects with 3D-Colliders
- Only, if the other object‘s collider is not a Trigger
- Only, if at least one of both objects has a RigidBody
- You will find all details about the collision in the Collision-Object

---

## 9. Trigger (3D)

```cs
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
  void OnTriggerEnter(Collider other) {
    Debug.Log("Trigger overlap started");
  }
  
  void OnTriggerStay(Collider other) {
    Debug.Log("Still Overlap Trigger");
  }
 
 void OnTriggerExit(Collider other) {
    Debug.Log("Trigger Overlap ended");
  }
}
```

- These three functions are called for collisions for 3D-Objects
- Only, if the other object‘s collider is a Trigger
- Only, if one of both objects has a RigidBody
- You will find all details about the collider in the Collider-Object

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
  
- There‘s a lot of useful mouse input events
- They are only triggered on gameObjects with colliders
- There‘s a better method of using IPointerClickHandler and similar interfaces in combination with PhysicsRayCaster

---

## 12. Other Useful Ones

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
             "information ot your game. Bad for performance.");
  }
  
    void OnValidate() {
    Debug.Log("Called on game start and whenever" +
             "a value is changes in the inspector." +
             "Great for validating the input.");
  }
}
```

- These all have different use cases.
- `OnApplicationQuit´ is great for saving data.
- `OnApplicationPause´ is great for pausing the game and opening the pause menu on return.

---
