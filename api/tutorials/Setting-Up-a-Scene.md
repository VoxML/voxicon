---
layout: page
title:  "VoxSim Documentation - Tutorials - Setting Up a Scene"
---
# Setting Up a Scene
There are four components required to give a Unity scene VoxSim functionality: *VoxWorld*, *CommuncationsBridge*, *BehaviorController*, and *IOController*.  All of these are prefab objects and can be found in **VoxSimPlatform/Prefabs/Modules**.  These can be instantiated in a scene by dragging the prefab into the scene or Unity hierarchy.\
<img src="../../images/Setting-Up-a-Scene1.png" width="300">

## Adding Semantic Objects
In order to make objects interactable, interpretable, and accessible to the VoxSim [event manager](../../VoxSimPlatform/Core/EventManager), they must all be [voxemes](../../VoxSimPlatform/Vox/Voxeme).
