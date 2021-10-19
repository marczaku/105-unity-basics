## 1. Direct Referencing

```cs
public class SuperThrusterInput : MonoBehaviour {
  public Thruster mainThruster;
  ```
  ```cs
void Start() {
  this.mainThruster.enabled = true;
  ```
<img width="416" alt="image" src="https://user-images.githubusercontent.com/7360266/137977123-1ebdec44-ba01-42d9-bfc3-d9311cee4d3a.png">

- **Pro:** All-round solution
- **Contra:** Needs to be set up manually
- **Pro:** Can be set up specifically
- **Contra:** Does not work for dynamic instantiation

---

## 2. GetComponent<T>()

```cs
  void DecayVelocity() {
     if (GetComponent<RigidBody2D>().velocity.magnitude < 0.5f) {
        GetComponent<RigidBody2D>().velocity = Vector2.zero;
    { else {
        GetComponent<RigidBody2D>().velocity *= 0.95f;
    }
  }
  
  void DecayAngularVelocity() {
     if (Mathf.Abs(GetComponent<RigidBody2D>().angularVelocity) < 10f) {
        GetComponent<RigidBody2D>().angularVelocity = 0f;
     } else {
       GetComponent<RigidBody2D>().angularVelocity = 0.95f; 
  }
}
  ```
- **Condition:** Component is on same GameObject
- **Pro:** No configuration required
- **Contra:** Costs some extra performance
- **Pro:** Can react to changes during runtime
- **Contra:** Not very specific, might be hard to understand

=> What, if it is not on the same GameObject?

---

## 3. GetComponentinparent<T>()
  
  ```cs
  void OnCollisionEnter2D(Collision2D other) {
     var otherRidigBody = other.gameObject.GetComponentInParent<Rigidbody2D>();
     otherRigidBody.angularVelocity = 3f,
  }
  ```
  
 <img width="431" alt="image" src="https://user-images.githubusercontent.com/7360266/137978554-25438b42-4e83-458f-91a9-054d95f51115.png">
  
- This will look on the same `GameObject`, but also on all parent `GameObjects`
- And return the first component, that it finds

---

## 4. GetComponentinchildren<T>()
  
```cs
   void OnCollisionEnter2D(Collision2D other) {
     var otherThruster = other.gameObject.GetComponentInChildren<Thruster>();
     Destroy(otherThruster);
  }
```
  
<img width="298" alt="image" src="https://user-images.githubusercontent.com/7360266/137978897-510fdc9b-5f0d-4973-b3ba-273dc3f4cf4e.png">

- This will look on the same `GameObject`, but also on all child `GameObjects` and their child `GameObjects` and so on…
- And return the first component, that it finds

---

## 5. GetComponents<T>()

  ```cs
  void Start() {
     this.mainThruster.enabled = true;
     foreach (var thruster in GetComponentsInChildren<Thruster>()) {
        thruster.enabled = true;
        if (thuster.transform.localPosition.x > 0) {
           this.rightTrusters.Add(thruster);
  }
  ```
- Whenever one is not enough!
- This will return all Components that can be found.
- You can use GetComponents`, `GetComponentsInParent` or `GetComponentsInChilden`

---

## 6. Findobjectoftype<T>()
  
  ```cs
  void Start() {
     var hud = FindObjectOfType<Hud>();
     hud.playerName.text = this.name;
  ```
  <img width="332" alt="image" src="https://user-images.githubusercontent.com/7360266/137979581-0a703ad6-bd43-4426-b57a-4689f7c5c876.png">

- This can be used to find the component ANYWHERE in the scene
- Useful to find things like HUD, GameManager, Player, or other things you expect ONCE in the scene

---
  
## 7. Findobjectsoftype<T>()
 
```cs
  void BaitAllEnemies() {
     foreach (var enemy in FindObjectsOfType<Enemy>()) {
        enemy.target = this;
        enemy.isAngry = true;
  }
}
  ```
- This can be used to find all components of one kind ANYWHERE in the scene
- Useful to find things like all Enemies, all Traps, all Checkpoints, …
  ---
  
  ## 8. Bonus: sendmessage
  
  ```cs
  public class Trap : MonoBehaviour {
  void OnTriggerEnter2D(Collider2D other) {
     other.gameObject.SendMessage("OnTrap");
  }
}
  
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
- Pro: A very cool concept!
- Pro: Strong architecture!
- Con: Performance is not great
- Con: Type-Safety gets lost (string)
- Con: Limited in use, can not „Consume“ events (like in every other language / framework)
  ---
