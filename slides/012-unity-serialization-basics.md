## 1. Serialization

<img width="458" alt="image" src="https://user-images.githubusercontent.com/7360266/139729499-5ebbb37e-2f5e-4d05-938d-0cb5969c369f.png">

- **The process of **
  - READING an object from Memory
  - CONVERTING IT to bytes
- Bytes may be readable text or Binary
- We can save Bytes or send them
- RAM is FAST, but SMALL and RESETS
- HARD DRIVE is SLOW, but LARGE and PERSISTENT
---

## 2. Deserialization

<img width="459" alt="image" src="https://user-images.githubusercontent.com/7360266/139729749-1808d698-0721-4253-98e7-e389c5974aee.png">

- **The process of **
  - READING bytes from Hard Drive or Network
  - CONVERTING it to an object in memory

---

## 3. Unity Serialization

<img width="299" alt="Screenshot 2021-11-01 at 20 31 54" src="https://user-images.githubusercontent.com/7360266/139730157-7c85663d-2a0c-4dab-a68d-17b6fda330d6.png">

- Unity needs to serialize anything that you CHANGE in the Editor and then SAVE
  - Scenes
  - Prefabs
  - GameObjects
  - Components
  - Project Settings
  - Build Settings
  - Import Settings

---

## 4. 

<img width="299" alt="Screenshot 2021-11-01 at 20 31 54" src="https://user-images.githubusercontent.com/7360266/139730259-128fb066-9a4d-4bae-b852-7a1e2052b6cb.png">

- Unity needs to deserialize anything that you LOAD or OPEN from the Hard Drive
  - Scenes
  - Prefabs
  - GameObjects
  - Components
  - Project Settings
  - Build Settings
  - Import Settings

---

## 5. GIT

<img width="406" alt="image" src="https://user-images.githubusercontent.com/7360266/139730422-db2a14dd-2494-41bb-a173-f70fdf7fc38d.png">

- Basically, anything that you can commit with GIT has been serialized:
- If you make changes in the editor, there is nothing to commit, yet
- If you save your changes, the changes are serialized and written to the Hard, so you can commit them

---

## 6. Empty Scene

<img width="361" alt="image" src="https://user-images.githubusercontent.com/7360266/139732013-c31df840-5781-40fe-8934-f18d741d1c7c.png">

- Not so empty!

---

## 7. Empty Scene - 2

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139732098-18253f00-8108-4a16-98c2-4531b7617ca4.png">

<img width="385" alt="image" src="https://user-images.githubusercontent.com/7360266/139732118-9d0ee7c5-ec1e-471a-a6e3-d26544ec122a.png">

- It‘s just per scene settings that can be grouped into four categories:
  - Occlusion Culling
  - Render
  - Lightmap
  - NavMesh
- You can find most of these settings under Window > Rendering

---

## 8. GameObject

<img width="279" alt="image" src="https://user-images.githubusercontent.com/7360266/139732215-07c265a8-11f3-4cb6-949d-73860b67dde4.png">

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139732230-4d69853b-1ed1-4210-868e-d4c4ee062492.png">

- A GameObject is serialized by being given a UniqueID (&1049995125)
- And saving its relevant Attributes
  - Layer, Tag
  - Name, IsActive
  - Its Components (as a reference through ist ID)
- You can find these attributes in the inspector
---

## 9. Components

<img width="278" alt="image" src="https://user-images.githubusercontent.com/7360266/139732505-53115f51-e66d-4b79-ab1f-fccf152f43d2.png">

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139732511-5352d036-4006-40e1-898b-f494e5a928ef.png">

- Every GameObject has a Transform in Unity!
- It is saved as one of its Components
- The gameObject has a reference to its component:
  - component: {fileID: 1049995127}
- The component has an ID: --- !u!4 &1049995127
- The component has a reference to its gameObject:
  - m_GameObject: {fileID: 1049995125}

---

## 10. Transform

<img width="280" alt="image" src="https://user-images.githubusercontent.com/7360266/139732695-5b165ad3-638f-498a-ae59-1e02981669d0.png">

<img width="167" alt="image" src="https://user-images.githubusercontent.com/7360266/139732705-3b792981-aa21-4e83-94be-ed3752fc5258.png">

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139732722-fae4342e-9576-46e7-9006-b63f248c49e2.png">

- A transform has its values serialized:
  - localRotation
  - localPosition
  - localScale
  - m_Children (all the child transforms of this transform)
  - m_Father: (the parent transform of this transform)

---

## 11. Monobehaviour

