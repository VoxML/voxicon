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

### Clients/IOClients
Every connection is handled using two components: the *client* and the *IOClient*.  The *client* is a class whose name matches the *type* field of a socket configuration (including namespace).  The *IOClient* handles the read operations that the client inherits from its parent class and maintains the reference to the **CommunicationsBridge** so that it can try to reconnect the client if the connect breaks for some reason.

Clients must inherit from one of two types: **[SocketConnection](SocketConnection)**--a standard TCP socket--or **[RESTClient](RESTClient)**.  In the constructor for the client, `client_type` must be set to the type of the associated IOClient:
```
public class NLURESTClient : RESTClient {
  public NLURESTClient() {
    clientType = typeof(IOClients.NLUIOClient);
  }
}
```

The IOClient must contain a reference to the associated client, and a reference to the communications bridge.  The communications bridge and client are assigned in the `Start()` method (the client is found by its label but can also be found by type):
```
public class NLUIOClient : MonoBehaviour {
  /// <summary>
  /// The associated REST client
  /// </summary>
  RESTClients.NLURESTClient _nluRestClient;
  public RESTClients.NLURESTClient nluRestClient {
    get { return _nluRestClient; }
    set { _nluRestClient = value; }
  }

  CommunicationsBridge commBridge;
  
  // Use this for initialization
  void Start() {
    commBridge = GameObject.Find("CommunicationsBridge").GetComponent<CommunicationsBridge>();
    _nluRestClient = (RESTClients.NLURESTClient)commBridge.FindRESTClientByLabel("NLTK");
  }

  // Update is called once per frame
  void Update() {
  }
}
```

## Working Examples

**VoxSimPlatform/Assets/Scripts/Examples** contains a working example of a custom connection using the REST client design pattern, to connect to an external [parser](../../NLU/INLParser).
