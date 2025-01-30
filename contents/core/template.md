---
  title: MarkBind Templates
  pageNav: 2
  pageNavTitle: "Template"
--- 
<br>

# MarkBind Templates 

MarkBind provides multiple templates to help users quickly set up their projects. When initializing a new MarkBind project, you can use the `--template <template-key>` flag to specify a template during `markbind init`

These templates are stored in `packages/core/template`.

Currently, there are 4 existing templates to choose from:
* Default
* Minimal
* Project
* Portfolio

These template files are used by the [Template Class]({{baseUrl}}/contents/core/template.md) when doing initialization of a markbind project. (`markbind init`).


# Template Class

The `Template` class is under `core/src/Site`, and handles the generation of a site from a template. 

### Template Class Diagram

<puml src="{{ baseUrl }}/diagrams/site/template.puml" width=900 />


<br> <br>

### Template Initialization

When `init` function calls the `template.init()` function it first validates the template option, then it simply copies the files into the existing directory. 

The markbind templates are located in `core/src/template`. Upon initalization, it checks that the template option chosen does exist in the given directory.

<br>

### Template Conversion

If the `--convert` flag is given in the cli, the program will attempt to convert existing files into a markbind project.

I believe that the primary use case for this is to convert a GitHub Wiki into a MarkBind project. For example, consider cloning [Font-Awesome's GitHub Wiki](https://github.com/fortawesome/font-awesome/wiki) and running `markbind init --convert`.

Essentially, it collects all pages, adds an index and about page, adds a default layout file that references all the existing markdown pages and builds the inital markbind template. 

## Template Sequence Diagram: Init and Convert

<puml src="{{ baseUrl }}/diagrams/site/template_sequence.puml" width=900 />

<br> <br>

## Explanation of the Sequence Diagram

### 1. `init` Process

1. The user calls `Template.init(root, options)`.
2. The `Template` class validates the template by checking if required files exist (`validateTemplateFromPath`).
3. If validation is successful:
   - Ensures the root directory exists (`fs.access` and `fs.mkdirSync`).
   - Copies the template files to the root directory (`fsUtil.copySyncWithOptions`).
   - Initialization is complete.
4. If validation fails, an error is returned to the user.

---

### 2. `convert` Process

1. The user calls `Template.convert()`.
2. The `Template` class reads the site configuration (`SiteConfig.readSiteConfig`).
3. It collects navigable pages (`collectNavigablePages`).
4. It adds an index page by copying `README.md` or `Home.md` to `index.md` (`fs.copy`).
5. It adds an about page by creating `about.md` if it doesn’t exist (`fs.outputFile`).
6. It adds default layout files:
   - Reads the footer and site navigation content (`fs.readFileSync`).
   - Compiles and renders the layout template (`VariableRenderer.compile` and `render`).
   - Writes the rendered layout to the output file (`fs.writeFileSync`).
7. It updates the site configuration to apply the default layout (`addDefaultLayoutToSiteConfig`).
8. Conversion is complete.

---

### Key Interactions

- **`fs`**: Handles file system operations like reading, writing, and copying files.
- **`SiteConfig`**: Manages the site configuration (`site.json`).
- **`VariableRenderer`**: Compiles and renders layout templates.
- **`logger`**: Logs information and warnings duri







