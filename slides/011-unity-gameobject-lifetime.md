## 1. Part of a Scene

<img width="371" alt="image" src="https://user-images.githubusercontent.com/7360266/139598025-c116ed41-6018-46bb-8d30-d92de314978e.png">

<img src="https://user-images.githubusercontent.com/7360266/139598026-2161e769-2f09-412c-8700-0e45c646872a.png" width="300" height="300">

- `GameObjects` can be part of a scene
- They exist as a Template in the editor
- They are saved in the .unity scene file
- Their lifetime starts when the scene is loaded in PlayMode
- Their lifetime ends when the scene is unloaded / another scene is loaded
- If the scene is currently open, it will automatically be loaded, when PlayMode is entered

---

## 2. Part of a Prefab

<img width="371" alt="image" src="https://user-images.githubusercontent.com/7360266/139598052-ccc56543-0de3-4359-965a-c587d2ad79cd.png">

<img src="https://user-images.githubusercontent.com/7360266/139598026-2161e769-2f09-412c-8700-0e45c646872a.png" width="300" height="300">

- `GameObjects` can be part of a prefab
- They exist as a Template in the prefab edit mode
- They are saved in the .prefab prefab file
- Their lifetime starts when you Instantiate the prefab during PlayMode
- Their lifetime ends when you Destroy it / another scene is loaded
- You can also add instances of your prefabs to a scene

---

## 3. Created Through Code

```cs
void CreateView() {
   var parent = New GameObject("GoldView");
   var child = New GameObject("GoldViewChild");
   child.transform.SetParent(parent.transform);
   var text = child.AddComponent<Text>();
   text.text = "This is a gold view.";
   }
   ```

- `GameObjects` can be created through a script
- Their lifetime starts when your code gets executed
- Their lifetime ends when you Destroy it / another scene is loaded
- This is not a recommended way, as designers can not make changes to GameObjects this way

---

## 4. GameObject Lifetime (simplified)

```cs
void Awake() {
   CreateView();
}
   ```
   
   ```cs
   void GameObjectLifeTime() {
      // first time loaded and enabled:
      Awake();
      OnEnable();
      Start();
      // every frame while enabled:
      Update();
      LateUpdate();
      Update();
      LateUpdate();
      // 50 times per seond while enabled:
      FixedUpdate();
      FixedUpdate();
      // possibly multiple times disabled and enabled:
      OnDisable();
      OnEnable();
      OnDisable();
      OnEnable();
      // unloading:
      OnDisable();
      OnDestroy();
   }
   ```
   
- You can react to a `GameObject`‘s lifetime
- Using Unity Event functions
- They get executed automatically in this order:
- You just need to define a parameterless method with that matching name:

---

## 5. Disabling GameObjects

```cs
void Sample(GameObject gameObject) {
   gameObject.SetActive(true);
   gameObject.SetActive(false);
   // this checks if the gameObject itself is marked to be enabled
   Debug.Log(gameObject.activeself);
   // this checks if the gameObject is actually enabled 
   // (because it could be disabled because it's parent is disabled)
   Debig.Log(gameObject.activeInHierarchy);
 }
   ```
   
- **You can disable GameObjects:**
  - Through the Editor
  - Through Code
- Disabling `GameObjects` automatically disables that `GameObject‘s children as well:
- 
<img width="258" alt="image" src="https://user-images.githubusercontent.com/7360266/139598294-ff36d56e-95ab-41bf-a505-30fac10d4c20.png">

<img width="369" alt="image" src="https://user-images.githubusercontent.com/7360266/139598301-1f67ec2e-68fe-438d-af00-7841c6024ec7.png">

---

## 6. Destroying GameObjects

```cs
void Sample(GameObject gameObject) {
   // you can destroy a gamObject:
   Destroy(gameObject);
   // this is necessary in very special occasions
   // when you are executing this code from
   // an editor extension insted 
   // of within your game logic:
   DestroyImmediate(gameObject);
 }
   ```
   
<img src="https://user-images.githubusercontent.com/7360266/139598344-80b3ece6-5535-41d4-ac56-47db52bf83f7.png" width="400" height="290">

- **You can destroy GameObjects:**
  - Through the Editor
  - Through Code
- Destroying GameObjects automatically destroys that GameObject‘s children as well

---

## 7. Instantiating Prefabs

```cs
// you can assign the prefab in the inspector of this script
public GameObject prefab;
void Sample(GameObject gameObject) {
   // instantiates the prefab as root go:
   Tnstantiate(prefab);
   // instantiates the prefab as a child of the gameObject:
   Instantiate(prefab, gameObject.transform);
   // instantiates the prefab at the given position and rotation:
   Instantiate(prefab, new Vector3(1, 2), Quaternion.Euler(1, 2, 3));
   // you can save a reference of the new instance to a vairable:
   var instance = Instantiate(prefab);
   var instantiate.name = "Cool Name";
   }
   ```
<img src="https://user-images.githubusercontent.com/7360266/139598374-b93fec5f-82d8-4683-96d9-7db776d80aec.png" width="400" height="300">

- **You can instantiate Prefabs:**
  - _Through the Editor:_ drag from the Project View to the scene Hierarchy
  - Through Code
- When Instantiating a Prefab, it becomes part of the currently active scene
---

## 8. Unloading Scenes

```cs
void Sample(GameObject gameObject) {
// you can unload a loaded scene:
SceneManager.UnloadSceneAsync("Level01");
// you can load a new scene without unloading the current one:
SceneManager.Loadscene("HUD", LoadSceneMode.Additive);
// you can load a new scene while unloading all current ones:
SceneManager.Loadscene("Level02");
// you can flag gameObjects to never be unloaded on scene switches:
DontDestroyOnLoad(gameObject);
}
```

<img src="https://user-images.githubusercontent.com/7360266/139598427-2b365b95-0e49-4998-9d1b-bef154dfeaf1.png" width="400" height="330">

- **You can Load a Scene:**
  - _Through the Editor:_ by double clicking on a scene, or Drag Dropping the scene to the hierarchy
  - Through Code
- When Loading a Scene
- All Current Scenes Are Unloaded
- Unless you Load it Additive
- When a Scene is unloaded, all gameObjects belonging to that scene will be destroyed

---
