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

Objects should have only one Voxeme component.  A Voxeme component on an object signals that the object should be initialized as a semantic object, which includes calculaing its mass at runtime and make it available as an argument to event programs, and links it to VoxML semantics.

The *Predicate* field of the Voxeme component must be set to the object's semantic type.  At runtime, this will link the Voxeme component on the object to the VoxML encoding of equivalent object.  VoxML encodings for objects should be placed in the folder **VoxML/voxml/objects** parallel to the project **Assets** folder.  The *Lex* field of the VoxML encoding should match the *Predicate* of the Voxeme component.

For example, a minimal VoxML encoding for "bowl" shown below would be saved in **VoxML/voxml/objects/bowl.xml**:
```
<?xml version="1.0" encoding="us-ascii"?>
<VoxML xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <Entity Type="Object" />
  <Lex>
    <Pred>bowl</Pred>
    <Type>physobj*artifact</Type>
  </Lex>
  <Type>
    <Head>cylindroid[1]</Head>
    <Components>
      <Component Value="surface[1]" />
      <Component Value="interior[2]" />
    </Components>
    <Concavity>Concave[2]</Concavity>
    <RotatSym>Y</RotatSym>
    <ReflSym>XY,YZ</ReflSym>
  </Type>
  <Habitat>
    <Intrinsic />
    <Extrinsic />
  </Habitat>
  <Afford_Str>
    <Affordances />
  </Afford_Str>
  <Embodiment>
    <Scale>&lt;agent</Scale>
    <Movable>true</Movable>
  </Embodiment>
</VoxML>
```

All objects of a single semantic type link to the same VoxML.  For example, if there are multile "bowl" objects in the scene that share the same semantics, they would all need to have Voxeme components on them that all have *Predicate* set to "bowl" and all would link to the above VoxML encoding.
