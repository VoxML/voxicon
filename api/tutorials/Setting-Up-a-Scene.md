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
