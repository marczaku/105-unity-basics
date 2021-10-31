## 1. Part of a Scene

- `GameObjects` can be part of a scene
- They exist as a Template in the editor
<img width="371" alt="image" src="https://user-images.githubusercontent.com/7360266/139598025-c116ed41-6018-46bb-8d30-d92de314978e.png">

- They are saved in the .unity scene file
<img src="https://user-images.githubusercontent.com/7360266/139598026-2161e769-2f09-412c-8700-0e45c646872a.png" width="300" height="300">

- Their lifetime starts when the scene is loaded in PlayMode
- Their lifetime ends when the scene is unloaded / another scene is loaded
- If the scene is currently open, it will automatically be loaded, when PlayMode is entered

---

## 2. Part of a Prefab

- `GameObjects` can be part of a prefab
- They exist as a Template in the prefab edit mode
<img width="371" alt="image" src="https://user-images.githubusercontent.com/7360266/139598052-ccc56543-0de3-4359-965a-c587d2ad79cd.png">

- They are saved in the .prefab prefab file
<img src="https://user-images.githubusercontent.com/7360266/139598026-2161e769-2f09-412c-8700-0e45c646872a.png" width="300" height="300">

- Their lifetime starts when you Instantiate the prefab during PlayMode
- Their lifetime ends when 
  - you Destroy it
  - another scene is loaded 
    - (which will also destroy all current Scene's GameObjects)
  - you quit the game
    - (which will also unload the current Scene)

---

## 3. Created Through Code

- `GameObjects` can be created through a script
- Their lifetime starts when your code gets executed
- Their lifetime ends when you Destroy it / another scene is loaded


```cs
void CreateView() {
   GameObject parent = new GameObject("GoldView");
   GameObject child = new GameObject("GoldViewChild");
   child.transform.SetParent(parent.transform);
   Text text = child.AddComponent<Text>();
   text.text = "This is a gold view.";
}
```

- This is not a recommended way, as designers can not make changes to GameObjects this way

---

## 4. GameObject Lifetime (simplified)
   
- You can react to a `GameObject`‘s lifetime
- Using Unity Event functions
- You just need to define a parameterless method with that matching name:
```cs
void Awake() {
   CreateView();
}
```
   
- They get executed automatically in this order:

First time Loaded and Enabled:
```cs
Awake();
Start();
OnEnable();
```

Every Frame while Enabled:
```cs
Update();
LateUpdate();
```

50x per second while enabled:
```cs
FixedUpdate();
FixedUpdate();
```

Everytime when Disabled:
```cs
OnDisable();
```

Everytime when Enabled again:
```cs
OnEnable();
```

Once, when Unloading (through Destroying the GameObject, Changing the Scene or Quitting the Game):
```cs
   OnDisable();
   OnDestroy();
}
```


---

## 5. Disabling GameObjects

You can disable GameObjects:

Through the Editor:

<img width="258" alt="image" src="https://user-images.githubusercontent.com/7360266/139598294-ff36d56e-95ab-41bf-a505-30fac10d4c20.png">

Through Code:

Activate:
```cs
gameObject.SetActive(true);
```

Deactivate:
```cs
gameObject.SetActive(false);
```

This checks if the gameObject itself is marked to be active
```cs
Debug.Log(gameObject.activeself);
```

This checks if the gameObject is really enabled:
- (because it could also be disabled indirectly, because it's parent is disabled)
```cs
Debug.Log(gameObject.activeInHierarchy);
```

Disabling `GameObjects` automatically disables that `GameObject's` children as well:

<img width="369" alt="image" src="https://user-images.githubusercontent.com/7360266/139598301-1f67ec2e-68fe-438d-af00-7841c6024ec7.png">

---

## 6. Destroying GameObjects

You can destroy GameObjects:

Through the Editor:

<img src="https://user-images.githubusercontent.com/7360266/139598344-80b3ece6-5535-41d4-ac56-47db52bf83f7.png" width="400" height="290">

Through Code:

```cs
Destroy(gameObject);
```
   
This is necessary in very special occasions, when you are executing this code from an editor extension instead of within your game logic:

```cs
DestroyImmediate(gameObject);
```
   
Destroying GameObjects automatically destroys that GameObject‘s children as well!

---

## 7. Instantiating Prefabs

You can instantiate Prefabs:

Through the Editor:
- Drag from the Project View to the scene Hierarchy

<img src="https://user-images.githubusercontent.com/7360266/139598374-b93fec5f-82d8-4683-96d9-7db776d80aec.png" width="400" height="300">

Through Code:

```cs
// you can assign the prefab in the inspector of this script
public GameObject prefab;

void Sample(GameObject gameObject) {
   // instantiates the prefab as root game Object:
   Tnstantiate(prefab);
   // instantiates the prefab as a child of the gameObject:
   Instantiate(prefab, gameObject.transform);
   // instantiates the prefab at the given position and rotation:
   Instantiate(prefab, new Vector3(1, 2), Quaternion.Euler(1, 2, 3));
   
   // you can save a reference of the new instance to a variable:
   GameObject instance = Instantiate(prefab);
   // and then work with it:
   instance.name = "Cool Name";
}
```
When Instantiating a Prefab, it becomes part of the currently active scene

---

## 8. (Un-)Loading Scenes

You can Load a Scene:

Through the Editor:
- by double clicking on a scene, or Drag Dropping the scene to the hierarchy

<img src="https://user-images.githubusercontent.com/7360266/139598427-2b365b95-0e49-4998-9d1b-bef154dfeaf1.png" width="400" height="330">

Through Code:

You can unload a loaded scene:
```cs
SceneManager.UnloadSceneAsync("Level01");
```

You can load a new scene without unloading the current one:
```cs
SceneManager.LoadScene("HUD", LoadSceneMode.Additive);
```
   
You can load a new scene while unloading all current ones:
```cs
SceneManager.Loadscene("Level02");
```

You can flag gameObjects to never be unloaded on scene switches:
```cs
DontDestroyOnLoad(gameObject);
```

Now, if you change the Scene, the GameObject that you have flagged will still be loaded.\
You can also clearly see, that it has been flagged through the Scene-View:

<img width="305" alt="image" src="https://user-images.githubusercontent.com/7360266/139600948-cb461b19-c999-4c2a-8f4f-5e0aed69b7fb.png">



- When Loading a Scene
  - All Current Scenes Are Unloaded
    - Unless you Load it Additively
- When a Scene is Unloaded
  - All GameObjects belonging to that scene will be destroyed
  - Unless, they are flagged as `DontDestroyOnLoad`

### Important Note!

All Scenes that you want to load within your Game, need to be added to the Game's Build Settings first:

<img width="635" alt="image" src="https://user-images.githubusercontent.com/7360266/139601100-4d9973e8-1231-4b08-9559-557ed665618f.png">

Access the Build Settings through the Menu File > Build Settings and Drag and Drop all Scenes that you need for your Game here.\
Make sure, to put the Scene first (At Index 0), that you want to start the Game with.

---

## 9. Quitting the Game

You Can Quit the Game:

Through the Editor:

<img width="250" alt="image" src="https://user-images.githubusercontent.com/7360266/139600892-c4946d49-8e24-477d-a94e-cec839fe3590.png">

Through Code:

```cs
Application.Quit();
```
