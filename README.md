# nodeGraph.js
Creates a D3 force graph from the current page's tags.
Designed to show the relationships between static pages on a site. (Also works with Wordpress, or any other Js-friendly platform)

Tags are read in two ways:

First, by reading the HTML div class of "tagForNodeGraph":
*nodeGraph.js links with the class "tagForNodeGraph".
*For each tag, a node is created.
*If the tag matches the expected pattern (connection_name), the connection is labelled from the text before the separator, and the node is labelled with the text after. Then the link is created by combining the hrefRootForNodes setting, and the node label.
*If the tag does not match the pattern, the tag becomes the node label, and the tag's link becomes the node's link.

Alternately, by setting tagClassInParentElement to true, nodeGraph.js looks for the class "tagForNodeGraph", and iterates through all links inside of it.
*Nodes are built as before


### Settings:
    "graphDivID": "nodeConnectionGraph", //the class name that will be targeted for adding the node graph. Example: <div class="nodeConnectionGraph"></div>
    "tagClass": "tagForNodeGraph", //The class where nodeGraph will search for tags to start building the nodes
    "tagClassInParentElement": false, //Are the tags children of tagClass?
    "separatorForNodeConnections": "_", //this allows for tag names such as "_connection_node" and "@connection@node"
    "separatorForSpacesInTags": ".", //If your html does not allow for spaces in tags, substitute for this separator
    "hrefRootForNodes": "", //When building the links on the nodes, we build like: $hrefRootForNodes + nodename. Note: Links can be explicitly defined in the tag
    "graphWidth": 920, //Width of the nodeGraph area
    "graphHeight": 500, //Height of the nodeGraph area


### D3
For understanding, nodeGraph.js is creating json from the current page, and D3 is using that json to create a force node graph.

So for tag with value “post”, output will be:
```
{
      startNode”:”(current page title)",
      “link”:””,
      “endNode”:”post”, (not split)) 
      “startNodeHref”:”(current page link)”,
      “endNodeHref”:”http://currentpage.com/tag/post” (use href of tag dom element)
}
```

But if tag value = “_post” then:
```
{
      startNode”:”(current page title)",
      “link”:””, (split by "_" and use first part)
      “endNode”:”post”, (split by "_" and use second part)
      “startNodeHref”:”(current page link)”,
      “endNodeHref”:”http://currentpage.com/post” (hrefRootForNodes+endNode)
}
```