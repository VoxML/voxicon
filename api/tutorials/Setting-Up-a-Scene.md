---
layout: page
title:  "VoxSim Documentation - Tutorials - Setting Up a Scene"
---
# Setting Up a Scene
There are four components required to give a Unity scene VoxSim functionality: *VoxWorld*, *CommunicationsBridge*, *BehaviorController*, and *IOController*.  All of these are prefab objects and can be found in **VoxSimPlatform/Prefabs/Modules**.  These can be instantiated in a scene by dragging the prefab into the scene or Unity hierarchy.\
<img src="../../images/Setting-Up-a-Scene1.png" width="300">

Adding these components gives access to:
* [VoxML semantics](../../VoxSimPlatform/Vox/Voxeme) and [interpretation](../../VoxSimPlatform/Core/Predicates)
* 3rd-party endpoint connections via the [communications bridge](../../VoxSimPlatform/Network/CommunicationsBridge)
* Text input via the [input contoller](../../VoxSimPlatform/Agent/InputController)
* [Event management](../../VoxSimPlatform/Core/EventManager), basic [spatial relation tracking](../../VoxSimPlatform/SpatialReasoning/RelationTracker), and [physics managment](../../VoxSimPlatform/CogPhysics/PhysicsPrimitives)
* [Modal window](../Modal-Windows) and [UI button](../UI-Buttons) management

When the VoxWorld components are all added, a new scene will play with a text input field and a couple of UI buttons:\
<img src="../../images/Setting-Up-a-Scene2.png" width="800">

## Creating Semantic Objects
In order to make objects interactable, interpretable, and accessible to the VoxSim [event manager](../../VoxSimPlatform/Core/EventManager), they must all be [voxemes](../../VoxSimPlatform/Vox/Voxeme).

To make an object into a Voxeme, select it in the hierarchy and add a *Voxeme* component to it.
<img src="../../images/Setting-Up-a-Scene3.png" width="300">

Choosing *VoxSim >> Voxify Object* will add Voxeme components automatically to all selected object.

Objects should have only one Voxeme component.  A Voxeme component on an object signals that the object should be initialized as a semantic object, which includes calculating its mass at runtime and making it available as an argument to event programs, and links it to VoxML semantics.

The *Predicate* field of the Voxeme component must be set to the object's semantic type.  At runtime, this will link the Voxeme component on the object to the VoxML encoding of equivalent object.  VoxML encodings for objects should be placed in the folder **VoxML/voxml/objects** parallel to the project **Assets** folder.  The *Lex*>>*Pred* field of the VoxML encoding should match the *Predicate* of the Voxeme component.

For example, a minimal VoxML encoding for "floor" shown below would be saved in **VoxML/voxml/objects/floor.xml**:\
<img src="../../images/Setting-Up-a-Scene4.png" width="800">

All objects of a single semantic type link to the same VoxML.  For example, if there are multiple "block" objects in the scene that share the same semantics, they would all need to have Voxeme components on them that all have *Predicate* set to "block" and all would link to the above VoxML encoding.

To add attributes, like colors, to voxemes, add an [AttributeSet](../../VoxSimPlatform/Vox/AttributeSet) component.  Set *Size* to the number of nominal attributes the object should be able to be referenced by, then fill them into the fields below.  For example, a bowl object with the following attribute set would be able to be referred to as both "white block" and "gray block".

<img src="../../images/Setting-Up-a-Scene5.png" width="300">

## Setting Up Sample Scene – Step by Step Instructions
1)	Create new voxml folder in **&lt;project root&gt;/VoxML** directory
  
2)	Create folders for objects, attributes, programs, relations, and functions in **&lt;project root&gt;/VoxML/voxml** directory.

<img src="../../images/Setting-Up-a-Scene6.png" width="300">

3)	Add desired VoxML encodings into newly created **&lt;project root&gt;/VoxML/voxml folders**. Below are the files that were moved for the sample scene:
* Attributes: black.xml, white.xml
* Objects: block.xml, floor.xml
* Programs: grasp.xml, hold.xml, move.xml, put.xml, ungrasp.xml
* Relations: all relations files
  
4)	Creating an immobile surface (e.g., ground).  Refer to the floor in VoxWorld-QS for an example.
* Add the object geometry to the scene and position it. 
* Select the surface object in the hierarchy. In the Inspector, change Tag to *UnPhysic* using dropdown menu.

<img src="../../images/Setting-Up-a-Scene7.png" width="300">
  
* With surface object still selected, from *VoxSim* menu at the top of the screen, select *Voxify Object*.
  
<img src="../../images/Setting-Up-a-Scene8.png" width="300">
  
* From inspector, in Voxeme (Script), change *Predicate* to the semantic type of this object.
* (Optional) Add color to the surface by adding a material.
  
5)	Create interactable objects. Refer to the blocks in VoxWorld-QS as an example.
* Add the object geometry to the scene and position it. 
* Ensure that position of the interactable object is directly on the immobile surface. Failing to do so can create a position discrepancy at run time between the interactable object and the voxeme object referring to the interactable object. If a ground object is created without changing dimensions or position, then the Y position of the newly created 1x1x1 cube should be 0.5.
  
<img src="../../images/Setting-Up-a-Scene9.png" width="300">
  
* Add Rigidbody component to the interactable object by selecting *Add Component* in the inspector window.
* With interactable object selected, from *VoxSim* menu at the top of the screen, select *Voxify* Object.
* From inspector, in Voxeme (Script), change *Predicate* to the semantic type of this object. 
* (Optional) Add color to the surface by adding a material.
* From inspector, click *Add Component*. Add the Attribute Set (Script). Set *Size* hold number of attributes. Add desired elements. For example, set *Size* to 1 and *Element* 0 to black to identify a black cube.
* Repeat the above steps to create additional interactable objects.
  
6)	Add agent. In the sample scene, we make the agent equivalent with the Main Camera.  The agent has no separate 3rd-person embodiment or visual presence.
* In the Hierarchy, create an Agent component and place the Main Camera below the Agent to make the Main Camera a child of the Agent. 
  
<img src="../../images/Setting-Up-a-Scene10.png" width="300">
  
* With Agent selected in Hierarchy, from the Inspector, change the tag to Agent.
  
<img src="../../images/Setting-Up-a-Scene11.png" width="300">
  
 * With Main Camera Selected, in Inspector click *Add Component* to add Referent Store (Script)
  
7)	Click play button. A dialogue box should be displayed where the user can type commands. Commands must be worded very specifically, based on the objects, attributes, programs, and relations the sample scene knows. For example, the user must type “Put black block on white block” instead of “Place black cube on white cube”, because the words “place” and “cube” are unknown.
  
<img src="../../images/Setting-Up-a-Scene12.png" width="800">
  
<img src="../../images/Setting-Up-a-Scene13.png" width="800">


