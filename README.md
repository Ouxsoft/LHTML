# LHTML5 Spec

March 2020 Edition

## Introduction
Living HTML5 (LHTML5) turns markup into orchestatable objects that perform logic and collaborate to make even better markup.

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

The HTML5 standard is not a dynamic markup standard. It cannot by itself generate dynamic HTML5. Therefore, engineers commonly embed a templating language, which is ignorant of the HTML5, within the HTML5 document. This language must be processed by a templating engine, which searches for its own syntax and then combines it with the data model. Because the templating language is ignorant of the HTML5, is not particularly well-suite. 

The root cause appears to originate from a language, which in many ways HTML5 stems from, call SGML (Standard Generalized Markup Language). For in 1986 when this standard was accepted, it was based on two postulates (or assumptions):
+ Declarative: Markup should describe a document's structure and other attributes rather than specify the processing that needs to be performed, because it is less likely to conflict with future developments.
+ Rigorous: In order to allow markup to take advantage of the techniques available for processing rigorously defined objects like programs and databases.

The LHTML5 standard questions the first axiom and prefers to use the following:
+ Declarative: Markup SHOULD describe a document's structure and other attributes. It does not perform processing, because of separation of concerns, but is processed. It may contain simple instructions for the processor. The processor decides if and how to interpret these instructions and whether to remove or replace with rendered content.

The LHTML5 standard contains both the document language and processing standards. The document language defines the standard for a LHTML5 document (referred to as a "document"). Its syntax is similar to HTML5 but permits additional custom elements and attributes that are used to inform the processor. Unlike HTML5, which describes content for the web browser, LHMTL5 allows internal stakeholders to add instructions that generates dynamic pages. The processor defines how to use the document language to build dynamic HTML5. A document is past into a processor (also referred to as "program", "interpreter", "parser", etc.) that is responsible for building the HTML5. This standard focuses primarily on how configured elements and attributes serve as instructions to instantiate modules, perform coordinated logical functions, and replace themselves with rendered content. 

## Purpose
LHTML5 exists to help empower web development teams and encourage effective communication. When fully impletmented it allows complex features to be extended to non tech savvy indivudals.   

It can aid in one time markup conversions including CSS framework upgrades.

## Document Language
The document MUST consist of tree elements that contain attributes. It is RECOMMENDED that it be well-formatted markup. It is RECOMMENDED that the document feature a root element (i.e. `<html>`). It is RECOMMEND that all tags that are opened be closed. 

All of the markup contained within the document MUST adhere to either the static markup or the dynamic markup standards.

