---
layout: page
title:  "VoxSim API - CommunicationsBridge"
---
# CommunicationsBridge
Namespace: VoxSimPlatform.Network\
Inherits: MonoBehaviour\
File: CommunicationsBridge.cs

## Setting up connections
Connections can be set up through the Launcher menu in the *VoxSimMenu.unity* scene (**VoxSimPlatform/Scenes**).  One the left side of the Launcher menu you'll see the *Connections* pane.  Click the '+' to add a new connection.
<img src="../../../images/CommunicationsBridge1.png" width="600">

Every connection is defined by four fields: a *label* (string), an *address* (string, must be a valid IP+port), a *type* (string, explained in more detail below), and an *active status* (bool).
<img src="../../../images/CommunicationsBridge2.png" width="600">
