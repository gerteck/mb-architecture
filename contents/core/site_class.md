---
  title: "Site Class & Site Config Class"
  pageNav: 2
  pageNavTitle: "Site, SitConfig Classes"
---

<br>

# Site Class

The `Site` class is initialized with configuration details (either default or user-specified) and is responsible for tasks such as generating the static site, handling page rendering, and deploying the built site to a GitHub repository. 

It interacts with other classes like SiteConfig, Page, VariableProcessor, PluginManager. Key methods of Site include generate, rebuildAffectedSourceFiles, deploy, and buildAssets. The component diagram helps to visualize the interactions between the `Site` class and the external components such as `fs`, `gh-pages`, and `logger`.

## Site Component Diagram

<puml src="{{ baseUrl }}/diagrams/site/component.puml" width=8000 />

<br>

The arrows represents the interactions between the components. The Site depends on external libraries for related operations, while the site component uses / calls other relevant components to execute specified operations.


<br>

<panel header="Site Class Component Diagram Explanation">

<box type="info"> 

The **Site Component Diagram** visualizes the interactions between the `Site` class and its associated components. Here's a breakdown of what is happening:

---

#### **1. MarkBind Site Package**
This package contains the core components of the MarkBind site generation process.

- **`Site` Class**:
  - The central component responsible for managing the site generation, rebuilding, and deployment.
  - It interacts with other components like `SiteConfig`, `Page`, `VariableProcessor`, `PluginManager`, etc.

- **`SiteConfig`**:
  - Manages the site configuration (e.g., `site.json`).
  - Provides configuration details like `baseUrl`, `pages`, `ignore`, etc.

- **`Page`**:
  - Represents a single page in the site.
  - Handles the generation of HTML from Markdown or other source files.

- **`VariableProcessor`**:
  - Processes variables defined in the site (e.g., `{{ baseUrl }}`, `{{ timestamp }}`).
  - Ensures variables are replaced with their actual values during site generation.

- **`PluginManager`**:
  - Manages plugins that extend the functionality of MarkBind.
  - Handles plugin lifecycle events like `beforeSiteGenerate` and `afterSiteGenerate`.

- **`SiteLinkManager`**:
  - Manages internal links within the site.
  - Ensures links are valid and properly resolved.

- **`ExternalManager`**:
  - Handles external resources like scripts, stylesheets, and assets.
  - Ensures external dependencies are included in the generated site.

- **`LayoutManager`**:
  - Manages the layout templates used for rendering pages.
  - Ensures consistent layout across all pages.

---

#### **2. External Libraries Package**
This package includes external libraries and utilities used by the `Site` class.

- **`fs`**:
  - Handles file system operations like reading, writing, and copying files.
  - Used for tasks like copying assets, reading configuration files, and writing generated HTML.

- **`ghpages`**:
  - A library for deploying the site to GitHub Pages.
  - Used in the `deploy` method to publish the site.

- **`logger`**:
  - Provides logging functionality for debugging and information purposes.
  - Logs messages like build progress, warnings, and errors.

---

#### **3. User Interaction**
- **`User`**:
  - Represents the end user who interacts with the `Site` class.
  - Triggers actions like `generate`, `rebuild`, and `deploy`.

---

#### **Key Interactions**
- The `Site` class acts as the central hub, coordinating interactions between all components.
- It reads configuration from `SiteConfig`, generates pages using `Page`, processes variables with `VariableProcessor`, and manages plugins with `PluginManager`.
- External libraries like `fs` and `ghpages` are used for file operations and deployment.
- The `User` initiates actions like site generation and deployment.

---

#### **How It Works**
1. The `User` triggers an action (e.g., `generate`, `rebuild`, or `deploy`).
2. The `Site` class coordinates with other components to perform the required tasks.
3. External libraries like `fs` and `ghpages` handle file operations and deployment.
4. The result (e.g., generated site or deployment URL) is returned to the `User`.

---

This diagram provides a high-level overview of the interactions between the `Site` class and its components, making it easier to understand the structure and flow of the MarkBind site generation process.

</box>

</panel>

<br><br>


The `Site` class is quite a heavyweight, and here is a class diagram for a large overview:

## Site Class Diagram

<puml src="{{ baseUrl }}/diagrams/site/site_class.puml" width=1000 />

<br>
<br>



## Site.Generate & Rebuild

In the static site build process, the Site object handles the generation of the site. An overview of this process is shown in the following sequence diagram.

<puml src="{{ baseUrl }}/diagrams/site/site_generate.puml" width=1000 />

<br><br>

