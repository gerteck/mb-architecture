---
  title: FAQ
---



# Vue Migration

What must I change?

* Most importantly, need to change the logic in `PageVueServerRenderer.ts` in Page.

* Update `core-web/src/index.js` setup process.
* Change `VueCommonAppFactory.js` in core-web.
  * `core-web/src/index.js`

* Need to change all the Vue Components

* Change how the `Vue` rendering and stuff is called in the `Page.generate` method if needed.

* Change `vueSlotSyntaxProcessor` in `core/src/html`
  * Transforms the deprecated slot syntax

Update certain plugins:

* update dataTable and Mermaid as they use Vue Directives



