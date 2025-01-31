---
  title: "NodeProcessor"
  pageNav: 2
  pageNavTitle: "NodeProcessor"
---

<br>

# NodeProcessor Class 

<br>

<puml src="{{ baseUrl }}/diagrams/html/nodeProcessor.puml" width=1000 />

<br><br>

The `NodeProcessor` class is responsible for processing and transforming HTML nodes in MarkBind. It takes in raw HTML or Markdown content, processes various MarkBind-specific elements, and returns the transformed HTML output. It plays a central role in rendering pages by handling:
* Frontmatter processing
* Markdown rendering
* Syntax transformations (e.g., Vue slots)
* Processing custom MarkBind components (e.g., include, panel, popover)
* Handling code highlighting, footnotes, and intrasite links
* Post-processing for final refinements

<br>




<puml src="{{ baseUrl }}/diagrams/html/nodeProcessorSequence.puml" width=700 />

<br><br>

The main method for NodeProcessor is the `process` function, which parses the Markdown or HTML content and processes each node in the DOM.

Recall that:

> Content Processing Flow: 
> Nunjucks -> Markdown -> HTML


Nunjucks largely processed in variableProcessor.

Markdown and HTML largely processed in nodeProcessor.