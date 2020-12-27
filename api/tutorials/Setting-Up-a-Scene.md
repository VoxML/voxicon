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
<img src="../../images/Setting-Up-a-Scene2.png" width="600">

## Creating Semantic Objects
In order to make objects interactable, interpretable, and accessible to the VoxSim [event manager](../../VoxSimPlatform/Core/EventManager), they must all be [voxemes](../../VoxSimPlatform/Vox/Voxeme).
