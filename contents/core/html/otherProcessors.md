---
  title: "Other Processors"
  pageNav: 2
  pageNavTitle: "Other Processors"
---

<br>

<page-nav-print> Table of Contents </page-nav-print>

<br>

# CodeBlockProcessor

The `CodeBlockProcessor` utility provides three main methods, `traverseLinePart` which traverses a line part and applies highlighting, `highlightCodeBlock` which applies pending highlighting to code block, and `setCodeLineNumbers` which appends the `line-numbers` class depending on global settings.

<br>

<puml src="{{ baseUrl }}/diagrams/html/codeProcessor.puml" width=900 />

<br><br>

# CustomListIconProcessor

The main function exported by this utility is the `processUlNode`, called to process a list `ul` node and its child elements (`li` nodes).

<br>

<puml src="{{ baseUrl }}/diagrams/html/customListIconProcessor.puml" width=900 />

<br><br>

# Context

The `Context` class is used to manage the state and control flow when processing files that may have references to each other. It tracks the current file being processed, the call stack of files, and variables that are relevant for the processing. The class is particularly useful for situations where the program might encounter cyclic dependencies or references, which is where the CyclicReferenceError class comes into play.


<br>

<puml src="{{ baseUrl }}/diagrams/html/context.puml" width=500 />

<br><br>

* cwf: current working file
* callStack: array of file paths processed so far

Responsible for managing state and flow of file processing while preventing infinite recursion caused by cyclic references.

# FootnoteProcessor

The FootnoteProcessor class is designed to handle the processing of footnotes within a MarkBind project. It stores footnotes, processes them, and eventually combines them into a structured block of HTML content. This class helps in rendering footnotes, including handling `<include>` elements that may also have footnotes.


<br>

<puml src="{{ baseUrl }}/diagrams/html/footnoteProcessor.puml" width=800 />

<br><br>


# HeaderProcessor

The HeaderProcessor utility provides two main functions, `setHeadingId` which increments the counter for same heading text, and sets id of the heading node.

It also provides `assignPanelId`, which assigns an id to the root element of a panel using the heading specified in the header slot if any.

<br>

<puml src="{{ baseUrl }}/diagrams/html/headerProcessor.puml" width=800 />

<br><br>

# IncludePanelProcessor

IncludePanelProcessor utility provides 3 main functions:
* `processPanelSrc`, `processInclude`, `processPopoverSrc`

The mechanism of all 3 functions are quite similar.

They do file/source validation, rendering, handling hashes, cyclic reference checks, updating node content, logging.

Differences being that the focus are different on either `panel`, `include` or `popover` elements, with special handling for element-specific logic.

<br>

<puml src="{{ baseUrl }}/diagrams/html/processInclude.puml" width=800 />

<br><br>

# LinkProcessor

This LinkProcessor utility has a few main functions, including `isIntraLink`, `convertRelativeLinks`, `validateIntraLink`, `collectSource` etc. It provides utility on processing links, validating and resolving sources.


<br>

<puml src="{{ baseUrl }}/diagrams/html/linkProcessor.puml" width=800 />

<br><br>

# MarkdownProcessor

MarkdownProcessor is a class to use the methods from `markdown-it`. `markdown-it` is an external library that does the Markdown parsing and rendering, and some custom plugins are used and customized together with `markdown-it`.


<br>

<puml src="{{ baseUrl }}/diagrams/html/markdownProcessor.puml" width=500 />

<br><br>


# MarkdownAttributeRenderer

Class that is responsible for rendering markdown-in-attributes. Process markdown attributes of element, and insert `<slot>` child as needed. etc.

<br>

<puml src="{{ baseUrl }}/diagrams/html/markdownAttributeRenderer.puml" width=800 />

<br><br>

# scriptAndStyleTageProcessor

This exports the utility function to remove every script and style node that may be written by the user, hoist to after `<body>` at a later stage to prevent Vue compilation of the tags.

# siteAndPageNavProcessor

This exports the classes `PageNavProcessor`, which replaces and stores a uuid identifier to the only page-nav element if there is one.

It also exports the function `renderSiteNav`.

# SiteLinkManager

The class is responsible for managing and validating intra-site links within the system.

<br>

<puml src="{{ baseUrl }}/diagrams/html/siteLinkManager.puml" width=800 />

<br><br>

### Dependencies:

- **lodash/has**: Used to check if an object has a specific property.
- **./linkProcessor**: Contains utility functions for processing links, such as validating intra-links and extracting resource paths.
- **./NodeProcessor**: Defines the `NodeProcessorConfig` type, which is used to configure the behavior of the `SiteLinkManager`.
- **../utils/node**: Defines the `MbNode` type, which represents a node in the document tree.
- **./headerProcessor**: Provides the `setHeadingId` function, which is used to set IDs for heading tags.


### Class Properties:

- **config**: An instance of `NodeProcessorConfig` that holds configuration settings.
- **intralinkCollection**: A `Map` that stores sets of resource paths keyed by the current working file (CWF). This is used to collect intra-links that need to be validated.
- **filePathToHashesMap**: A `Map` that stores sets of hashes (IDs) keyed by file paths. This is used to maintain a mapping of reachable sections within files.


### Constructor:

- Initializes the `config`, `intralinkCollection`, and `filePathToHashesMap` properties.


### Methods:

- **_addToCollection(resourcePath: string, cwf: string)**: Adds a resource path and CWF to the `intralinkCollection`, ensuring each pair is unique.
- **validateAllIntralinks()**: Validates all intra-links collected in the `intralinkCollection` if intra-site link validation is enabled.
- **collectIntraLinkToValidate(node: MbNode, cwf: string)**: Collects an intra-link to be validated later if the node should be validated and intra-link validation is not disabled.
- **maintainFilePathToHashesMap(node: MbNode, cwf: string)**: Adds sections that could be reached by intra-links with hashes to the `filePathToHashesMap`.
- **maintainHashesForInclude(node: MbNode, cwf: string)**: Recursively adds reachable sections of the included node to the `filePathToHashesMap` for validation.




