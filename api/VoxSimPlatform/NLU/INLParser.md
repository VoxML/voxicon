---
layout: page
title:  "VoxSim API - INLParser"
---
# INLParser
Namespace: VoxSimPlatform.NLU\
Inherits: none\
File: INLParser.cs

## Methods
<table>
  <tr>
    <td><strong>Private</strong></td><td></td>
  </tr>
  <tr>
    <td>NLParse</td><td>Parses the input</td>
  </tr>
  <tr>
    <td>ConcludeNLParse</td><td>Retrieves parsed output from parser (for use with asynchronous parser clients)</td>
  </tr>
  <tr>
    <td>InitParserService</td><td>Initializes the parser with its expected syntax</td>
  </tr>
</table>

## Instantiating a parser

The `parser` is a member variable of the **CommunicationsBridge** class.

```
public class CommunicationsBridge : MonoBehaviour {
  private INLParser _parser;
  public INLParser parser {
    get { return _parser; }
    set { _parser = value; }
  }
}
```

Therefore instantiating a parser for use is as simple as assigning an instance to the `parser` variable, as in done in the `InitDefaultParser()` method in **CommunicationsBridge**:

```
public void InitDefaultParser() {
  _parser = new SimpleParser();
}
```

Parsers can be implemented directly in C# or written externally in any language and connected via socket or REST connection.  In the case of an external parser, in the method where you instantiate the parser, you also need to run `InitParserService()` and pass in the client handling the connection, and (optionally) the type of the [*expected syntax*](../../NLU/IGenericSyntax.cs) of the parser.

```
public class NLUModule : MonoBehaviour
{
  bool parserInited = false;

  public CommunicationsBridge communicationsBridge;
  public string parsed;
  
  // Use this for initialization
  void Start()
  {

  }

  // Update is called once per frame
  void Update()
  {
    // set the parser here (and not in Start()) because we need to be sure that CommunicationsBridge
    //  has fully started before trying access elements of it
    if (!parserInited)
    {
      VoxSimPlatform.Examples.RESTClients.NLURESTClient nluClient =
        (VoxSimPlatform.Examples.RESTClients.NLURESTClient)communicationsBridge.FindRESTClientByLabel("NLTK");

      // if client found
      if ((nluClient != null) && (nluClient.isConnected))
      {
        // if the client is connected
        //  create the parser with its expected syntax
        //  and set an event handler that will call
        //  communicationsBridge.GrabParse()
        communicationsBridge.parser = new VoxSimPlatform.Examples.NLParsers.PythonJSONParser();
        communicationsBridge.parser.InitParserService(nluClient,
          typeof(VoxSimPlatform.Examples.Syntaxes.NLTKSyntax));
        nluClient.PostOkay += LookForNewParse;
      }

      parserInited = true;
    }
  }

  public void LookForNewParse(object sender, EventArgs e)
  {
    parsed = communicationsBridge.GrabParse();
  }
}
```

## Parser Implementations

Your custom parser class must contain an implementation of `NLParse()` that either does the parse itself (if implemented within C# entirely), or hands off the information to the external parsing client to be parsed, as below:

```
public string NLParse(string rawSent) {
  if (nluRESTClient != null) {
    RESTDataContainer result = new RESTDataContainer(nluRESTClient.owner, nluRESTClient.Post(route, rawSent));
  }
  return "WAIT";  // return signal that we're waiting for a result
}
```

External parsers that received data from VoxSim and use the REST architecture need to also have an implementation of `ConcludeNLParse()` that retrieves the parse from the external parser, in the expected syntax, and then ultimately turns that into the [predicate-argument format](../../Core/EventManager.cs) that VoxSim requires.

```
public string ConcludeNLParse() {
  string returnVal = "";
  String toPrint = "";
  
  if (nluRESTClient != null) {
    // Grab the result
    toPrint = nluRESTClient.webRequest.downloadHandler.text;
  }

  if (toPrint == "empty" || toPrint == null || toPrint == "") {
    return "";
  }
  
  returnVal = JsonToFormat(toPrint);
  return returnVal;
}
```

## Working Examples

**VoxSimPlatform/Assets/Scripts/NLU/SimpleParser** is the default parser VoxSim will run with if no other parser is instantiated.  It is a very simple rule-based parser with a fixed vocabulary, and is executed directly within C# (and so does not have to be initialized as a service or with an expected syntax.

**VoxSimPlatform/Assets/Scripts/Examples/NLParsers** contains a working example of an interface to custom parser that uses the Python NLTK Library, and connects using the [REST client design pattern](../../Network/RestClient.cs) (Python client found in **VoxSimPlatform/Assets/Externals/python/nltk_parser_example**).

