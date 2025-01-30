---
  title: "Page Vue Server Renderer"
  pageNav: 2
  pageNavTitle: "Page Vue Server Renderer"
---

# Page Vue Server Renderer

The `pageVueServerRenderer` module is responsible for compiling and rendering Vue pages in MarkBind.

It provides three main functions, 

* `compileVuePageAndCreateScript`: Compiles page into a render function (through creating a Vue application). places it into the js script, so that browser can retrieve the page render function, render page during Vue mounting. Done to avoid overhead of compiling the page into Vue application on browser. *(Browser doesn't have to do this step, since we already do it).*

* `renderVuePage`: Renders Vue Page into HTML string using Vue SSR.

* `updateMarkBindVueBundle`: Hot reloading of MarkBind Vue Components, when the vue components change, this function updates the bundle and re-renders all pages.

It compiles Vue templates on the server, generates scripts for efficient client-side rendering and supports dynamic updates when MarkBind's Vue components change.

Note that both `compile` and `render` are used in `Page.generate()`. Refer to Page [here](./page_class.md).

<panel header="Page.generate() Sequence Diagram">

<br>

<puml src="{{ baseUrl }}/diagrams/site/page_generate.puml" width=900 />

<br><br>


</panel>


<br>

<puml src="{{ baseUrl }}/diagrams/site/pageVueServerRenderer.puml" width=900 />

<br><br>

