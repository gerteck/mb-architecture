---
  title: "Page Class"
  pageNav: 2
  pageNavTitle: "Page Class"
---

# Page Class


The Page class represents a single page in a MarkBind site. It manages the generation of HTML content from Markdown or other source files. It interacts with other components like PageConfig, SiteConfig, NodeProcessor, ExternalManager, etc.

<br>

<puml src="{{ baseUrl }}/diagrams/site/page_class.puml" width=900 />

<br><br>


## 1. Key Properties
* pageConfig: Configuration for the page (e.g., source path, result path, assets).
* siteConfig: Configuration for the site (e.g., base URL, title prefix, ignore patterns).
* asset: Assets (e.g., CSS, JS) used by the page.
* frontmatter: Frontmatter data extracted from the page.
* headings: Map of heading IDs to their text content.
* keywords: Map of heading IDs to keywords.
* navigableHeadings: Headings used for page navigation.



## 2. Key Methods
* resetState(): Resets or initializes stateful variables for the page.
* isDependency(filePath): Checks if a file is a dependency of the page.
* prepareTemplateData(content): Prepares data for rendering the page template.
* filterIconAssets(preVueSsrHtml, postVueSsrHtml): Filters out unused icon assets.
* collectNavigableHeadings(content): Collects headings for page navigation.
* collectHeadingsAndKeywords(pageContent): Collects headings and keywords from the page content.
* processFrontmatter(frontmatter): Processes frontmatter data.
* buildPageNav(content): Builds the page navigation bar.
* generate(externalManager): Generates the page HTML.
* outputPageHtml(content): Writes the generated HTML to the output file.


## 3. Interactions
* PageConfig: Provides configuration for the page.
* SiteConfig: Provides configuration for the site.
* NodeProcessor: Processes the page content (e.g., Markdown, includes).
* PageSources: Manages the sources of the page (e.g., included files).
* ExternalManager: Manages external resources (e.g., scripts, stylesheets).
* LayoutManager: Manages the layout of the page.
* PluginManager: Manages plugins that extend the functionality of the page.
* SiteLinkManager: Manages internal links within the site.
* VariableProcessor: Processes variables in the page content.
* cheerio: Used for parsing and manipulating HTML content.
* fs: Handles file system operations (e.g., reading, writing files).
* logger: Logs messages for debugging and information purposes.

## Page generate()

When the user initiates page generation, the `generate(externalManager)` method is called. It then resets all stateful variables of the page, to prepare for generating this fresh page. It then uses `variableProcessor` to render site variables in the page content. After that, it uses `NodeProcessor` to process the page content.

It then processes frontmatter data extracted by `NodeProcessor`, and uses `PluginManager`to post process the content by applying plugin transformations. At this point, it also collects plugin assets as well (e.g. scripts, styles).

The page navigation bar is built if needed, and it is combined with the layout with the page content using the `LayoutManager`. It then uses `ExternalManager` to generate external dependencies (e.g. scripts, stylesheets). It then collects headings and keywords.

Next, it uses `pageVueServerRenderer` to compile the page into a Vue application. Then, use it to server-side render the Vue page into HTML.

Then, we filter out unused icon assets, write the generated HTML to output file and completed!

<br>

<puml src="{{ baseUrl }}/diagrams/site/page_generate.puml" width=900 />

<br><br>


## PageConfig

The PageConfig class represents the configuration for a page in MarkBind, and contains properties that define how the page should be generated, including assets, layout and plugins.


<br>

<puml src="{{ baseUrl }}/diagrams/site/pageConfig.puml" width=900 />

<br><br>
