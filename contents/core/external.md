---
  title: "External, ExternalManager Classes"
  pageNav: 2
  pageNavTitle: "External, ExternalManager Classes"
---

<br>

# External, ExternalManager Classes

The ExternalManager class manages and generates Externals. `Externals` are source files referenced in a `Page`, `Layout` or other `Externals` that result in a separate output file, which are loaded dynamically and on-demand in the browser. Instances managed by a single `ExternalManager` instance object.

<br>

<puml src="{{ baseUrl }}/diagrams/site/external.puml" width=900 />

<br><br>

## External.resolveDependency()

The resolveDependency method generates the content of the External instance. 

* The `User` calls the `resolveDependency` method on the external class. The `External` class initializes the `fileConfig` object with the provided configuration. Then, use `VariableProcessor` to render site variables in the external file content. Use `NodeProcessor` to process external file content. Use `PluginManager` to post-process content (plugin transformations). Use `fs` to output generated content to output file.

<br>

<puml src="{{ baseUrl }}/diagrams/site/external_resolveDependency.puml" width=900 />

<br><br>

