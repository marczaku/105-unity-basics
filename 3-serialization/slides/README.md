# Slides 3 - Scripting

# 3. Serialization

## What's Serialization?

### Serialization

<img width="458" alt="image" src="https://user-images.githubusercontent.com/7360266/139729499-5ebbb37e-2f5e-4d05-938d-0cb5969c369f.png">

- Our objects are stored in Runtime Memory (RAM)
  - The RAM is FAST, but: SMALL and NON-PERSISTENT
- On our Hard Drive, we can Write Bytes
  - The Hard Drive is SLOW, but: LARGE and PERSISTENT
- Over our Network Adapter, we can also send Bytes

That's what Serialization is:

- The process of
  - READING an object from Memory
  - CONVERTING IT to bytes

### Types of Serialization

Let's look at ways of serializing following object:

```cs
int i = int.MaxValue;
```

#### String Serialization

Idea: We convert the object to a human-readable string and save that.

```cs
File.WriteAllText("save", i.ToString());
```

- Result: `"2147483647"`
- Encoding: ASCII
- Binary: `00110010 00110001 00110100 00110111 00110100 00111000 00110011 00110110 00110100 00110111 `
- Size: 10 bytes

Advantages:
- Human-Readable
- Better merge-able in git

#### Binary Serialization

```cs
File.WriteAllBytes("save", BitConverter.GetBytes(i));
```

Result: `"ÿÿÿ�"`
Binary: `11111111 11111111 11111111 01111111`
Size: 4 bytes (size of `int`)

Advantages:
- More Compressed
- Better Performance

### Deserialization

<img width="459" alt="image" src="https://user-images.githubusercontent.com/7360266/139729749-1808d698-0721-4253-98e7-e389c5974aee.png">

- From our Hard Drive, we can Read Bytes into Memory
  - So we can use it in our Code
- Over our Network Adapter, we can also receive Bytes

But first, we need to use Serialization to convert it to an object in Memory again:

- The process of
  - READING bytes from Hard Drive or Network
  - CONVERTING it to an object in memory

#### String Deserialization

```cs
int i = BitConverter.ToInt32(File.ReadAllBytes("save"));
```

#### Binary Deserialization

```cs
int i = Convert.ToInt32(File.ReadAllText("save"));
```

---

## Unity Serialization

- Unity needs to serialize anything that you CHANGE in the Editor and then SAVE to the Hard Drive:
  - Scenes
  - Prefabs
  - GameObjects
  - Components
  - Project Settings
  - Build Settings
  - Import Settings

This ensures, that:
- your changes still exist even after shutting down Unity or restarting your computer
- you can share your changes with your colleagues over a network

---

## Unity Deserialization

- Unity needs to deserialize anything that you LOAD or OPEN from the Hard Drive
  - Scenes
  - Prefabs
  - GameObjects
  - Components
  - Project Settings
  - Build Settings
  - Import Settings

This ensures, that after starting Unity:
- all your changes are restored to where you left off before you quit Unity.
- all changes downloaded from your colleagues are synchronized with your Editor.

## Unity Serialization & Git

<img width="406" alt="image" src="https://user-images.githubusercontent.com/7360266/139730422-db2a14dd-2494-41bb-a173-f70fdf7fc38d.png">

- Basically, anything that you can commit with GIT has been serialized:
- If you make changes in the editor, there is nothing to commit, yet
- If you save your changes, the changes are serialized and written to the Hard Drive, so you can commit them

## YAML

```
YAML is a human-friendly data serialization
  language for all programming languages.
```

It is a standardized approach to serializing objects in a human-readable way.

Before standards like YAML, every company would "invent" their own interpretations of how to serialize an object.

e.g. an Object like:

```cs
public class Player{
  string name = "Marc";
  int health = 5;
  int strength = 12;
}
```

Might be serialized by one company as:

```
Marc:5,12
```

But by another one:

```
Name:Marc;Health:5;Strength:12
```

In YAML, it would look like this:

```yaml
name: Marc
health: 5
strength: 12
```

## An Empty Scene

If we Create a new Scene In Unity, Save it as `EmptyScene` and then open the `EmptyScene.unity` File in a Text-Editor, we will see...

<img width="361" alt="image" src="https://user-images.githubusercontent.com/7360266/139732013-c31df840-5781-40fe-8934-f18d741d1c7c.png">

It is not so empty!\
The format that we see the information written in here, is called `YAML`

If we fold all the information, we can see...
- It‘s just per scene settings that can be grouped into four categories:
  - Occlusion Culling
  - Render
  - Lightmap
  - NavMesh

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139732098-18253f00-8108-4a16-98c2-4531b7617ca4.png">

Those are Per-Scene Settings. As in, settings that can be edited for each scene.
- You can find most of these settings under Window > Rendering

<img width="385" alt="image" src="https://user-images.githubusercontent.com/7360266/139732118-9d0ee7c5-ec1e-471a-a6e3-d26544ec122a.png">

## GameObject

- A GameObject is serialized by being given a UniqueID (here: `&1049995125`)
- And saving its relevant Fields
  - Layer, Tag
  - Name, IsActive
  - Its Components (as a reference through ist ID)

You will see in the sample, that all Fields are written `m_{FieldName}` this is an old-fashioned way of naming private Member-Fields. `m_` stands for Member.

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139732230-4d69853b-1ed1-4210-868e-d4c4ee062492.png">

- You can find these attributes in the inspector

<img width="279" alt="image" src="https://user-images.githubusercontent.com/7360266/139732215-07c265a8-11f3-4cb6-949d-73860b67dde4.png">

## Components

<img width="278" alt="image" src="https://user-images.githubusercontent.com/7360266/139732505-53115f51-e66d-4b79-ab1f-fccf152f43d2.png">

- Every `GameObject` has a `Transform` in Unity!
- It is saved as one of its Components
- The gameObject has a reference to its component: `- component: {fileID: 1049995127}`
- The component has an ID: `--- !u!4 &1049995127`
- The component has a reference to its gameObject: `- m_GameObject: {fileID: 1049995125}`

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139732511-5352d036-4006-40e1-898b-f494e5a928ef.png">

This is, how Unity knows, when Loading the Scene, which Component belongs to which GameObject.

## Transform Component

- A transform has its values serialized:
  - localRotation
  - localPosition
  - localScale
  - m_Children (all the child transforms of this transform)
  - m_Father: (the parent transform of this transform)

<img width="280" alt="image" src="https://user-images.githubusercontent.com/7360266/139732695-5b165ad3-638f-498a-ae59-1e02981669d0.png">

<img width="167" alt="image" src="https://user-images.githubusercontent.com/7360266/139732705-3b792981-aa21-4e83-94be-ed3752fc5258.png">

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139732722-fae4342e-9576-46e7-9006-b63f248c49e2.png">

## Monobehaviour Component

<img width="362" alt="image" src="https://user-images.githubusercontent.com/7360266/139732811-937cca22-076c-46b1-b53d-293b846b7230.png">

- If your Class inherits from MonoBehaviour
- Then you can attach it to any GameObject
- Also multiple times, if you like

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139732820-f4f73cb7-ee79-4705-8237-eefdc50b9997.png">

- It will be serialized as one of its components
- And it will be serialized itself as MonoBehaviour

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139732833-d514f105-2d05-476e-bb3f-77b1532f4813.png">

- `m_Script` will point at the GUID of your script
- which is stored in the `.meta` File with the same Name
  - e.g. `Vehicle.cs` has a matching `Vehicle.cs.meta`-File.
  - Unity creates these files automatically when you add a new File to the Project

<img width="342" alt="image" src="https://user-images.githubusercontent.com/7360266/139732792-80437530-32b1-42d6-b8fc-10e54f8fece0.png">