The method `buildSourceFiles` is called, which in turn calls `generatePages()`. This renders all pages specified in the site configuration file to the output folder. We do this by running `page.generate()` for all collected pages.

Similarly, the rebuild process follows a similar process.

## Site.Deploy

<center>
  <puml src="{{ baseUrl }}/diagrams/site/deploy.puml" width=400 />
</center>

<br>

For deployment, MarkBind makes use of the `gh-pages` library to publish files to a gh-pages branch on GitHub. 

If the `no-build` option is specified and if the site files are not built, which it verifies with the npm library `fs-extra`, it will throw an error. 

The `build` process is a pre-requisite of the `deployment` process.

The `deploy` process automatically calls the `build` (generate) process otherwise.



<br><br>

# **SiteConfig Class Overview**  

The `SiteConfig` class is located in `packages/core/src/Site/SiteConfig.ts` and is responsible for reading, validating, and managing the site configuration (`site.json`). It provides a structured representation of the configuration with default values for unspecified properties.  

This class plays a crucial role in defining key aspects of a MarkBind site, such as:  

- **Site metadata** (title, locale, timezone)  
- **Page settings** (layouts, external scripts, searchability)  
- **Deployment configuration** (GitHub Pages repo, branch, commit message)  
- **Plugins and extensions** (custom plugins and plugin contexts)  

The `SiteConfig` class is primarily used during site initialization, site generation, and deployment.  

<center>
  <puml src="{{ baseUrl }}/diagrams/site/siteConfig.puml" width=400 />
</center>

<br> <br>

---

## **Key Properties and Functionalities**  

### **1. Base URL Handling**  
```ts
this.baseUrl = cliBaseUrl !== undefined
  ? cliBaseUrl
  : (siteConfigJson.baseUrl || '');
```

The baseUrl is retrieved from the site.json file unless a command-line --baseUrl argument is provided, in which case the CLI value takes precedence.

### 2. Pages and Layout Configuration

```ts
this.pages = siteConfigJson.pages || [];
this.pagesExclude = siteConfigJson.pagesExclude || [];
this.ignore = siteConfigJson.ignore || [];
```
* pages: Defines the pages included in the site.
* pagesExclude: Specifies files that should be excluded from the site.
* ignore: A list of files that MarkBind should ignore during site generation.

Each page entry can define specific settings such as a custom layout, external scripts, or front matter overrides.


### 3. Styling Configuration

```ts
this.style = siteConfigJson.style || {};
this.style.codeTheme = this.style.codeTheme || 'dark';
this.style.codeLineNumbers = this.style.codeLineNumbers !== undefined
  ? this.style.codeLineNumbers : false;
```
Defines the site's styling preferences, including, 
* codeTheme: Can be 'dark' or 'light'.
* codeLineNumbers: Determines whether line numbers should be displayed in code blocks (default: false).

### 4. Deployment Configuration

```ts
this.deploy = siteConfigJson.deploy || {};
```

Stores deployment-related settings such as, 
* repo: The repository to deploy the site to.
* branch: The branch used for deployment (e.g., gh-pages) 
* message: The commit message for deployment, 
* baseDir: The base directory for deployment.

This configuration is used by the markbind deploy command when pushing the generated site to GitHub Pages.

### 5. Plugin System
```ts
this.plugins = siteConfigJson.plugins || [];
this.pluginsContext = siteConfigJson.pluginsContext || {};
```

* plugins: Stores a list of active plugins for the site. 
* pluginsContext: Holds specific configuration settings for each plugin. 

This allows users to extend MarkBind’s functionality with custom plugins.

### 6. Intrasite Link Validation
```ts
this.intrasiteLinkValidation = siteConfigJson.intrasiteLinkValidation || {};
this.intrasiteLinkValidation.enabled = this.intrasiteLinkValidation.enabled !== false;
```

Ensures that internal links within the site are valid and do not point to non-existent pages.


### 7. Reading the Site Configuration File
```ts
static async readSiteConfig(rootPath: string, siteConfigPath: string, baseUrl?: string): Promise<any> {
  try {
    const absoluteSiteConfigPath = path.join(rootPath, siteConfigPath);
    const siteConfigJson = fs.readJsonSync(absoluteSiteConfigPath);
    const siteConfig = new SiteConfig(siteConfigJson, baseUrl);

    return siteConfig;
  } catch (err) {
    throw new Error(`Failed to read the site config file '${siteConfigPath}' at`
      + `${rootPath}:\n${(err as Error).message}\nPlease ensure the file exists or is valid`);
  }
}
```

Reads the site.json configuration file and initializes a SiteConfig object. If the file does not exist or is invalid, an error is thrown.
