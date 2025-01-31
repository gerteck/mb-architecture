---
  title: "Layout, Layour Manager Classes"
  pageNav: 2
  pageNavTitle: "Layout, Layour Manager Classes"
---

# Layout, Layout Manager Classes

The layout is defined in usually defined in a single layout file, typically `default.md`, contained in the `_markbind/layouts` folder. The `Layout` instance does not generate any output files directly but store intermediate processing results to be used in a `Page`.

<br>

<puml src="{{ baseUrl }}/diagrams/site/layout.puml" width=900 />

<br><br>

The `LayoutManager` class manages multiple `Layout` instances. Both of which depend on `LayoutConfig`, which extends `ExternalManagerConfig`.