### Fields

- Fields of your MonoBehaviours will be serialized if:
  - They are serializable (more on that later)
  - They are `public` (implicit) OR
  - They have an explicit [SerializeField] Attribute

Public fields are automatically serialized by Unit:

```cs
public int publicField = 6;
```

If you want to prevent this, you can use the `[HideInInspector]`-Attribute:

```cs
[HideInInspector] public int publicUnSerializedField = 6;
```

Private Fields are not serialized:

```cs
private int privateField = 12;
```

Unless you mark them with a `[SerializeField]`-Attribute:
```cs
[SerializeField] private int privateSerializedField = 7;
```

Static Fields are not serialized:
```cs
public static int publicStaticField = 12;
```

You will find your Serialized Fields in the Inspector. This allows you to change the values that are stored on your instance of the script:

<img width="439" alt="image" src="https://user-images.githubusercontent.com/7360266/139733238-d75e5fd6-e7c8-4b4d-9dfa-2a1600beb262.png">

You will then find your serialized fields saved in your serialized `MonoBehaviour`-Object in the Scene-File:

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139733244-29767576-605a-46f2-9a4c-05f9820270a7.png">

### Methods

Methods are not serialized.

```cs
public void GetMaxHealth() => 100;
```

### Properties

Properties are NOT serialized.
- They are technically just Getter & Setter Methods (Remember?)
- And Methods are not serialized

```cs
public int PublicProperty { get; set; }
```

### Classes

Your Custom Classes will not be serialized
- Even, if they are used as Type of a public field
```cs
public class SomeClass {
  public int someValue = 13;
}

public SomeClass classInstance;
```

Unless you mark the class explicitly
- With a [System.Serializable] Attribute
- Unity will now know, that you want to Serialize this Class, whenever it is used as a serialized field

```cs
[System.Serializable]
public class SerializableClass {
  public int someValue = 27;
}
 
  public SerializableClass serializableClassInstance;
```

Again, you will find the Serializable Field both in the Inspector and the Scene-File:

<img width="451" alt="image" src="https://user-images.githubusercontent.com/7360266/139734517-7b62b4f8-e5ac-461f-9f70-dda1ab5e4055.png">

<img width="385" alt="image" src="https://user-images.githubusercontent.com/7360266/139734533-e79bc4b2-7039-4766-89a8-abbe4f0cc424.png">

#### Deserialization does not maintain same object instances:

Warning: Classes are always deserialized as copies, not as shared instances:

