---
layout: page
title:  "VoxSim API - CommunicationsBridge"
---
# CommunicationsBridge
Namespace: VoxSimPlatform.Network\
Inherits: MonoBehaviour\
File: CommunicationsBridge.cs

## Setting up connections
Connections can be set up through the Launcher menu in the *VoxSimMenu.unity* scene (**VoxSimPlatform/Scenes**).  One the left side of the Launcher menu you'll see the *Connections* pane.  Click the '+' to add a new connection.\
<img src="../../../images/CommunicationsBridge1.png" width="300">

Every connection is defined by four fields (left to right, top to bottom): a *label* (string), an *address* (string, must be a valid IP+port), an *active status* (bool - check to connect to this socket when the CommunicationsBridge component starts up, uncheck to skip it), and a *type* (string, explained in more detail below).\
<img src="../../../images/CommunicationsBridge2.png" width="300">
