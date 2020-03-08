# LHTML5 Spec

[![Documentation Status](https://readthedocs.org/projects/lhtml5/badge/?version=latest)](https://lhtml5.readthedocs.io/en/latest/?badge=latest)

March 2020 Edition

## Introduction
LHTML5 (Living HTML5) is a standard for describing dynamic HTML5. It exists to help empower web developers as a team encourage and effective communication. LHTML5 provides a structured standardization for both a document language and processing standard. 

The document language section defines the standard for a LHTML5 document (referred to as a "document"). Its syntax is similar to HTML5 but permits additional custom elements and attributes that are used to inform the processor. Unlike HTML5, which describes content for the web browser, LHMTL5 allows internal stakeholders to add instructions that produce dynamic pages.

The processor standard section defines how the document language is used to build web pages. A document is past into program (referred to as "processor", "interpreter", or "parser") that builds a HTML5 page. It focuses primarily on how configured elements and attributes serve as instructions to instantiate modules, perform coordinated logical functions, and replace themselves with rendered content. 

Anyone familiar with HTML5 should find LHTML5 a breeze. Let's dive right in! 

### Copyright notice
Copyright (c) 2017-present Matthew Heroux

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

### Conformance
A conforming implementation of LHTML5 must fulfill all normative requirements. Conformance requirements are described in this document via both descriptive assertions and key words with clearly defined meanings.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY ", and "OPTIONAL" will be used as defined in [RFC2119](https://www.ietf.org/rfc/rfc2119.txt). When used with the normative RFC2119 meanings, they will be all uppercase. Occurrences of these words in lowercase comprise normal prose usage, with no normative implications.

## Preamble

The HTML5 standard was not created for dynamic markup. To produce dynamic markup a templating language is commonly embedded  within the HTML5 document. A templating engine then processes the document while remaining ignorant of the markup. It searches for its own flavor of templating syntax and then combines it with the data model. A language that is ignorant of the HTML5 is not well-suite for combining it with the data model.

The reason an embedded templating language is commonly used to create dynamic markup within HTML5 appears to date back to 1986. For that is when the SGML (Standard Generalized Markup Language) standard, which HTML in many ways stems from, was accepted. SGML was based on two postulates (or assumptions):
+ Declarative: Markup should describe a document's structure and other attributes rather than specify the processing that needs to be performed, because it is less likely to conflict with future developments.
+ Rigorous: In order to allow markup to take advantage of the techniques available for processing rigorously defined objects like programs and databases ([reference](https://en.wikipedia.org/wiki/Standard_Generalized_Markup_Language)).

The LHTML5 standard questions the first axiom and extends it as follows:
+ Declarative: Markup SHOULD describe a document's structure and other attributes. It may contain simple instructions for the processor. It does not perform processing, because of separation of concerns, but is processed. The processor decides if and how to interpret these instructions and SHOULD remove or replace them with rendered content.

## Document Language
The document MUST consist of tree elements that contain attributes. It is RECOMMENDED that it be well-formatted markup. It is RECOMMENDED that the document feature a root element (i.e. `<html>`). It is RECOMMEND that all tags that are opened be closed. 

All of the markup contained within the document MUST adhere to either the static markup or the dynamic markup standards.

### Static Markup
The document may contain static markup. Static markup is markup that the processor does not receive instructions from. Static markup MUST adhere to the [HTML5 spec](https://html.spec.whatwg.org/multipage/), which details how modern markup documents are delivered to the browser. Static markup is most often page specific markup that makes sense to manage within the document. The paragraph text of a page is a good example of content that often makes sense to remain as static markup. 

### Dynamic Markup
Dynamic markup is the powerhouse of the document. Dynamic markup is markup that the processor MUST be able to alter during runtime. This type of markup, which serve as instructions for the processor, is entirely optional. It include the presence of custom attributes, custom elements, and argument elements that may or may not be defined in the HTML5 spec. 

These tags and attributes act as the blueprints for dynamic content. The tags serve as placeholders for that dynamic content and once processed will be replaced with valid HTML5.The document may make use of any tags as dynamic markup. Even pre-exist HTML5 tags can be enhanced or transformed. For example, with LHTML5 for accessibility compliance, the alt attribute could be set to decorator when missing from `img` elements. 

The processor's builders generally replace dynamic elements with rendered dynamic content that was not previously within the element.
 
### Custom Attributes
The document language permits the use of custom element attributes, which are not defined in the HTML5 spec. 

#### Example 
The following example show an invalid HTML5 attribute, "type", within the `<head>` element. When this document is passed to an LHTML5 parser (along with the proper config and modules) the parser will instantiate the `<head>` element as a module and replaces the invalid element with HTML5 valid render content. 

```html5
<!doctype html>
<html lang="en">
<head type="default"/>
<body>
  <script src="js/scripts.js"></script>
</body>
</html>
```

### Custom Elements
LHTML5 design encourages the creation of custom elements over building a processing tree consisting of large nested logical elements that are difficult to maintain. The document allows for the use of custom elements that are not defined in the HTML5 spec. 

#### Example
The following shows an example of an unparsed document containing a few custom elements. This document would not be suitable to send to a site visitor's web browser without having been pass through to a parser.

This example makes use of four Modules. These modules are instantiated using the `<html>`, `<block>`, `<news>` and `<footer>` elements. The `<html>` element invokes a module that adds the completed `<head>` element. The `<block>` element is replaced by a fully built HTML5 navigation bar. The `<h1>` element is part of the static page content; itt remains uninitiated and unaltered. The `<news>` element pulls up to 20 news stories from a database and display them with a `<div>` containing thumbnails and a headline. The `<footer>` section is automatically populated with a copyright notice.

```html5
<html>
    <block name="NavBar" style="light"/>
    <h1>News &amp; Events</h1>
    <news>
        <arg name="limit">20</arg>
        <arg name"style">thumnail</arg>
    </news>
    <footer/>
</html>
```

### Arguments 
An argument is a value that is sent to the Module when it is called.

#### Argument Attributes
The processor MUST pass a dynamic elements attributes as  arguments. Storing and pass arguments as element attributes has its limits, as too much content can decrease readability. 

#### Example
In the following example, `block` features three arguments:
+ `name` set to the value "Test"; 
+ `min` set to a value of 0; and 
+ `limit` set to a value of 1. 

```lhtml5
<block name="Test" min="0" limit="1"/>
```

#### Argument Elements
Arguments can be added as a children of the element using the `<arg/>` element. When using this method the `<arg>` name attribute specifies the argument's name and the inner contents specify the value.

#### Example
In the following example, `block` features three arguments:
+ `name` set to the value "Test"; 
+ `min` set to a value of 0; and 
+ `limit` set to a value of 1. 

```lhtml5
<block name="Test">
    <arg name="min">0</arg>
    <arg name="limit">1</arg>
</block>
```

#### Argument Type Definition
A processor implementation may be written in a language that makes use of strict types. It is RECOMMENDED to define these data types within the `<arg>` element's "type" attribute. The various types options available are not defined within this standard because they may vary depending on the language. 
 
##### Example

```lhtml5
<block name="Test">
    <arg name="id" type="int">0</arg>
    <arg name="msg" type="string">Hello</arg>
    <arg name="metadata" type="json">{"content":"50"}</arg>
</block>
```


### Dialect
The presence of custom elements and attributes in turn alters and shapes the project's dialect. That is why it is important to thoughtfully design these language changes. A decisive factor in success of a dialect (and thus the project's success) is its effectiveness to serve as a message to communicate between its stakeholders. A dialect's design is RECOMMENDED to carry a message that allows project stakeholders to effectively communicate. These stakeholders MAY include any of the following:

+ Backend developer;
+ Template designers;
+ Frontend developers;
+ UX/UI designers;
+ WYSIWYG users.

The present of these elements allows internal stakeholders to communicate design. Elements and arguments can be passed from front end developers to backend end developers 

### Processor Standards
When an LHTML5 document is build for site visitor it SHOULD adhere to HTML5 standards without exception. 

#### Builders
The same LHTML5 document may be built different ways depending on the specified builder. 

A builder may decide not to parse modules. 

+ Search indexes;
+ the Web browser.

#### Configuration
A valid configuration must be capable of removing all unpermited HTML5 attributes.

#### Construction

#### Modules
LHTML5 is a modular language for emergent purposes. Modules are the worker bees of LHTML. Potential uses of LHTML5 modules include, but are not limited to:

+ reduce architectural debt and clutter of redundant elements present across multiple pages;
+ extend backend features to frontend WYSIWYG users;
+ transform existing elements to ensure compliance;
+ increase productivity by allowing for HTML5 short hand;
+ separate complex page logic from the template;
+ embed simple conditional logic;
+ enable the use of dynamic content (such as variables);
+ custom elements that are used to instantiate modules; and
+ others.


The processor's config SHOULD be responsible for determining which modules to instantiate. This config MUST 
describe each module using the following fields:

| Field | Summary |
| --- | ---| 
| `name` | Machine readable identifier for the module. |
| `xpath` | Find elements within the document using an XPath expression. |
| `class_name` | Determines what class to instantiate the module as. Maybe either a custom element or a native HTML5 element. |


 
##### XPath Expression 
A Module is instantiated as object when elements are found within the document using XPath expressions. Only Modules defined in the processor config that are found within the document are turned to objects. 

| Type | Example | Description |
| --- | --- | --- |
| Native | `<head/>`  | A module instantiated using an preexisting HTML5 element. This is often the case when the element exist within the page but needs to be corrected or auto completed during parsing. | 
| Custom | `<block/>` | A module instantiated using a element not defined within the HTML5 spec. This is useful for namespacing custom elements that WYSIWYG users can drop into a page or when defining new content types. It's a way of adding a term to communicate a feature to web stakeholders. |

Elements not found MUST not be instantiated and SHOULD remain unaltered. 

##### Class Name
During parsing, a modules `class_name` determines what class the module is instantiated as. These class name
The type 

The Module's `class_name` may use the element's attributes as variables to resolve the class. Depending on the config, the following may show an example of a module that is instantiated the either the class `Modules/Block/Test` or `Modules/Block`.

Depending on the configuration classes not found MAY be marked with one of the following error handlers
`<!-- NOT FOUND -->`
`<!-- NOT FOUND -->`

## `args`
During runtime the processor takes specified elements and instantiates them as objects. The element can feature arguments that are passed to the Module as properties. An argument's purpose is to be passed as a parameter, used by a module's method.

### Arguments from Derived Attribute 
Arguments can be added as attributes within an element. The following is an example of an argument `limit` being set to 1.
```lhtml5
<block name="Test" limit="1"/>
```

### Arguments from Child Arguments
Using arguments in the form of attributes has its limitations. Arguments can also be added as a children of the element using the `arg` element. In the following example, `block` features an arg named `min` set to a value of 0 and an arg `limit` set to a value of 1. 
```lhtml5
<block name="Test">
    <arg name="min">0</arg>
    <arg name="limit">1</arg>
</block>
```
### Array Arguments
When multiple arguments are passed using the same name it creates an argument array. The following arg `type` contains an array with three values: pickles, ketchup, and mustard.
```lhtml5
<block name="Test">
    <arg name="type">pickles</arg>
    <arg name="type">ketchup</arg>
    <arg name="type">mustard</arg>
</block>
```

## Modules
### Nested Modules
Modules can be nested inside one another. The following shows an example of a `var`, which is short for variable, module nested inside a `block` module.
```html5
<block name="UserProfile">
    <var name="fist_name"/>
</block>
```

#### Module Ancestor Properties
All Modules MUST be able to access their own private variables. In addition, all Modules can access their ancestors element's public variables. 

##### Example

```HTML
 <block name="ModuleA">
 	<block name="ModuleB">
 		<block name="ModuleC"/>
 	</block>
 	<block name="ModuleD">
 		<block name="ModuleE">
 	</block>
 </block>
```
* ModuleA can access no other modules public properties. 
* ModuleB can access ModuleA's public properties.
* ModuleC can access ModuleA's and ModuleB's public properties.
* ModuleD can access ModuleA's public properties.
* ModuleE can access ModuleA's and ModuleD's public properties.

## Module Methods
The config defines method calls to be orchestrated against all the modules instantiated. The methods can differ project to project but it stands to reason that one of the last ones will render the output from the module and its content will replace the original element entirely.
