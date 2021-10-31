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

<img src="https://user-images.githubusercontent.com/7360266/139598181-121cd9e9-5eab-45b3-b1d2-b869182e13ec.png" width="400" height="150">

- `GameObjects` can be created through a script
- Their lifetime starts when your code gets executed
- Their lifetime ends when you Destroy it / another scene is loaded
- This is not a recommended way, as designers can not make changes to GameObjects this way

---

## 4. GameObject Lifetime (simplified)

<img src="https://user-images.githubusercontent.com/7360266/139598257-e03def2b-0b63-42ab-9af4-b49f0d5862d7.png" width="200" height="450">

<img width="217" alt="image" src="https://user-images.githubusercontent.com/7360266/139598271-d5dbbcb0-706f-4580-9de8-0c801038f0ee.png">

- You can react to a `GameObject`‘s lifetime
- Using Unity Event functions
- They get executed automatically in this order:
- You just need to define a parameterless method with that matching name:

---

## 5. Disabling GameObjects


- **You can disable GameObjects:**
  - Through the Editor
  - Through Code
- Disabling `GameObjects` automatically disables that `GameObject‘s children as well:
- 
<img width="258" alt="image" src="https://user-images.githubusercontent.com/7360266/139598294-ff36d56e-95ab-41bf-a505-30fac10d4c20.png">

<img src="https://user-images.githubusercontent.com/7360266/139598298-2d64d567-a771-4cd6-9f41-4605dfe440ea.png" width="400" height="250">

<img width="369" alt="image" src="https://user-images.githubusercontent.com/7360266/139598301-1f67ec2e-68fe-438d-af00-7841c6024ec7.png">

---

## 6. Destroying GameObjects

<img src="https://user-images.githubusercontent.com/7360266/139598344-80b3ece6-5535-41d4-ac56-47db52bf83f7.png" width="400" height="290">

<img width="327" alt="image" src="https://user-images.githubusercontent.com/7360266/139598348-663b534c-dea5-4e7d-abe4-868b6dc5ccec.png">

- **You can destroy GameObjects:**
  - Through the Editor
  - Through Code
- Destroying GameObjects automatically destroys that GameObject‘s children as well

---

## 7. Instantiating Prefabs

<img src="https://user-images.githubusercontent.com/7360266/139598374-b93fec5f-82d8-4683-96d9-7db776d80aec.png" width="400" height="300">

<img src="https://user-images.githubusercontent.com/7360266/139598375-9d23bb11-5c09-4817-bf9b-8e632c79f5ed.png" width="400" height="260">

- **You can instantiate Prefabs:**
  - _Through the Editor:_ drag from the Project View to the scene Hierarchy
  - Through Code
- When Instantiating a Prefab, it becomes part of the currently active scene
---

## 8. Unloading Scenes

<img src="https://user-images.githubusercontent.com/7360266/139598427-2b365b95-0e49-4998-9d1b-bef154dfeaf1.png" width="400" height="330">

<img width="385" alt="image" src="https://user-images.githubusercontent.com/7360266/139598434-581aeee0-db36-4f81-b60c-1308e4e52510.png">

- **You can Load a Scene:**
  - _Through the Editor:_ by double clicking on a scene, or Drag Dropping the scene to the hierarchy
  - Through Code
- When Loading a Scene
- All Current Scenes Are Unloaded
- Unless you Load it Additive
- When a Scene is unloaded, all gameObjects belonging to that scene will be destroyed

---
