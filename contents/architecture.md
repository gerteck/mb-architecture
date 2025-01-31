<frontmatter>
  title: Architecture and Packages
  pageNav: 2
</frontmatter>

# Packages

The MarkBind project is developed in a <tooltip content="We follow a monorepo approach, similar to Babel and other open source projects. To see a discussion on the pros and cons of this approach, read [here](https://github.com/babel/babel/blob/main/doc/design/monorepo.md).">monorepo</tooltip> ([MarkBind/markbind](https://github.com/MarkBind/markbind)) of 4 packages:


* The core library, which parses and processes MarkBind's various syntaxes, resides in the `packages/core/` directory.


* The command-line interface (CLI) application, which accepts commands from users and then uses the core library to parse and generate web pages, resides in the `packages/cli/` directory.


* The core web library, which contains a generated web bundle from various setup scripts and the UI components library, resides in `packages/core-web/`.


* The UI components library, which MarkBind authors can use to create content with complex and interactive structure, resides in the `packages/vue-components/` directory.


  Stack used: *Node.js*, *Vue.js*


### MarkBind core library


**The core library mainly houses:**


* Functions and libraries used to parse and process MarkBind into usable output are stored in `src`. The architecture described in [Architecture](architecture.md) is contained here. A brief rundown of what it includes:


  * Various key functionalities in processing MarkBind syntax into valid HTML output, stored in `html`. The other part of the content processing flow is found in `variables`, which manages site variables and facilitates the Nunjucks calls.


  * `Page` files generate a single page of the site, and are managed by the `Site` instance. `Site` uses the Page model's interface to generate pages, and performs various other utility-like functions related to site generation such as copying of external assets into the output folder.


  * `Layout` holds the files relating to the layout of the site and are managed by `LayoutManager`. Similarly, `External` files, which are separate output files to be loaded dynamically and on-demand, are managed by a `ExternalManager` instance.


  * Various libraries (contained in `lib`) and plugins (in `plugins`) are also stored here. Some external libraries have also been amended to suit MarkBind's purpose – see `patches`.


* MarkBind's <a tags="environment--combined" href="/userGuide/templates.html">templates</a><a tags="environment--dg" href="https://markbind.org/userGuide/templates.html">templates</a>, used in the `markbind init` command.


* Unit Tests (though there are more unit tests and functional tests in the cli library)


**The key external libraries used are:**


* [markdown-it](https://github.com/markdown-it/markdown-it), which does the Markdown parsing and rendering. There are also several customized markdown-it plugins used in MarkBind, which are located inside the `src/lib/markdown-it/` directory.


  * Several markdown-it plugins are installed to enhance the existing Markdown syntax. They can be found in `src/package.json`. Some of them are patched in the `src/lib/markdown-it/patches/` directory to fit MarkBind's needs.


  * Additionally, there are some markdown-it plugins in the `src/lib/markdown-it/plugins/` directory (either forked, modified or written to enhance existing functionalities).


* [htmlparser2](https://github.com/fb55/htmlparser2), a speedy and forgiving HTML parser which exposes a DOM-like object structure to work on. To comply with the markdown spec, and our custom requirements, `src/patches/htmlparser2.js` patches various behaviours of this library.


* [cheerio](https://cheerio.js.org/), which is a node.js equivalent of [jQuery](https://jquery.com/). Cheerio uses [htmlparser2](https://github.com/fb55/htmlparser2) to parse the HTML as well, hence our patches propagate here.


* [Nunjucks](https://mozilla.github.io/nunjucks/), which is a JavaScript templating engine. Nunjucks is used to support our variable system to help with reusing small bits of code in multiple places. The package is patched and stored in `src/patches/nunjucks` to make it compatible with other MarkBind syntax processing steps.


### MarkBind CLI


The CLI application uses and further builds on the interface exposed by the core library's `Site` model to provide functionalities for the author, such as `markbind serve` which initiates a live reload workflow.


**The key external libraries used are:**


* [commander.js](https://github.com/tj/commander.js/), which is a node.js CLI framework.


* [live-server](https://github.com/tapio/live-server), which is a simple web server for local development and preview of a MarkBind site. The package is patched and stored in `src/lib/live-server` with our custom fine tuning.


### MarkBind core-web library


This package houses the various frontend assets used in the core package.


Some external assets included are Vue.js, Bootstrap bundles, and FontAwesome bundles.


Internal bundles are also present, generated from setup scripts, custom stylesheets and the UI components library.


### UI components library


This package consists of a mix of [Bootstrap](https://getbootstrap.com/components/) and proprietary components rewritten in [Vue.js](https://vuejs.org) based on our needs for educational websites.


We forked it from the original [yuche/vue-strap](https://github.com/yuche/vue-strap) repo into the [MarkBind/vue-strap](https://github.com/MarkBind/vue-strap) repo, and then later merged it into the main [MarkBind/markbind](https://github.com/MarkBind/markbind) repo.



# Architecture

<div class="lead mb-5">

This page provides an overview of the MarkBind's architecture.
</div>

![MarkBind Architecture Diagram]({{baseUrl}}/diagrams/architecture.png)

The above diagram shows the key classes and <popover content="The content processing flow acts on a **single** source file (`.md` / `.html`), generating output files or intermediate processing results depending on the content type.">content processing flow</popover> in MarkBind. You may note the following from these:

### Key classes

The 3 key classes representing different types of content are as follows:

1. `Page` — a page, as specified by the user in their various site configuration `glob` or `src` properties. These are directly managed by the `Site` instance.
1. `Layout` — a single layout file, as contained in the `_markbind/layouts` folder, collectively managed by `LayoutManager`.

<box type="tip" seamless>

Note that `Layout` instances do not generate any output files directly, but rather, they store intermediate processing results to be used in a `Page`.
</box>

1. `External` — source files referenced in a `Page`, `Layout` or even another `External` that result in a <tooltip content="hence the class naming `External`">**separate**</tooltip> output file. These output files are loaded dynamically and on-demand in the browser. These instances are managed by a single `ExternalManager` instance.

<box type="info" seamless>

For example, the file referenced by a `<panel src="xx.md">` generates a separate `xx._include_.html`, which is requested and loaded only when the panel is expanded.
</box>

### Content processing flow

Every `Page`, `Layout` and `External` follows the same overall content processing flow, that is:

_Nunjucks :fas-arrow-right: Markdown :fas-arrow-right: Html_

<box type="info" seamless>

Note that generation of `External`s (e.g. `<panel src="...">`) **do not fall within** the content processing flow !!where they are **referenced**!!.
These are only flagged for generation, and then processed by `ExternalManager` **after**, in **another** similar content processing flow within an `External` instance.
</box>

****Rationale****

This simple three stage flow provides a simple, predictable content processing flow for the user, and removes several other development concerns:

1. As the templating language of choice, Nunjucks is always processed first, allowing its templating capabilities to be used to the full extent.
Its syntax is also the most compatible and independent of the other stages.

2. Secondly, Markdown is **rendered before HTML**, which produces more HTML. This also allows core Markdown features (e.g. code blocks) and Markdown plugins with eccentric syntaxes to be used without having to patch the HTML parser.

3. Having processed possibly conflicting Nunjucks and Markdown syntax, HTML is then processed last.


<div class="clearfix">
  <span class="float-start"><a class="btn btn-light" href="{{ baseUrl }}/index.html"><md>:far-arrow-alt-circle-left: Home</md></a>
  </span>
  <span class="float-end"><a class="btn btn-light" href="./cli.html">
    <md> Command Line Interface :far-arrow-alt-circle-right: </md>
    </a>
  </span>
</div>

<br>


# How MarkBind Works

A summary from senior [CS3281-2024](https://nus-cs3281.github.io/2024/#xu-shuyao)


## MarkBind Rendering Flow

The rendering flow for creating a complete website from Markdown using MarkBind involves several key components and processes:

### **1. Site Initialization**
- The rendering process starts with the initialization of a `Site` instance in `Site/index.ts`.
- The `Site` constructor sets up the necessary paths, templates, and initializes various managers and processors.

### **2. Site Configuration**
- The `readSiteConfig` method in `Site/index.ts` reads the site configuration file (`site.json`) using the `SiteConfig` class.
- The site configuration includes settings such as base URL, page configuration, asset paths, and more.

### **3. Page Collection**
- The `collectAddressablePages` method in `Site/index.ts` collects the addressable pages based on the site configuration.
- It processes the `pages` and `pagesExclude` options to determine the valid pages.

### **4. Plugin Setup**
- The `PluginManager` class in `plugins/PluginManager.ts` handles the initialization and management of plugins.
- It collects the specified plugins from the site configuration and loads them using the `Plugin` class.

### **5. Asset Building**
- The `buildAssets` method in `Site/index.ts` builds the necessary assets for the site.
- It copies the specified assets from the site's root path to the output path.

### **6. Markdown Processing**
- The `generatePages` method in `Site/index.ts` initiates the rendering process for each page.
- It creates a `Page` instance for each page using the `createPage` method.
- The `Page` class in `Page/index.ts` handles the rendering of individual pages.
- It uses the `process` method of the `NodeProcessor` class to process the Markdown content.

### **7. Plugin Execution**
- During the page rendering process, the `PluginManager` executes the relevant hooks for each plugin.
- Plugins can define hooks such as `beforeSiteGenerate`, `processNode`, `postRender`, etc., to modify the page content or perform additional tasks.

### **8. Layout and Template Rendering**
- The `LayoutManager` class in `Layout/LayoutManager.ts` handles the generation and combination of layouts with pages.
- It uses the `Layout` class to process and render the layout files.
- The rendered pages are then passed through the `VariableProcessor` to replace variables with their corresponding values.

### **9. Post-rendering Tasks**
- After rendering, additional tasks such as writing site data and copying core web assets are performed.

### **10. Output Generation**
- Finally, the rendered pages and assets are written to the output directory specified during site initialization.

---

## **Vue.js Integration in MarkBind**
MarkBind integrates Vue.js for building user interfaces and enhancing the rendering process. Here's how Vue.js is used in MarkBind:

### **1. Server-Side Rendering (SSR)**
- MarkBind utilizes Vue's **server-side rendering** capabilities to pre-render the pages on the server.
- The `pageVueServerRenderer` module in `Page/PageVueServerRenderer.ts` handles the compilation of Vue pages and the creation of render functions.

### **2. Client-Side Hydration**
- After the initial **server-side rendering**, the rendered HTML is sent to the client.
- On the **client-side**, Vue takes over and **hydrates** the pre-rendered HTML, making it interactive and reactive.

### **3. Vue Components**
- MarkBind defines various **Vue components** to encapsulate reusable UI elements and functionality.
- These components are used throughout the rendered pages to enhance the user experience.

### **4. Integration with MarkBind Plugins**
- MarkBind plugins can also utilize **Vue.js** to extend the functionality of the application.
- Plugins can define **custom Vue components, directives, and hooks** to interact with the rendering process.

---

## **Build Process and Asset Management**
MarkBind follows a build process to generate the final website and manage the necessary assets:

### **1. Asset Building**
- The `buildAssets` method in `Site/index.ts` handles the **building of assets** for the site.
- It copies the specified assets from the site's root path to the output path.

### **2. Core Web Assets**
- MarkBind relies on **core web assets** such as CSS and JavaScript files.
- The `copyCoreWebAsset` method in `Site/index.ts` copies these assets from the `@markbind/core-web` package to the output directory.

### **3. Plugin Assets**
- Plugins can also provide their own assets, such as **CSS and JavaScript files**.
- The `PluginManager` handles the **collection and copying** of plugin assets to the output directory.

### **4. Icon Assets**
- MarkBind supports various **icon libraries**, including:
  - Font Awesome
  - Glyphicons
  - Octicons
  - Material Icons
- The following methods in `Site/index.ts` handle the copying of these assets:
  - `copyFontAwesomeAsset`
  - `copyOcticonsAsset`
  - `copyMaterialIconsAsset`

### **5. Bootstrap Theme**
- MarkBind allows customization of the **Bootstrap theme** used in the site.
- The `copyBootstrapTheme` method in `Site/index.ts` handles the copying of the selected Bootstrap theme to the output directory.


