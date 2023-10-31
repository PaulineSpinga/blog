---
title: "HCI - Unity game"
date: 2023-10-01T13:45:06+06:00
image: images/blog/HCI/blog-post-unity.png
feature_image: images/blog/HCI/unity.png
author: Pauline SPINGA
subject : "HCI"
---
The aim of the project is to compute a **roll a ball** game. The idea is to roll a ball within a box in order to collect items. To win the game, the player must collect all the items. 

To complete the project, I followed the 
[learn.unity tutorial](https://learn.unity.com/project/roll-a-ball).

Some of the issues I encountered : 

- Syntax error in my C# script : **inputeValue** instead of **InputValue**
- There was a rotation when I was moving the ball with my keyboard. To fix it : freeze rotation in x, y, z for the oject **ball** 

Here is a demo of the game : 

![demo](https://i.imgur.com/lgouSlH.mp4)


Here are the step the compute this game : 

### Setting up the game 
* Create 3D plane and rename it **Ground**
* Create 3D Sphere and rename it **Player**
* You can scale the objects to give them the desired size
* You can add colors and textures to your objects by creating **Material** in a **Materials** folder. You can choose colors, textures, brightness, etc. Then, apply your Material to your object.

### Moving the player
* Add a rigidbody to your player 
* Install the **Input System Package**
* Add the **Input Component** to the player and add an Input Action Asset called **InputActions**
* Create a C# script called **PlayerController** in a **Scripts** folder. Add the following code : 

```C#
using System.Collections;
using System.Collections.Generic;
using Unity.VisualScripting;
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerController : MonoBehaviour
{
 // Rigidbody of the player.
 private Rigidbody rb; 

 // Movement along X and Y axes.
 private float movementX;
 private float movementY;

 // Speed at which the player moves.
 public float speed = 0; 

 // Start is called before the first frame update.
 void Start()
    {
 // Get and store the Rigidbody component attached to the player.
        rb = GetComponent<Rigidbody>();
    }
 
 // This function is called when a move input is detected.
 void OnMove(InputValue movementValue)
    {
 // Convert the input value into a Vector2 for movement.
        Vector2 movementVector = movementValue.Get<Vector2>();

 // Store the X and Y components of the movement.
        movementX = movementVector.x; 
        movementY = movementVector.y; 
    }

 // FixedUpdate is called once per fixed frame-rate frame.
 private void FixedUpdate() 
    {
 // Create a 3D movement vector using the X and Y inputs.
        Vector3 movement = new Vector3 (movementX, 0.0f, movementY);

 // Apply force to the Rigidbody to move the player.
        rb.AddForce(movement * speed); 
    }
}
```


### Moving the camera

* Set the Camera position and undo the parent-child relationship by dragging the Main Camera above the Player GameObject
* Write a CameraController script : 

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraController : MonoBehaviour
{
 // Reference to the player GameObject.
 public GameObject player;

 // The distance between the camera and the player.
 private Vector3 offset;

 // Start is called before the first frame update.
 void Start()
    {
 // Calculate the initial offset between the camera's position and the player's position.
        offset = transform.position - player.transform.position; 
    }

 // LateUpdate is called once per frame after all Update functions have been completed.
 void LateUpdate()
    {
 // Maintain the same offset between the camera and player throughout the game.
        transform.position = player.transform.position + offset;  
    }
}
```
* Reference the Player GameObject : Drag the Player GameObject from the Hierarchy window into the Player slot in the CameraController component. 

### Setting up the play Area
* Add 4 cubes and scale them in order to build walls
* Add material
* For each wall, add a box collider and make sure the **Is Trigger** box is unchecked : we want walls to stop your Player ball. Scale and center each collider for it to wrap each wall. 
### Creating collectibles
* Add 3D objects to represent your collectibles. You can choose any shapes you want : cube, sphere, or create your own (diamond, stars, etc).
* Add as many collectibles as you wish
* You can create an Empty Objects names **Collectibles** as parent of all collectibles 
* Optional : create a **Rotator** script to make your collectibles rotate on their own. Add the following code : 

```C#
using UnityEngine;

public class Rotator : MonoBehaviour
{

 // Update is called once per frame
 void Update()
    {
 // Rotate the object on X, Y, and Z axes by specified amounts, adjusted for frame rate.
        transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
    }
 
}
```
### Detect collision with collectibles

This idea is to disable each collectible you touched. This is possible because each collectible has a Rigidbody and an **IsTrigger**  collider that will detect the collision without stopping the player.
Thus, when a collectible is **On trigger**, the **OntriggerEnter** function is called. 

### Displaying Score and Text 

* Each time you pick a collectible, a counter has to be incremented
* In your scene, add a text object, named "Count text" and edit the text. The function SetCountText() will look at the counter value in order to update the displayed value in the TextMeshProUGUI element.
* Don't forget to assign every Public variable in the Inspector window of the element you apply the script on 
* When all the collectibles are collected, display a **Win text**. By default it will be desactivated, but when you reach the maximum number of collectible, the text area is activated. 

Here is the final script of the PlayerCOntroller, including the detection collision with collectibles : 

```C#
using System.Collections;
using System.Collections.Generic;
using Unity.VisualScripting;
using UnityEngine;
using UnityEngine.InputSystem;
using TMPro;

public class PlayerController : MonoBehaviour
{
 // Rigidbody of the player.
 private Rigidbody rb; 

 // Variable to keep track of collected "PickUp" objects.
 private int count;

 // Movement along X and Y axes.
 private float movementX;
 private float movementY;

 // Speed at which the player moves.
 public float speed = 0;

 // UI text component to display count of "PickUp" objects collected.
 public TextMeshProUGUI countText;

 // UI object to display winning text.
 public GameObject winTextObject;

 // Start is called before the first frame update.
 void Start()
    {
 // Get and store the Rigidbody component attached to the player.
        rb = GetComponent<Rigidbody>();

 // Initialize count to zero.
        count = 0;

 // Update the count display.
        SetCountText();

 // Initially set the win text to be inactive.
        winTextObject.SetActive(false);
    }
 
 // This function is called when a move input is detected.
 void OnMove(InputValue movementValue)
    {
 // Convert the input value into a Vector2 for movement.
        Vector2 movementVector = movementValue.Get<Vector2>();

 // Store the X and Y components of the movement.
        movementX = movementVector.x; 
        movementY = movementVector.y; 
    }

 // FixedUpdate is called once per fixed frame-rate frame.
 private void FixedUpdate() 
    {
 // Create a 3D movement vector using the X and Y inputs.
        Vector3 movement = new Vector3 (movementX, 0.0f, movementY);

 // Apply force to the Rigidbody to move the player.
        rb.AddForce(movement * speed); 
    }

 
 void OnTriggerEnter(Collider other) 
    {
 // Check if the object the player collided with has the "PickUp" tag.
 if (other.gameObject.CompareTag("PickUp")) 
        {
 // Deactivate the collided object (making it disappear).
            other.gameObject.SetActive(false);

 // Increment the count of "PickUp" objects collected.
            count = count + 1;

 // Update the count display.
            SetCountText();
        }
    }

 // Function to update the displayed count of "PickUp" objects collected.
 void SetCountText() 
    {
 // Update the count text with the current count.
        countText.text = "Count: " + count.ToString();

 // Check if the count has reached or exceeded the win condition.
 if (count >= 12)
        {
 // Display the win text.
            winTextObject.SetActive(true);
        }
    }
}
```
### Build the game
* Open **Build Settings** : File > Build Settings
* In Scenes in Build, select Add Open Scenes to add your game to the build. You can also drag scenes from the Project window to this list. 
* Save changes
* Select Build, choose a build location (usually a **build folder** you create at the root of the project)
* You can build and test your build where you saved it.

Well done, you've created your very first unity game !

