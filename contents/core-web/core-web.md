<frontmatter>
  title: Core Web Library
  pageNav: 2
</frontmatter>

# MarkBind Core Web

The @markbind/core-web package contains the browser-specific components and utilities for MarkBind. It provides the bundled JavaScript, styles, and configurations necessary for rendering MarkBind sites in the browser.

This package includes:  
* Vue Component Integration: Provides core Vue-based components and utilities for MarkBind.
  * Pre-built browser bundles for MarkBind’s Vue-based components. 
* Webpack configurations to compile client-side and server-side assets.
  * Webpack Bundling: Packages frontend assets (JavaScript and CSS) for use in the browser.
* Utility scripts for managing styles, printing, and rendering behavior.

It is primarily used by the MarkBind core package to facilitate web-based rendering of MarkBind content.


## Core-Web Library

This library contains a `asset` folder for storing static files such as css, js, glyphicon assets.

This library also contains the `/dist` folder, (short for distribution), which is where `Webpack` outputs the final bundled and optimized assets after running the build process.

## Dependency

The `core-web` library is used in
* `core/src/Site` in `copyCoreWebAsset()`
* `PageVueServerRenderer` as bundle


<br>

<puml src="{{ baseUrl }}/diagrams/core-web/module.puml" width=900 />

<br>
<br>

The module integrates the Vue components into the system through the `MarkBindVue` plugin. The `appFactory` helps create the Vue app instances needed for each page, to set up the necessary configurations.

The core styles for MarkBind are also included here.

Most importantly, the module provides Webpack configurations for both client-side and server-sider rendering, and builds and bundles assets for optimal performance. 

## Webpack

Webpack helps to bundle the js and css files together for delivery.

**Where and when is webpack used?**

Webpack is used in the @markbind/core-web package during the build process to bundle and optimize assets for both client-side and server-side usage. This happens when running `npm run build`, which triggers Webpack via webpack.build.js.


> Editing frontend features:
>
> We update the frontend markbind.min.js and markbind.min.css bundles during release only, and not in pull requests. Hence, if you need to view the latest frontend changes (relating to packages/core-web or packages/vue-components), you can either:
> 
> Run markbind serve -d (with any other applicable options). (recommended)
> This adds the necessary webpack middlewares to the development server to compile the above bundles, and enables live and hot reloading for frontend source files.
> 
> Run npm run build:web in the root directory, which builds the above bundles, then run your markbind-cli command of choice.


Also, note that `npm run build:web` is just an alias for `cd packages/core-web && npm run build`.



