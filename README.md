# Jellyfish3
Ideas for describing software architecture as code is new DSL.  It should be

* easy to read and write, especially for non-coders
* have a limited set of keywords and syntax
* have minimal tooling requirements 

The syntax **isn't YAML**...  It Just Looks Like YAML (JLLY).  Also, `.jlly` would be a 
good file extension.

Review in this order:

1. One file showing syntax: [AlarmClock.sd3](simple-project/AlarmClock.sd3)
1. A project following a package structure: [AlarmClock.sd3](intermediate-project/alarm/AlarmClock.sd3)
1. Custom extensions
    * [AlarmKlock.sd3](intermediate-project/customklocks/AlarmKlock.sd3)
    * [extensions.sd3](intermediate-project/meta-model/extensions.sd3)

## Features

* Unspecified fields and components: Supports incremental modeling to avoid having to declare everything all 
  at once or up front.
* Simple syntax: Almost like YAML but friendly support for non-YAML blocks.
* Custom extensions: Extend the DSL in any language you want.  Intergrate existing models, data definitions, IDLs, etc easily by injecting
  data types and models dynamically.  
* Simple tooling: a single EXE that parses/validates and generates JSON.  Pass the JSON to some custom app
  that can do whatever.

## Advanced workflows
The Jellyfish tooling itself now just parses & validates.  It could produce a JSON description of the project
which can then be passed to other tools to do things like document generation, code generation, analysis, etc.
A tool like Gradle or something could be used to orchestrate the workflow. 

# Thomas Normalized Form

```
package: <string>

import data:
  <data reference>*

import model:
  <model reference>*

(data: or model:)*

data: <string>
  (extends: <data reference>)?
  (doc: <text>)?
  values: or fields:

values:
  <string>+

fields:
  (<data reference or primitive> <string>)+

primitive:
  boolean
  integer
  decimal
  string
  text # text is multiline string
  markdwon # built in support for markdown

model: <string>
  (extends: <model reference>)?
  (doc: <text>)?
  (inputs:)?
  (outputs:)?
  (components:)?
  (gherkin:)? # built in support for Gherkin
  
inputs: 
  (<data reference or primitive> <string>)*

outputs:
  (<data reference or primitive> <string>)*

components:
  (<model reference> <string>)*

gherkin:
  (feature: or file:)*

feature: <Gherkin syntax>

file: <string>

```