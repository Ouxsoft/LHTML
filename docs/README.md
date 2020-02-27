# LHTML5

[![Documentation Status](https://readthedocs.org/projects/lhtml5/badge/?version=latest)](https://lhtml5.readthedocs.io/en/latest/?badge=latest)

February 2020 Edition

## Introduction
Living Hypertext Markup Language 5 (LHTML5) was adapted, over the subsequent years, to describe a standard for web servers to store and build flexible dynamic HTML5 documents. HTML is a standard for describing content for the web browser. LHTML is a standard for communicating pages between all web stakeholder. The language is intentionally considerate communication between:

 + Backend developer;
 + Template designers;
 + Search indexes;
 + Frontend developers;
 + UX/UI designers;
 + WYSIWYG users; and
 + Web browser.
 
LHTML5 uses a syntax that is similar to HTML5; it's as if the elements and attributes you wished HTML5 had were there. This help keep it simple as there is no need to learn another syntax. Rather than becoming an obstacle. This syntax layer serves as instruction for the LHTML5 parser. Mainly, they are used to instantiate modules which replace their origin with rendered content.

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

The keywords "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY ", and "OPTIONAL" will be used as defined in RFC 2119 [RFC2119](https://www.w3.org/TR/2004/WD-qaframe-spec-20040830/#RFC2119). When used with the normative RFC2119 meanings, they will be all uppercase. Occurrences of these words in lowercase comprise normal prose usage, with no normative implications.

## Language

### HTML5 Spec Inheritance
Except for providing instruction for LHTML5 modules, a LHTML5 document MUST adhere to HTML5 standards. The [HTML5 spec](https://html.spec.whatwg.org/multipage/) defines how static markup documents are sent from the web server to the browser. After a LHTML5 document is parsed it MUST adhere to the HTML5 standards without exception.

### LHTML5 Module Instructions

LHTML5 can on the surface look like an oversimplified and somewhat embellished HTML5 document. 

The over simplicity stems from 

LHTML5 modules being able to alter the document and automate redundant elements. 

The embellished looks come from custom elements that are used to instantiate modules. 


### Example Document

A basic LHTML5 unparsed document looks like this:

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

LHTML5 documents consist of tree elements containing attributes and inner text. The above example makes use of four Modules that are instantiated using the `<html>`, `<block>`, `<news>` and `<footer>` elements. The `<html>` element invokes a module that adds a `<head>` tag. The `<block>` tag is replaced by a HTML5 navigation bar. The `<h1>` element remains uninitiated and unaltered. The `<news>` element pulls up to 20 news stories and display them with a `<div>` containing thumbnails and a headline. The `<footer>` section is automatically populated with a copyright notice. 

### Modules
Modules are the worker bees of LHTML. During document parsing Modules are instantiated as object when elements are found using `xpath` expressions. Not all elements are parsed. Only Modules defined in the parser config are turned to objects. The rest remain unaltered. 

#### Native Element
A module can be instantiated using an existing HTML5 element. This is often the case when the element exist within the page but needs to be corrected or auto completed during parsing to a stakeholder.

```html5
<head/>
``` 

#### LHTML5 Elements
A module can be instantiated using a element that does not exist within the HTML5 spec. This is useful for namespacing elements that WYSWYG users can drop into a page or defining new content. It's a way of adding a term to communicate a feature to web stakeholders. 
 
```html5
<block/>
```

#### Construction
The parser's config defines the `xpath` expression and `class_name` used to find elements and instantiate them as modules. That `class_name` may useh the element's attributes as variables to resolve the class. Depending on the config, the following may show an example of a module that is instantiated as the either the class `Modules/Block/Test` or `Modules/Block`.

```html5
<block name="Test"/>
```

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
LHTML5 is a well-formatted markup scripting language; Coldfusion is not. In ColdFusion items like <cfelse> in <cfif><cfelse></cfif> are not open and closed meaning it is not well-formatted. 

## Customizable Tags
The LHTML5 design encourages the create of Module, such as navbar, rather than building a 10 layer deep statement of conditions. Unlikely ColdFusion, LHTML5 relies less on large nested objects that can be hard to read.

Coldfusion limits developers to a set of prebuilt tags. In LHTML5 any tag can be used. And pre-exist HTML5 tags can be enhanced or transformed. For example, with LHTML5 for accessibility compliance, the alt attribute could be set to decorator when missing from img elements.