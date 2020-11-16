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

Every connection is defined by four fields (left to right, top to bottom): an *name* (string), a *URL* (string, must be a valid IP+port), an *enabled status* (bool - check to connect to this socket when the CommunicationsBridge component starts up, uncheck to skip it), and a *type* (string, explained in more detail below).\
<img src="../../../images/CommunicationsBridge2.png" width="300">

"Save Socket Config" will save all socket configurations to a file called **socket_config.xml** in a folder called **local_config** parallel to the **Assets** folder.  For example:
```
<?xml version="1.0"?>
<VoxSimSocketConfig xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <SocketsList>
    <Socket>
      <Name>NLTK</Name>
      <Type>VoxSimPlatform.Examples.RESTClients.NLURESTClient</Type>
      <URL>127.0.0.1:5000</URL>
      <Enabled>true</Enabled>
    </Socket>
  </SocketsList>
</VoxSimSocketConfig>
```
Socket configurations can be edited in this XML file directly and then loaded into VoxSim by clicking "Load Socket Config."
