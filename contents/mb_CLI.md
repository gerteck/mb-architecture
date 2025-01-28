<frontmatter>
  title: MarkBind CLI
</frontmatter>


# MarkBind Command Line Interface

<br>

MarkBind makes use of the `commander.js` npm package to build its CLI application, which simplifies the defining, parsing, of commands and adding options. 


<puml src="/diagrams/cli/markbind_cli.puml" width=900 />


<br> <br>

The command line supports a total of 4 commands. `init`, `serve`, `build` and `deploy`.

Each command is refactored into the `cli/src/cmd` folder.

## markbind Init

When we run `markbind init`, we call the async function `init(root, options)`. This function creates a new `Template` object, which either initalizes a new markbind project, or converts existing documentation into a markbind project.

Here is an activity diagram depicting the `init` function. 

<panel header="Activity Diagram - markbind init">

<puml src="/diagrams/cli/init.puml" width=900 />


</panel>