<img width="342" alt="image" src="https://user-images.githubusercontent.com/7360266/139732792-80437530-32b1-42d6-b8fc-10e54f8fece0.png">

<img width="362" alt="image" src="https://user-images.githubusercontent.com/7360266/139732811-937cca22-076c-46b1-b53d-293b846b7230.png">

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139732820-f4f73cb7-ee79-4705-8237-eefdc50b9997.png">

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139732833-d514f105-2d05-476e-bb3f-77b1532f4813.png">

- If your Class inherits from MonoBehaviour
- Then you can attach it to any GameObject
- Also multiple times, if you like
- It will be serialized as one of its components
- And it will be serialized itself as MonoBehaviour
- Script will point at the GUID of your script

---

## 11. Fields

```cs
// public fields are serialized:
public int publicField = 6;
// private fields are not serialized:
int privateField;
//SerializeField makes sure that private fields are
// serialized and shown in the inspector
// it is useful to avoid that other classes
// have access to this field
[SerializeField]
private int privateSerializedField = 7;
```

<img width="439" alt="image" src="https://user-images.githubusercontent.com/7360266/139733238-d75e5fd6-e7c8-4b4d-9dfa-2a1600beb262.png">

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139733244-29767576-605a-46f2-9a4c-05f9820270a7.png">

- Fields of your MonoBehaviours will be serialized if:
  - They are a basic type AND:
  - They are Public (implicit) OR
  - They have an explicit [SerializeField] Attribute
- They will not be serialized if:
  - They are private and do not have an attribute
  - They are static

---

## 12. Properties

```cs
// properties are not serialized
public int PublicProperty {get; set; }
```

- Properties are NOT serialized
- They are technically just Getter & Setter Methods
- And Methods are not serialized

---

## 13. Classes

```cs
// classes are not serialized
public class SomeClass {
  public int someValue = 13;
}

// unless you marc it as Serializable:
[System.Serializable]

public class Serializable Class {
   public int someValue = 27;
   public SerializableClass(int someValue) {
      this.someValue = someValue;
    }
 }
 
 public SerializableClass serializableClassInstance;
```
   
<img width="451" alt="image" src="https://user-images.githubusercontent.com/7360266/139734517-7b62b4f8-e5ac-461f-9f70-dda1ab5e4055.png">

<img width="385" alt="image" src="https://user-images.githubusercontent.com/7360266/139734533-e79bc4b2-7039-4766-89a8-abbe4f0cc424.png">

- Your Custom Classes will not be serialized
- Even, if they are used as Type of a public field
- Unless you mark the class explicitly
  - With a [System.Serializable] Attribute
- Unity will now know, that you want to Serialize this Class, whenever it is used as a serialized field

---

## 14. Arrays

```cs
// arrays can be serialized:
public int[] publicArrayField = {
  0x11223344, 3, 2
  };
public string[] publicStringArrayField = {
  "Hello", "world", "!"
  };
public SerializableClass[] publicSerializableClassArray = {
  new SerializableClass(12),
  new SerializableClass(19),
};
```
<img width="393" alt="image" src="https://user-images.githubusercontent.com/7360266/139744258-3eea0a0b-b93a-4582-93c0-d1f27658181f.png">

<img width="353" alt="image" src="https://user-images.githubusercontent.com/7360266/139744269-463899ff-9ba2-4f5a-ba39-a5f8f257e5b6.png">

- Unity can serialize Arrays of SerializableTypes
  - Arrays of Basic Types
  - Arrays of Serializable Classes
---

## 15. Unityengine.object

```cs
// unityengine.object can be serialized:
// here, you can drop referances to:
// other scripts, gameobjects, assets
// either from scenes/prefabs or the project view
public HelloWorld helloWorld;
public GameObject gameObject;
public Object anyObject;
```

<img width="427" alt="image" src="https://user-images.githubusercontent.com/7360266/139744520-692ecb0c-8833-4100-9012-e5929b12dd9a.png">

<img width="446" alt="image" src="https://user-images.githubusercontent.com/7360266/139744531-630060cf-0ddc-4397-9824-40a6a5b287f1.png">

- Fields of UnityEngine.Objects are serialized as References
- That is because UnityEngine.Objects are serialized themselves as part of
  - The Project
  - A Scene
  - A Prefab
- The GUID is Unique to that Object

---

## 16. Other Cases

- UNITY CAN SERIALIZE:
  - List<T>
- UNITY CAN NOT SERIALIZE:
  - Dictionary<TKey, TValue>
  Any other GenericClass<T>
  Interfaces

- => THERE IS SOME UPDATES WITH UNITY 2020, THOUGH!
---
