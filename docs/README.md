# LHTML5

[![Documentation Status](https://readthedocs.org/projects/lhtml5/badge/?version=latest)](https://lhtml5.readthedocs.io/en/latest/?badge=latest)

February 2020 Edition

## Introduction
Living Hypertext Markup Language 5 (LHTML5) was adapted, over the subsequent years, to describe a standard for web servers to store and flexibly build dynamic HTML5 documents. 

LHTML5 syntax is similar to HTML5, which is a standard that describes content for the web browser. Unlike HTML5. LHTML5 enables a website's internal stakeholders to communicate through the presence of modular elements and attributes. These serve as instructions to instantiate modules, perform coordinated logical functions, and replace their origin with the rendered content. Anyone with knowledge of HTML5 should find LHTML5 simple to learn as there are only a few extra concepts. Let's dive right in! 

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

## Overview
The LHTML5 spec defines both a base language and a standard for parsing dynamic web page documents.

## Language
Web servers use LHTML5 language to store and communicate dynamic web pages. A document consisting of this language (referred to as a "LHTML5 document") is past into a parser to build a HTML5 page. Like HTML5, a LHTML5 document MUST consist of tree elements containing attributes and inner text. The document SHOULD adhere to [HTML5 spec](https://html.spec.whatwg.org/multipage/) (as it details how modern markup documents are delivered to the browser) ***except*** where module elements and child arguments are present. 

#### Custom Dialect
Design custom elements and attributes thoughtfully. The use of custom markup elements and attributes shapes the project's LHTML5 language dialect. A decisive factor in a project dialect's success (and thus the project's success) is its effectiveness communicate a message between its stakeholders. A dialect's design is RECOMMENDED to carry a message that allows project stakeholders to effectively communicate. These stakeholders MAY include any of the following:

+ Backend developer;
+ Template designers;
+ Search indexes;
+ Frontend developers;
+ UX/UI designers;
+ WYSIWYG users; and
+ the Web browser.

##### Attributes
LHTML5 modules allow for the language to make use of custom attributes that are not defined in the HTML5 spec. The following example show an invalid HTML5 attribute, "type", within the `<head>` element. When this document is past to an LHTML5 parser (along with the proper config and modules) the parser will instantiate the `<head>` element as a module and replaces the invalid element with HTML5 valid render content. 

###### Example 
```html5
<!doctype html>
<html lang="en">
<head type="default"/>
<body>
  <script src="js/scripts.js"></script>
</body>
</html>
```

##### Elements
LHTML5 modules allow for the language to also make use of custom elements that are not defined in the HTML5 spec. When an LHTML5 document is build for site visitor it SHOULD adhere to HTML5 standards without exception. The following is an example of an unparsed LHTML5 document, which is
clearly unsuitable to send to a visitor's web browser without having been passed through an LHTML5 parser. 

###### Example
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

This example makes use of four Modules. These modules are instantiated using the `<html>`, `<block>`, `<news>` and `<footer>` elements. The `<html>` element invokes a module that adds the completed `<head>` element. The `<block>` element is replaced by a fully built HTML5 navigation bar. The `<h1>` element is part of the static page content; itt remains uninitiated and unaltered. The `<news>` element pulls up to 20 news stories from a database and display them with a `<div>` containing thumbnails and a headline. The `<footer>` section is automatically populated with a copyright notice. 

### Parsing
#### Builders
The same LHTML5 document may be built different ways depending on the specified builder. 

#### Configuration

#### Construction
The parser's config SHOULD be responsible for determining which modules to instantiate. This config MUST 
describe each module using the following fields:

| Field | Summary |
| --- | ---| 
| `name` | Machine readable identifier for the module. |
| `xpath` | Find elements within the document using an XPath expression. |
| `class_name` | Determines what class to instantiate the module as. Maybe either a custom element or a native HTML5 element. |

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

 
##### XPath Expression 
A Module is instantiated as object when elements are found within the document using XPath expressions. Only Modules defined in the parser config that are found within the document are turned to objects. 

| Type | Example | Description |
| --- | --- | --- |
| Native | `<head/>`  | A module instantiated using an preexisting HTML5 element. This is often the case when the element exist within the page but needs to be corrected or auto completed during parsing. | 
| Custom | `<block/>` | A module instantiated using a element not defined within the HTML5 spec. This is useful for namespacing custom elements that WYSIWYG users can drop into a page or when defining new content types. It's a way of adding a term to communicate a feature to web stakeholders. |

Elements not found MUST not be instantiated and SHOULD remain unaltered. 

##### Class Name
During parsing, a modules `class_name` determines what class the module is instantiated as. These class name
The type 

The Module's `class_name` may use the element's attributes as variables to resolve the class. Depending on the config, the following may show an example of a module that is instantiated the either the class `Modules/Block/Test` or `Modules/Block`.

Classes not found should be marked with one of the following error handlers
<~-- NOT FOUND -->
<~-- NOT FOUND -->

## `args`
During runtime the parser takes specified elements and instantiates them as objects. The element can feature arguments that are passed to the Module as properties. An argument's purpose is to be passed as a parameter, used by a module's method.

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
+ Modules can access their own private variables. 
+ Modules can access their ancestors public variables.

```HTML
 <div id="1">
 	<div id="2">
 		<div id="3"/>
 	</div>
 	<div id="4">
 		<div id="5"/>
 	</div>
 </div>
```
If `div` were a module in the above example, the following would be true:
* Module with id #1 can access no other Module public properties. 
* Module with id #2 can access module #1 public properties.
* Module with id #3 can access modules #1 and #2 public properties.
* Module with id #4 can access module #1 public properties.
* Module with id #5 can access modules #4 and #1 public properties.

## Module Methods
The config defines method calls to be orchestrated against all the modules instantiated. The methods can differ project to project but it stands to reason that one of the last ones will render the output from the module and its content will replace the original element entirely.

## Well-Formatted
LHTML5 is a well-formatted markup scripting language. It is RECOMMEND to close tags that are opened. 

## Customizable Tags
In LHTML5 any tag can be used. And pre-exist HTML5 tags can be enhanced or transformed. For example, with LHTML5 for accessibility compliance, the alt attribute could be set to decorator when missing from img elements. LHTML5 discourages large nested logical elements that can be hard to read. The design encourages the creation of Module rather than building a 10 layer deep statement of conditions. 