```cs
public class Weapon{
  public string name;
}

```cs
public class Player : UnityEngine.MonoBehaviour{
   public Weapon weapon;
}
```

```cs
Weapon weapon = new Weapon();
player1.weapon = weapon;
player2.weapon = weapon;
player1.weapon.name = "Axe";
```

After above script, both players will have a reference to the same weapon instance with the name `"Axe"`.

But after reloading the scene, if you were to do:

```cs
player1.weapon.name = "Dagger";
```

Then only player1's Weapon's name would change.

#### Deserialization does not maintain `null` values

Also, Unity always creates an object instance when deserializing a Field:

```cs
player1.weapon = null;
```

After reloading the scene, if you were to print:

```cs
Debug.Log(player.weapon == null);
```

It would print:
```
false
```

### UnityEngine.Object

- Fields of a Type inheriting from `UnityEngine.Object` are serialized as References
- That is because `UnityEngine.Objects` are serialized themselves as part of either
  - The Project, e.g. `Texture2D`, `Sprite`
  - A Scene, e.g. `Camera` on the Main Camera in a Scene.
  - A Prefab, e.g. `BoxCollider` on a Prefab Asset.

```cs
public HelloWorld helloWorld;
public GameObject gameObject;
public Object anyObject;
```

You can then reference Instances through the Circle Icon, or by Drag-Dropping an Asset or a Scene-Instance to the Field.

<img width="427" alt="image" src="https://user-images.githubusercontent.com/7360266/139744520-692ecb0c-8833-4100-9012-e5929b12dd9a.png">

- The GUID is Unique to that Object
  - either again through the `.meta`-File, if it is an Asset 
  - or the serialized Id that we've seen earlier in the Scene-File.

<img width="446" alt="image" src="https://user-images.githubusercontent.com/7360266/139744531-630060cf-0ddc-4397-9824-40a6a5b287f1.png">


### Arrays

Unity can serialize Arrays. This is very useful to serialize anything that you want to have a variable count of, e.g.
- an `int[]` for the experiences needed for level up
  - there might be only 10 levels in your Game now, but it might be 20 later.
- a `string[]` containing AI player names, which will be randomized when

```cs
public int[] publicArrayField = {
  0x11223344, 3, 2
};
public string[] publicStringArrayField = {
  "Hello", "world", "!"
};
```

```cs
public SerializableClass[] publicSerializableClassArray = {
  new SerializableClass(12),
  new SerializableClass(19),
};
```

```cs
public GameObject[] monsterPrefabs;
public Transform[] spawnPoints;
```

You can edit them in the Inspector.

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139744269-463899ff-9ba2-4f5a-ba39-a5f8f257e5b6.png">

And they are saved in the Scene-File.

<img width="393" alt="image" src="https://user-images.githubusercontent.com/7360266/139744258-3eea0a0b-b93a-4582-93c0-d1f27658181f.png">

#### Others

- Unity can serialize fields of Type:
  - `List<T>`
  - Generic Classes
- Unity can not serialize fields of Type:
  - `Dictionary<TKey, TValue>`
  - Interfaces :(
  - Delegate

---

# 4. GameObject Lifetime

## Part of a Scene

`GameObjects` can be part of a scene
- They exist as a Template in the editor
<img width="371" alt="image" src="https://user-images.githubusercontent.com/7360266/139598025-c116ed41-6018-46bb-8d30-d92de314978e.png">

They are saved in the .unity scene file
<img src="https://user-images.githubusercontent.com/7360266/139598026-2161e769-2f09-412c-8700-0e45c646872a.png" width="300" height="300">

- START: when the scene is loaded in PlayMode
- DESTROY: 
  - you destroy it via code: `Destroy(gameObject);`
  - the scene is unloaded / another scene is loaded
    - you can prevent this by invoking: `DontDestroyOnLoad(gameObject);`
  - you quit the game (which unloads the scene)

You should use scene to create separate levels / menus of your game which can be loaded one by one or additively.


### (Un-)Loading Scenes

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

### Build Settings

All Scenes that you want to load within your Game, need to be added to the Game's Build Settings first:

<img width="635" alt="image" src="https://user-images.githubusercontent.com/7360266/139601100-4d9973e8-1231-4b08-9559-557ed665618f.png">

Access the Build Settings through the Menu File > Build Settings and Drag and Drop all Scenes that you need for your Game here.\
Make sure, to put the Scene first (At Index 0), that you want to start the Game with.

## Part of a Prefab

`GameObjects` can be part of a prefab
- They exist as a Template in the prefab edit mode
<img width="371" alt="image" src="https://user-images.githubusercontent.com/7360266/139598052-ccc56543-0de3-4359-965a-c587d2ad79cd.png">

They are saved in the `.prefab` file
<img src="https://user-images.githubusercontent.com/7360266/139598026-2161e769-2f09-412c-8700-0e45c646872a.png" width="300" height="300">

- START: when you Instantiate the prefab during PlayMode
- DESTROY: same as before

```cs
using UnityEngine;
public class MonsterSpawner : MonoBehaviour {
  // reference assigned through Inspector
  public GameObject monsterPrefab;

