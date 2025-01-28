<frontmatter>
  title: MarkBind CLI
</frontmatter>


# MarkBind Command Line Interface

<br>

MarkBind makes use of the `commander.js` npm package to build its CLI application, which simplifies the defining, parsing, of commands and adding options. 

<box type="tip">

Here is a component diagram of MarkBind CLI

</box>

<puml src="/diagrams/cli/markbind_cli.puml" width=900 />


<br> <br>

The command line supports a total of 4 commands. `init`, `serve`, `build` and `deploy`.

Each command is refactored into the `cli/src/cmd` folder.

# 1. markbind Init

Here is an activity diagram depicting the `init` function. 

<panel header="Activity Diagram - markbind init">

<puml src="/diagrams/cli/init.puml" width=900 />

</panel>

<br>

When we run `markbind init`, we call the async function `init(root, options)`. This function creates a new `Template` object, which either initalizes a new markbind project, or converts existing documentation into a markbind project.

<box type="info">


## Template Class

The `Template` class is under `core/src/Site`, and handles the generation of a site from a template. 

<panel header="Class Diagram - Template">

<puml src="/diagrams/cli/template.puml" width=900 />

</panel>

<br>

### Template Initialization

When `init` function calls the `template.init()` function it first validates the template option, then it simply copies the files into the existing directory. 

The markbind templates are located in `core/src/template`. Upon initalization, it checks that the template option chosen does exist in the given directory.

### Template Conversion

If the `--convert` flag is given in the cli, the program will attempt to convert existing files into a markbind project.

I believe that the primary use case for this is to convert a GitHub Wiki into a MarkBind project. For example, consider cloning [Font-Awesome's GitHub Wiki](https://github.com/fortawesome/font-awesome/wiki) and running `markbind init --convert`.

Essentially, it collects all pages, adds an index and about page, adds a default layout file that references all the existing markdown pages and builds the inital markbind template. 

</box>

# 2. markbind serve


Here is a sequence diagram of the general mechanism of `markbind serve`.

<puml src="/diagrams/cli/serve.puml" width=900 />





















