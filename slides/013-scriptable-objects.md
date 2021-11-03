## 1. Classes

Classes serialized as Fields of Components...

```cs
[System.Serializable]
public class Level {
   public int level;
   public int experience;
}
```

```cs
public class Hero : NonoBehaviour {
   public level Level;
}
 ```
 
 ...become a part of the Component that they are part of:

<img width="382" alt="image" src="https://user-images.githubusercontent.com/7360266/139946588-1e981919-a08c-4c58-8b98-109bc29b0808.png">

<img width="382" alt="image" src="https://user-images.githubusercontent.com/7360266/139946601-bfc61709-1c03-46f2-8aeb-dee28ffd5f26.png">

---

## 2. UnityEngine.Object

Classes inheriting from UnityEngine.Object...

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


---

## 3. unityengine.object - 2
- UnityEngine.Object is not very useful by itself
<img width="389" alt="image" src="https://user-images.githubusercontent.com/7360266/139946991-503a814e-62fa-416c-9218-e3ab6dd06f91.png">

- It contains basic logic for resource management by Unity (`Destroy`, `Instantiate`, ...)
- It has a `name`
- The Equality Operator `==` has been overloaded to check, if a `UnityEngine.Object` was destroyed

---

## 4. Object and Null

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

In reality, though, any class inheriting from `UnityEngine.Object` needs to be explicitly destroyed using `Object.Destroy`, though:

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

---

## 5. Object Lifetime

Method | Function | C# | C++
------ | -------- | -- | ---
ObjectFactory.CreateInstance | A new instance of the Object is created | A new instance of the class is created | A C++ Object with instanceId and Pointer is Created
Instantiate | An existing Object gets cloned | New instance of the class + copy values | A C++ Object with instanceId and Pointer is Created
Destroy | An existing Object gets destroyed | Nothing happens | The C++ Object gets destroyed, the InstanceId and Pointer are Freed
= null | The reference to the C# class gets lost | The Garbage Collector cleans up the class | Nothing happens


<img width="440" alt="image" src="https://user-images.githubusercontent.com/7360266/139948820-231e1e5c-e170-43bb-8b0d-6001d53230b0.png">
<sup>Living Object C#</sup>

<img width="418" alt="image" src="https://user-images.githubusercontent.com/7360266/139951820-5ebe602b-3bd6-4257-bdbb-88e1aa97596c.png">

<sup>== null ? FALSE</sup>
<p></p>
<img width="436" alt="image" src="https://user-images.githubusercontent.com/7360266/139948936-92a408a5-1276-446a-87b1-a6390c8eef18.png">
<sup>Destroyed Object C#</sup>
<img width="409" alt="image" src="https://user-images.githubusercontent.com/7360266/139948970-c1e273f1-e0c6-4fb1-8f42-b673fa5c249b.png">
<sup>== null ? TRUE</sup>

---

## 7. UnityEngine.Object useless?