  void FixedUpdate() {
    Instantiate(monsterPrefab);
  }
}
```

You should use prefabs when you want to instantiate objects during runtime. You can then dynamically instantiate the prefab:
- 0 times, if you don't need it right now
- 1 time, if you only need one instance
- 100 times, if you want to spawn e.g. a hundred monsters


### Instantiating Prefabs

You can instantiate Prefabs:

Through the Editor:
- Drag from the Project View to the scene Hierarchy

<img src="https://user-images.githubusercontent.com/7360266/139598374-b93fec5f-82d8-4683-96d9-7db776d80aec.png" width="400" height="300">

Through Code:

```cs
// you can assign the prefab in the inspector of this script
public GameObject prefab;

void Sample(GameObject gameObject) {
   // instantiates the prefab with no parent
   Tnstantiate(prefab);
   // instantiates the prefab as a child of the gameObject
   Instantiate(prefab, gameObject.transform);
   // instantiates the prefab at the given position and rotation:
   Instantiate(prefab, new Vector3(1, 2, 0), Quaternion.Euler(1, 2, 3));
   
   // you can save a reference of the new instance to a variable:
   GameObject instance = Instantiate(prefab);
   // and then work with it:
   instance.name = "Cool Name";
}
```

When Instantiating a Prefab, it becomes part of the currently active scene.
- which means, if that scene is unloaded, the prefab instance will be destroyed as well

## Creating GameObjects

`GameObjects` can be created through a script
- START: when your code gets executed
- DESTROY: same as before


```cs
void CreateView() {
   GameObject goldView = new GameObject("GoldView");
   GameObject goldLabel = new GameObject("GoldItem");
   goldLabel.transform.SetParent(goldView.transform);
   Text text = goldLabel.AddComponent<Text>();
   text.text = "This is a gold view.";
}
```

This is not a recommended way, since designers can't contribute to GameObject configurations this way.

## Destroying GameObjects

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

## GameObject Lifetime Events
   
- You can react to a `GameObject`‘s lifetime
- Using Unity Event functions
- You just need to define a parameterless method with that matching name:
```cs
void Awake() {
   CreateView();
}
```
   
- They get executed automatically in this order:

First time Loaded:
```cs
Awake();
```

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

50x (unless changed) per second while enabled:
```cs
FixedUpdate();
FixedUpdate();
```

Every time when Disabled:
```cs
OnDisable();
```

Every time when Enabled again:
```cs
OnEnable();
```

Once, when Unloading (through Destroying the GameObject, Changing the Scene or Quitting the Game):
```cs
   OnDisable(); // if enabled before
   OnDestroy(); // if awoken before
}
```

## Disabling GameObjects

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

Disabling `GameObjects` automatically disables that `GameObject's` children as well:

<img width="369" alt="image" src="https://user-images.githubusercontent.com/7360266/139598301-1f67ec2e-68fe-438d-af00-7841c6024ec7.png">

This checks if the gameObject itself is marked to be active
```cs
Debug.Log(gameObject.activeSelf);
```

This checks if the gameObject is really enabled:
- it could be `activeSelf: true`, but its parent is disabled
```cs
Debug.Log(gameObject.activeInHierarchy);
```

## Quitting the Game

You Can Quit the Game:

Through the Editor:

<img width="250" alt="image" src="https://user-images.githubusercontent.com/7360266/139600892-c4946d49-8e24-477d-a94e-cec839fe3590.png">

Through Code (in a built application):

```cs
Application.Quit();
```

Above code sample only works in a built application. If you want to stop the PlayMode, you need to use:

```cs
UnityEditor.EditorApplication.ExitPlaymode();
```

However, above code won't compile when you try building the game... To have a solution for all cases, just use the following code snippet:

```cs
#if UNITY_EDITOR
UnityEditor.EditorApplication.ExitPlaymode();
#else
Application.Quit();
#endif
```

It makes sure, that your game only contains the correct code in both cases, Editor and built application.