### Static Markup
The document may contain static markup. Static markup is markup that the processor does not receive instructions from. Static markup MUST adhere to the [HTML5 spec](https://html.spec.whatwg.org/multipage/), which details how modern markup documents are delivered to the browser. Static markup is most often page specific markup that makes sense to manage within the document because it doesn't appear elsewhere. The paragraph text of a page is a good example of content that often makes sense to remain as static markup. 

### Dynamic Markup
Dynamic markup is the powerhouse of the document. Dynamic markup is markup that the processor MUST be capable of processing and rendering during runtime. This type of markup, which serve as instructions for the processor, is entirely optional. It include the presence of custom attributes, custom elements, and argument elements. The custom attributes and custom elements may or may not be defined in the HTML5 spec. Arguments elements are not within the HTML5 spec.

These elements and attributes act as the blueprints for dynamic content. The tags serve as placeholders for that dynamic content and once processed will be replaced with valid HTML5. The document may make use of any tags as dynamic markup. Even pre-exist HTML5 tags can be enhanced or transformed. For example, with LHTML5 for accessibility compliance, the alt attribute could be set to "decorator" when missing from `img` elements or srcset attributes could be automatically generated. 

The processor's builders generally replaces dynamic elements with rendered dynamic content that was not previously within the element.
 
### Custom Attributes
The document language MAY use custom attributes not defined in the HTML5 spec. These elements SHOULD serve as instructions for the processor. 

#### Example 
The following example show an invalid HTML5 attribute, "type", within the `<head>` element could be used to maintain the element's inner content. When this document is passed to an LHTML5 processor (along with the proper config and modules) the processor will instantiate the `<head>` element as a module, perform logic, remove the invalid attribute, and replace the inner HTML5 with valid render content. 

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
The document MAY use custom elements not defined in the HTML5 spec. LHTML5 design encourages the creation of custom elements over building a processing tree consisting of large nested logical elements, which are difficult to maintain. 

#### Example
The following shows an example of an unparsed document that would not be suitable to send to a site visitor's web browser unprocessed. This example contains two custom elements and makes use of four Modules. The Modules are instantiated using the `<html>`, `<block>`, `<news>` and `<footer>` elements and accomplish the following:
 + The `<html>` element adds a completed `<head>` element when detected as missing. 
 + The `<block>` element is replaced by a fully built HTML5 navigation bar. 
 + The `<news>` element pulls up to 20 news stories from a database and display them with a `<div>` containing thumbnails and a headline. 
 + The `<footer>` element is populated with a copyright notice.

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
Notice the `<h1>` element is part of the static page content; it remains uninitiated and unaltered.

### Arguments 
During runtime the processor finds specified elements and instantiates them as objects (Modules). An element MAY feature arguments. These arguments are made available and accessible to Module as properties. The arguments MAY be defined using the element's attributes or child `<arg>` elements.

#### Arguments from Derived Attributes
Arguments can be added as attributes within a dynamic element. The processor MUST pass all dynamic element's attributes as arguments. Storing and passing arguments as element attributes has its limits, as too much content can decrease readability. 

#### Example
In the following example, `block` declares three arguments using attributes.
+ `name` set to the value "Test"; 
+ `min` set to a value of 0; and 
+ `limit` set to a value of 1. 

```lhtml5
<block name="Test" min="0" limit="1"/>
```

#### Argument Elements
Arguments can be declared using a children `<arg/>` elements. When using this method the `<arg>` name attribute specifies the argument's name and the inner contents specify the value.

#### Example
In the following example, `block` features three arguments, two of which are declared using argument elements.
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
A processor implementation may be written in a language that makes use of strict types. For standardization, it is RECOMMENDED to define these data types within the `<arg>` element's "type" attribute. The various types values available are not defined within this standard because they may vary depending on the language. 
 
##### Example

```lhtml5
<block name="Test">
    <arg name="id" type="int">0</arg>
    <arg name="msg" type="string">Hello</arg>
    <arg name="metadata" type="json">{"content":"50"}</arg>
</block>
```

##### Passing Array Arguments
An argument array can be made using multiple similarly arg elements with the same name. The following arg `type` contains an array with three values: 0 => pickles, 1 => ketchup, and 2 => mustard.
```lhtml5
<block name="Test">
    <arg name="type">pickles</arg>
    <arg name="type">ketchup</arg>
    <arg name="type">mustard</arg>
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
LHTML5 is a modular language for emergent purposes. How a document is process is determined by both the builder, modules, and configuration.

#### Builders
The same LHTML5 document may be built different ways depending on the specified builder. The builders may handle configurations differently. A processor may feature multiple builders, e.g. 
+ A WYSIWYG builder;
+ A search engine index builder;
+ A web browser builder.

When an LHTML5 document is build for site visitor it SHOULD adhere to HTML5 standards without exception. 

#### Configuration
The configuration contain the project's settings that are passed to the one of the processor's builder. The configurations are stored in a config, which is often a standalone file. 

#### Example
The following example demonstrates one potential YAML config.
```yaml
modules:
  types:
    - name: 'Block'
      class_name: 'LivingMarkup\Modules\Blocks\{name}'
      xpath: '//block'
      settings:
        - cache_duration: '1 hour'
        - search_index: false
    - name: 'Partial'
      class_name: 'LivingMarkup\Modules\Partials\{name}'
      xpath: '//partial'
    - name: 'Image'
      xpath: '//img'
      class_name: 'LivingMarkup\Modules\Image'
    - name: 'Hyperlink'
      xpath: '//a'
      class_name: 'LivingMarkup\Modules\Hyperlink'
    - name: 'Variable'
      xpath: '//var'
      class_name: 'LivingMarkup\Modules\Variable'
    - name: 'If Statement'
      xpath: '//if'
      class_name: 'LivingMarkup\Modules\IfStatement'
    - name: 'Redact'
      xpath: '//redact'
      class_name: 'LivingMarkup\Modules\Redact'
  methods:
    - name: 'beforeLoad'
      descirption: 'Execute before object data load'
    - name: 'onLoad'
      descirption: 'Execute during object data load'
    - name: 'afterLoad'
      description: 'Execute after object data loaded'
    - name: 'beforeRender'
      description: 'Execute before object render'
    - name: 'onRender'
      description: 'Execute during object render'
      execute: 'RETURN_CALL'
    - name: 'afterRender'
      descirption: 'Execute after object rendered'
```

##### Module Types
The processor's config SHOULD be responsible for determining how to instantiate Modules. A valid config MUST be capable of providing instruction to remove all elements and attributes not permitted by the HTML5 spec. This config MUST describe each module using the following fields:

| Field | Summary |
| --- | ---| 
| `name` | Machine readable identifier for the module. |
| `xpath` | Find elements within the document using an XPath expression. |
| `class_name` | Determines what class to instantiate the module as. |
 
##### `xpath:`
The xpath withing the config is used to find elements to instantiate as Module. Only Modules defined in the processor config that are also found within the document should be instantiated.  

| Type | Example | Description |
| --- | --- | --- |
| Native | `<head/>`  | A native module is instantiated using an preexisting HTML5 element. This is often the case when the element exist within the page but needs to be corrected or auto completed during parsing. | 
| Custom | `<block/>` | A custom module is instantiated using a element not defined within the HTML5 spec. This is useful for namespacing custom elements that WYSIWYG users can drop into a page or when defining new content types. It's also a way of adding a term to communicate a feature to internal web stakeholders. |

The processor should ignore any elements not found using an xpath from the config. These elements SHOULD not bt instantiated and SHOULD remain unaltered. 

##### `class_name:`
The same element can be instantiated different ways. During parsing, a module's `class_name` determines what class the module is instantiated as. The Module's `class_name` may use the element's attributes as variables to resolve the class.

Depending on the configuration, if a the process cannot find a classes it MAY replace the elment with a HTML5 comment indicating an error has occurred. 

###### Example
The config determines the class that a module is instantiated as. A config with a different class_name declared will instantiate the same module differently. Take the following for example:

```lhtml5
<block name="Test"/>
```

| class_name | Module Class Instantiated | 
| --- | --- |
| "Modules/Block/{name}" | `Modules/Block/Test` |
| "Modules/Block" | `Modules/Block` | 
| "TestElement" | `TestElement` |

#### Modules
Modules are the worker bees of LHTML5. They are objects that complete logic to modify the document. They receive instruction from the documents args and the configuration. Potential uses of LHTML5 modules include, but are not limited to:

+ reduce architectural debt and clutter of redundant elements present across multiple pages;
+ extend backend features to frontend WYSIWYG users;
+ transform existing elements to ensure compliance;
+ increase productivity by allowing for HTML5 short hand;
+ separate complex page logic from the template;
+ embed simple conditional logic;
+ enable the use of dynamic content (such as variables); and
+ custom elements that are used to instantiate modules.

##### Nested Modules
A dynamic element can be nested inside one another dynamic element. Therefore a module can be nested inside another module.
 
###### Example
The following shows an example of a `var` (short for variable) module nested inside a `block` module.
```html5
<block name="UserProfile">
    <var name="fist_name"/>
</block>
```

#### Module Ancestor Properties
All Modules MUST be able to access their own private variables. In addition, all Modules can access their ancestors element's public variables. 

##### Example
The following example demonstrates module's ability to access their ancestor elements.
* ModuleA can access no other modules public properties. 
* ModuleB can access ModuleA's public properties.
* ModuleC can access ModuleA's and ModuleB's public properties.
* ModuleD can access ModuleA's public properties.
* ModuleE can access ModuleA's and ModuleD's public properties.

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

##### Module Methods
The config defines method calls to be orchestrated against all the modules instantiated. These methods MAY differ project to project. It stands to reason that one of the last methods called will be to render the Module's output and its content will replace the original element entirely. 

Method calls to instantiated objects MAY occur in either forward or reverse order depending on the orchestration needs. 

###### Example
The following examples demonstrate the two different orders, forward and reverse, in which Module methods can be called. The processor SHOULD make render calls (e.g. onRender) in reverse. In this example, the order of module method calls are indicated by process_id attribute.

Forward - Recursive iterate forward. 
```lhtml5
<block process_id="1">
	<block process_id="2">
		<block process_id="3"/>
	</block>
	<block process_id="4">
		<block process_id="5"/>
		<b>Test</b>
	</block>
	<a href="#test">Test</a>
</block>

```

Reverse - Recursive reverse iterate. 
```lhtml5
<block process_id="5">
	<block process_id="4">
		<block process_id="3"/>
	</block>
	<block process_id="2">
		<block process_id="1"/>
		<b>Test</b>
	</block>
	<a href="#test">Test</a>
</block>
```

## Implementations
+ [LivingMarkup](https://github.com/hxtree/LivingMarkup) a PHP Implementation

## Reporting issues

Please report issue and open new tickets:

+ [Report Issues](https://github.com/hxtree/lhtml5/issues)

## Contributing

To contribute sign into your Github account, navigate to the [README.md](https://github.com/hxtree/lhtml5/blob/master/README.md), click the [edit button](https://github.com/hxtree/lhtml5/edit/master/README.md), and commit your changes.

## Acknowledgments

Thanks to Kseniya Gorbunova for their useful comments that have led to changes to this specification.
