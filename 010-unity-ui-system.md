## 1. Canvas

<img width="439" alt="image" src="https://user-images.githubusercontent.com/7360266/138223298-0b4cef49-edd7-4f95-8791-12d7280de7d7.png">

- In order to be able to render Uis, you need a Canvas
- Each canvas will represent one composited image
- Kind of like a real canvas ☺
- You can create it using GameObject > UI > Canvas

---

## 2. Image
![image](https://user-images.githubusercontent.com/7360266/138223460-0b88b3b9-89eb-438f-b2e4-6e247a783044.png)
![image](https://user-images.githubusercontent.com/7360266/138223467-6717d51e-5cc6-4693-a86a-edaa5a737ffb.png)
![image](https://user-images.githubusercontent.com/7360266/138223478-3da0c92d-8488-4e19-b8db-d969f40e8e24.png)
![image](https://user-images.githubusercontent.com/7360266/138223486-2822753a-05bd-4678-8609-d24b3f635a42.png)

![image](https://user-images.githubusercontent.com/7360266/138223446-d336406d-3308-404c-8e8a-860c0eab9aaf.png)

- The most simple UI element is an Image.
- You can select a Sprite to pick an image file from your project. Try it now!
- You can change the color to tint the image.
- You can apply a material for cool effects.

---

## 3. All UI Elements

![image](https://user-images.githubusercontent.com/7360266/138223527-06ad0f32-92ed-46d1-a554-bb7bc016cbef.png)

- In all UI Elements, you can decide, whether they should be a Raycast Target
  - This will make them clickable
  - It will also make sure that you do not click through them
  - So, menu background images should be Raycast Targets!
- You can decide to make them Unmaskable

---

## 4. Rect Transform

<img width="438" alt="image" src="https://user-images.githubusercontent.com/7360266/138223645-ed20cf32-29f5-487c-81a6-9750f4d17d20.png">
<img width="231" alt="image" src="https://user-images.githubusercontent.com/7360266/138223650-5ec9a6d0-cfea-4159-8366-8dfa00b3466e.png">
<img width="182" alt="image" src="https://user-images.githubusercontent.com/7360266/138223659-b8429502-d24f-4397-bb89-28ba7d27a2f8.png">

- UI Elements have Rect Transforms instead of Transforms
- (Transforms were not complicated enough)
- They have several new functions:
  - Anchors
  - Width & Height
  - Movable Pivot
- To see and edit them, use the Rect Tool

---

## 5. Rect transform: anchors

<img width="390" alt="image" src="https://user-images.githubusercontent.com/7360266/138223749-77d8b74c-2838-4db8-98c2-c6c52f41035b.png">

- They are very important for good UI Layouting
- You have several options to make you Uis for example:
  - Stick to the top left corner of the screen
  - Stick to the bottom center of the screen
  - Expand full-screen
  - Expand the left side of the screen

---

## 6. Stick to the top-left of the screen

<img width="346" alt="image" src="https://user-images.githubusercontent.com/7360266/138223786-6c376292-21b8-43d2-b5c6-3884c62ba208.png">

<img width="313" alt="image" src="https://user-images.githubusercontent.com/7360266/138223802-60166f40-f353-4639-9b94-2987a11da841.png">

---

## 7. Stick to the bottom-center of the screen

<img width="344" alt="image" src="https://user-images.githubusercontent.com/7360266/138223825-1191e74e-9982-4a2c-a74d-f5cfcbb6ea39.png">

<img width="349" alt="image" src="https://user-images.githubusercontent.com/7360266/138223833-47e1a3e7-28dd-4a41-b9b6-ef7bbd2f0907.png">

---

## 8. Expand full-screen

<img width="354" alt="image" src="https://user-images.githubusercontent.com/7360266/138223866-75bef4a6-0135-40ea-bfe4-15523668152f.png">

<img width="351" alt="image" src="https://user-images.githubusercontent.com/7360266/138223882-5adb06f3-e793-4218-9f81-19c2e7782dec.png">

---

## 9. Expand the left side of the screen

<img width="346" alt="image" src="https://user-images.githubusercontent.com/7360266/138223915-c737b084-213d-4e99-bf58-e22d5ac46914.png">

<img width="351" alt="image" src="https://user-images.githubusercontent.com/7360266/138223923-d2d0ede4-fff3-4ac8-9470-ce4a1509b0f7.png">

---

## 10. Rect transform: width and height

<img width="158" alt="image" src="https://user-images.githubusercontent.com/7360266/138224000-6dd52622-9894-48fe-bb39-2a6fb9f25939.png">
<img width="78" alt="image" src="https://user-images.githubusercontent.com/7360266/138224009-0b20b23b-187f-4a1e-ad90-addd38bd7ea3.png">
<img width="159" alt="image" src="https://user-images.githubusercontent.com/7360266/138224020-4cb921b5-43c5-46da-9ebe-4fc671aa6c1c.png">
<img width="144" alt="image" src="https://user-images.githubusercontent.com/7360266/138224030-41b1479c-a71b-4f4c-9d89-1c1d75cb8607.png">
<img width="162" alt="image" src="https://user-images.githubusercontent.com/7360266/138224043-32748b7f-4cb6-45c5-9813-aea2b4450c8c.png">
![image](https://user-images.githubusercontent.com/7360266/138224055-e742ebb5-d963-49dd-966a-48376d086bf6.png)

- Controls the width and height of an object that is not being scaled
- Or the offset of an object that is being scaled

---

## 11. Rect transform: pivot
- Very important in combination with Anchoring:
<img width="349" alt="image" src="https://user-images.githubusercontent.com/7360266/138224099-340b714b-e484-4533-8520-5114903f300c.png">
<img width="266" alt="image" src="https://user-images.githubusercontent.com/7360266/138224109-2ee6a6fb-655f-4d13-bb09-ffbd83e245c6.png">
<img width="180" alt="image" src="https://user-images.githubusercontent.com/7360266/138224118-844b2b09-9a2f-4327-aca4-e5aa9ef6d76e.png">
<img width="282" alt="image" src="https://user-images.githubusercontent.com/7360266/138224126-524101ea-533d-4f5c-85cc-33e6977d655a.png">
<img width="219" alt="image" src="https://user-images.githubusercontent.com/7360266/138224141-6af4472b-dfa7-49c4-993a-6aa7e9988235.png">
<img width="270" alt="image" src="https://user-images.githubusercontent.com/7360266/138224166-14effdcf-56f7-4825-9250-6fc93cb38a40.png">
<img width="217" alt="image" src="https://user-images.githubusercontent.com/7360266/138224177-78756710-f851-4073-bbc5-045a4f84de10.png">

---

## 12. Rect transform: why?

![image](https://user-images.githubusercontent.com/7360266/138224245-c8a5ce14-c3e2-4918-bc8f-7ca2fc92b98f.png)

![image](https://user-images.githubusercontent.com/7360266/138224254-1ca9d669-c4e3-4dd2-b6bd-382b99a2d243.png)

- It‘s very important for scalable Uis. That work on different resoutions
- Phones, Tablets, Monitors, …

---

## 13. Rect transform: how?

![image](https://user-images.githubusercontent.com/7360266/138224378-462ee2f3-6c76-4a2c-ac8a-e73aae752d9f.png)

- Try to anchor Uis into corners
  - So it works for different aspect ratios
- Scale the canvas with DPI
  - So they won‘t get too small on high resolution
- Make sure that they don‘t overlap
- If necessary, create custom Uis for different platforms
- Work with Anchors and Width/Height only. Not with scale on Graphics!

---

## 14. Text

- Can be used to put text on the canvas
- Has many options for fonts, etc.
- You can change the text in your code:

![image](https://user-images.githubusercontent.com/7360266/138224430-f1a9bac8-4fb2-40bc-af34-23dc465451b5.png)

![image](https://user-images.githubusercontent.com/7360266/138224445-6acbd5d9-98df-4298-91ce-42c4fb9b6397.png)

---

## 15. Button

<img width="396" alt="image" src="https://user-images.githubusercontent.com/7360266/138224522-2a66fe90-ea25-489c-8a94-1b3fee407373.png">

<img width="354" alt="image" src="https://user-images.githubusercontent.com/7360266/138224537-a03816ed-e9af-4ec0-9332-71704f47d668.png">

- Is it exactly, what it sounds like
- You can call any public functions (with restrictions to parameters) using On Click()

---

## 16. Unity Events

![image](https://user-images.githubusercontent.com/7360266/138224656-508ed5b3-f930-4908-a4d1-de35c5d6aca4.png)

![image](https://user-images.githubusercontent.com/7360266/138224735-59fdc413-42d6-4081-9b55-ad94be7713fb.png)

- You can add Callbacks to Unity Events, for example Button‘s On Click() Event.
- Here, I will click the + Button first
- Then Drop the Hud-GameObject into the Target Field
- Then change No Function to Hud > RefreshHud()
- You can also add a Callback using code, but that‘s more complicated.

---

## 17. Not covered yet

![Uploading image.png…]()

- The ones on the right, and many more ☺
- We will cover them one by one
- Check out the Unity Documentation for more infos

---
