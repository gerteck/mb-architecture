---
  title: "Variable Processor, Renderer"
  pageNav: 2
  pageNavTitle: "Variable Processor, Renderer"
---

# VariableProcessor

The variableProcessor class handles site variables, (extraction, storage and rendering). It maintains a mapping of variables for each (sub)Site. It calls variableRenderer to replace placeholders with actual values.

<br>

<puml src="{{ baseUrl }}/diagrams/site/variableProcessor.puml" width=900 />

<br><br>

# VariableRenderer

The variableRenderer performs actual rendering of variables.

<br>

<puml src="{{ baseUrl }}/diagrams/site/variableRenderer.puml" width=900 />

<br><br>