![image](https://user-images.githubusercontent.com/7360266/139949199-615c38ef-1e20-4157-a730-59eb9cda47b0.png)

<img width="321" alt="Screenshot 2021-11-02 at 22 10 30" src="https://user-images.githubusercontent.com/7360266/139952154-b091ce27-6eff-4e3e-9d53-f9157fd43ca0.png">

- UnityEngine.Object by Itself is a useless class, though!
- It contains some basic Logic like
  - Drag & Drop
  - InstanceID & C++ Pointer
  - Name
---

## 8. UnityEngine.Object Inheritors

<img width="339" alt="Screenshot 2021-11-02 at 21 48 27" src="https://user-images.githubusercontent.com/7360266/139949437-784f552e-ea2d-4aab-b2e8-06948b7b0169.png">

- Many Assets Inherit from UnityEngine.Object Though!
- And have custom Unity C# and C++ Code to work
  - SceneAsset
  - Texture
  - GameObject
  - …
- They can all exist as an Asset in the ProjectView

---

## 9. MonoBehaviour

<img width="349" alt="Screenshot 2021-11-02 at 21 49 27" src="https://user-images.githubusercontent.com/7360266/139949594-9b10c98e-9527-4ee4-8d7d-1aa201e6fb6c.png">

- A Component is something that can be attached to a GameObject
- A Behaviour is a Component that can be enabled and disabled
- A MonoBehaviour is the base class for all our C# Scripts and brings some features like Invoke and Coroutines

---

## 10. Monobehaviour - 2

<img width="633" alt="image" src="https://user-images.githubusercontent.com/7360266/139949713-2b645efb-29c7-453b-ab60-397f4d4162db.png">

<img width="330" alt="image" src="https://user-images.githubusercontent.com/7360266/139949758-c335513a-aaab-4d22-8d18-9018e6e67e19.png">

<img width="223" alt="image" src="https://user-images.githubusercontent.com/7360266/139949771-247ba7a6-3be0-4fa1-b7e7-432a4a4b8742.png">

- We can not inherit from Component or Behaviour Directly
- Because only MonoBehaviours can be serialized properly, referencing our Scripts
- Without having to write C++ Code

- Unity Components like Transform inherit from Component or Behaviour
- To improve performance

---

## 11. Our options so far

<img width="227" alt="image" src="https://user-images.githubusercontent.com/7360266/139949951-314b40e3-fba5-4042-a141-d65d2b297179.png">

<img width="175" alt="image" src="https://user-images.githubusercontent.com/7360266/139949956-3f21f2a4-f9d1-44e3-a18f-b1456d7fe19a.png">

<img width="393" alt="image" src="https://user-images.githubusercontent.com/7360266/139949965-bc91fd7a-def4-452a-bbf2-0f2140d3b593.png">

<img width="392" alt="image" src="https://user-images.githubusercontent.com/7360266/139949979-59085fd3-110f-480e-8694-433d41b92767.png">

- Scenes & Assets: Are part of a project
- GameObjects: Are part of a scene or prefab
- Components: Are part of GameObjects
- Custom Class: 
  - Can be used by components
  - Can be used by code using new()
  - Can be serialized, if [System.Serializable]
- Custom MonoBehaviour:
  - Can be used like any other component (Add to GameObject in Editor or through Script)- 

---

## 12. Things we can / can not do

- Scenes: NO custom scenes can be created
- Assets: ??? <=
- GameObjects: NO custom gameObjects can be created
- Components: We can create custom MonoBehaviours
- Classes: We can create custom classes
- Editors: TBD
- EditorWindows: TBD

---

## 13. Scriptable Objects

```cs
using UnityEngine;
[CreateAssetMenu]
public class Item : ScriptableObject {
   public int price;
   }
   ```
![image](https://user-images.githubusercontent.com/7360266/139950409-51048af1-97c6-4e1d-88ec-4879a5dcf47b.png)

<img width="219" alt="image" src="https://user-images.githubusercontent.com/7360266/139950413-b8f50c20-891a-4e98-95aa-aa7fd78c06ca.png">

<img width="390" alt="image" src="https://user-images.githubusercontent.com/7360266/139950419-0b2052e7-5798-4984-99bc-5b04eade1582.png">

- Make a class Inherit from ScriptableObject
- Give it a [CreateAssetMenu] Attribute
- You can now rightclick wihin Project View and Create a new Instance of that Class
- The Instance will exist as an Asset in your Project View
- You can select it to view and edit its values in the Inspector

---

## 14. Scriptable Object Serialization

<img width="294" alt="image" src="https://user-images.githubusercontent.com/7360266/139950534-fe37612d-b25c-4d2e-adc8-052bf88a6fb8.png">

![image](https://user-images.githubusercontent.com/7360266/139950541-df3e3326-eb40-4927-916f-5a866f3b4b86.png)

- A new .asset File is created
- As well as a matching .meta File
- The .asset File contains the serialized instance of your class
- Does this remind you of something?

---

## 15. Scriptable object == monobehaviour??

<img width="277" alt="image" src="https://user-images.githubusercontent.com/7360266/139950623-a671c302-e513-4298-bfc2-02e340d289b0.png">

![image](https://user-images.githubusercontent.com/7360266/139950631-ad13f917-50a3-4886-b331-a8d946600dd1.png)

<img width="293" alt="image" src="https://user-images.githubusercontent.com/7360266/139950644-202cb4f4-5385-4979-8463-f2892d82ec82.png">

![image](https://user-images.githubusercontent.com/7360266/139950661-7773f5df-d469-4caa-aa92-f56af0858c15.png)

- Kind of the same thing as a MonoBehaviour saved in a Prefab / Scene

---

## 16. What‘s the difference?

<img width="392" alt="image" src="https://user-images.githubusercontent.com/7360266/139950833-464590c0-e4be-4e28-a491-b0c217201e16.png">

<img width="393" alt="image" src="https://user-images.githubusercontent.com/7360266/139950845-118319ea-4097-4e63-b36e-5e0f80d23815.png">

- A Scriptable Object does not need a GameObject to be attached to
- And it cannot be attached to a GameObject, even if you wanted to
- It has therefore less overhead.
- That‘s it!

---

## 17. Why not use a class instead?

<img width="393" alt="image" src="https://user-images.githubusercontent.com/7360266/139950911-eaf6611f-58a9-4767-ad9b-7f89c0a20bc3.png">

- Classes have even slightly less overhead than ScriptableObjects!
- Classes can be part of MonoBehaviours
- But Classes can not be shared between multiple MonoBehaviours
- ScriptableObjects can be created in the project view
- And then drag‘n‘dropped onto multiple MonoBehaviour-Fields

---

## 18. Common use cases

<img width="527" alt="image" src="https://user-images.githubusercontent.com/7360266/139951199-ec871a16-d1bc-4317-aece-11ab2d393709.png">

- Sharing Variables across Objects / Scenes
- PlayerPrefab Updates PlayerHP ScriptableObject Instance
- Other Elements, like UI and Enemies can reference the same Instance

. . .

- Sharing Events across Objects / Scenes
- PlayerPrefab Invokes OnPlayerDied ScriptableObject Instance
- Other Elements, like UI and Enemies can subscript to the same instance and receive Callbacks

. . .

- Making Systems available across Objects / Scenes

<img width="832" alt="image" src="https://user-images.githubusercontent.com/7360266/139951335-9e3b2a56-3d3e-4c28-8d68-f26d587915ff.png">

---

## 19. Features for free

<img width="258" alt="Screenshot 2021-11-02 at 22 04 06" src="https://user-images.githubusercontent.com/7360266/139951424-37793b0d-e664-494c-a4f1-f8f3303a99f3.png">

- Asset Management: Create, Duplicate, Delete within Unity
- Serialization: Save, Redo, Undo
- Inspector: Edit, Toggle, TextField, Slider, Lists, Color Pickers, …
- Referencing: Meta-File-ID System for safe referencing, even when renamed
- Drag & Drop utility

---
