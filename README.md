# LHTML Standard
A markup abstraction layer.

Version 2.0.0

## Introduction
***L***iving ***HTML*** (LHTML) is a customizable markup-based templating engine standard. 

In short, the standard defines the following process procedures: 
1. The process receives a string that it turns into a Document Object Model (DOM).
2. Using a configuration it searches the DOM with a Xpath query and instantiates matches as specified classes. 
3. Next, it iterates calls to these object using a list of methods. 
4. Finally, it returns a resulting string containing the altered markup.

Anyone familiar with HTML5 should find LHTML a breeze. Let's dive right in! 

### Copyright notice
Copyright (c) 2017-present Ouxsoft

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

### Conformance
A conforming implementation of LHTML must fulfill all normative requirements. Conformance requirements are described in this document via both descriptive assertions and key words with clearly defined meanings.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY ", and "OPTIONAL" will be used as defined in [RFC2119](https://www.ietf.org/rfc/rfc2119.txt). When used with the normative RFC2119 meanings, they will be all uppercase. Occurrences of these words in lowercase comprise normal prose usage, with no normative implications.

## Preamble
HTML5 is the latest evolution of the standard markup language for the World Wide Web. Unfortunately, HTML5 has inherited many flaws that are hindering modern web development teams. The weaknesses most concerning are discussed herein.

Perhaps the largest obstacle is that HTML5 exists to organize static information for presentation. It is not designed to consider team collaboration, maintainability of a code base, or continuous improvement. It is a machine-centric language -- written for a machine to send a message to a machine -- that some people know how to write.

The language is strict and only approved tags are permitted, dampening the ability to extend features and empower developers.  HTML5 is inadequate for communicating web design between development teams that don’t talk using one of the predefined elements. The syntax says too little with too much input. 

Portions of code bases using HTML5 are difficult to maintain because it is full of repetition. There is no consideration for the DRY (don't repeat yourself) design principle. A simple update in a CSS framework often results in a massive website rewrite.

Engineers are forced to adapt a templating language because HTML5 cannot generate dynamic content by itself. This language is then processed by a templating engine, which searches for its templating syntax and then combines it with the data model. Even though the templating language must exist inside the HTML5 document, it remains ignorant of the preexisting markup. The templating language cannot effectively make improvements to the markup or allow for ease in maintenance due to this ignorance.

Why did developers decide the assumptions in this paradigm work best? The root cause appears to originate from the ancestor of HTML5, known as SGML (Standard Generalized Markup Language). In 1986, when SGML was accepted, it was based on two postulates (or assumptions):

+ Declarative: Markup should describe document structure and other attributes rather than specify the processing that needs to be performed because it is less likely to conflict with future developments.
+ Rigorous: In order to allow markup to take advantage of the techniques available for processing, it requires rigorously defined objects like programs and databases.

The LHTML standard questions the declarative axiom and replaces it as follows:

+ Declarative: Markup SHOULD describe document structure and other attributes. Due to separation of concerns, it does not perform processing. However, it is processed and may contain simple instructions for the processor. It is up to the processor to decide if and how to interpret these instructions, and whether or not to remove or replace them with rendered content.

A standard that aims to allow for markup to be an emergent means of web communication is needed.

## Purpose
LHTML goal is to define an emergent markup language standard that can communicate web design. LHTML aims to empower internal web stakeholders and encourage effective communication. This standard is released separate from any implementation to allow the underlying technology to be swapped out.

Using LHTML enables web stakeholders to describe a document in an emergent way that works for them both now and into the future. Developers can make their HTML5 templates easier to maintain with LHTML. Complex elements can be maintained in a single location. Existing elements can be enhanced. LHTML empowers non tech savvy individuals to use complex features through simple APIs.
It makes it easier to perform CSS framework upgrades within code bases.

## Document Language
The LHTML standard contains both the document language and processing standards. It is the goal of the document language to communicate web design between web stakeholders. The document language defines the standard for an LHTML document (referred to as a "document"). 

The document language's syntax is similar to HTML5 but permits additional custom elements and attributes that are used to inform the processor. Unlike HTML5, which describes content for the web browser, LHTML allows internal stakeholders to add instructions to generate dynamic pages. 

The document MUST consist of tree elements that contain attributes. It is RECOMMENDED that it be well-formatted markup. It is RECOMMENDED that the document feature a root element (i.e. `<html>`). It is RECOMMENDED that all tags that are opened be closed. 

All of the markup contained within the document MUST adhere to either the static markup or the dynamic markup standards below.

### Static Markup
The document may contain static markup. Static markup is defined as markup that the processor does not receive instructions from. Static markup is often page specific content. A page's unique paragraph text is a often a good example of static markup. It makes sense to manage this content within the document because it doesn't appear elsewhere. 

Static markup MUST adhere to the [HTML5 spec](https://html.spec.whatwg.org/multipage/), which details how modern markup documents are delivered to the browser. 

### Dynamic Markup
Dynamic markup is the powerhouse of the document. It is markup that the processor MUST be capable of processing and rendering at runtime. This type of markup is entirely optional. 

Dynamic markup includes the presence of custom attributes, custom elements, and argument elements. These custom attributes and custom elements may or may not be defined in the HTML5 spec. Argument elements are not within the HTML5 spec.

These elements and attributes act as the blueprint for dynamic content. They provide instructions for the processor and act as placeholders. A processor MAY replace dynamic elements with rendered dynamic content.

The document MAY use any tags for dynamic markup. Even pre-existing HTML5 tags can be enhanced or transformed. For example:

+ the `alt` attribute could be set to "decorator" when missing from `img` elements for accessibility compliance; or
+ `srcset` attributes could be automatically generated. 
 
### Custom Attributes
The document language MAY use custom attributes not defined in the HTML5 spec. These elements SHOULD serve as instructions for the processor. 

#### Example 
The following example shows an invalid HTML5 attribute, "type", within the `<head>` element could be used to maintain the element's inner content. When this document is passed to an LHTML processor (along with the proper config and elements), the processor will instantiate the `<head>` element as a element, perform logic, remove the invalid attribute, and replace the inner HTML5 with valid render content. 

```lhtml
<!doctype html>
<html lang="en">
<head type="default"/>
<body>
  <script src="js/scripts.js"></script>
</body>
</html>
```

### Custom Elements
The document MAY use custom elements not defined in the HTML5 spec. LHTML design encourages the creation of custom elements over building a processing tree consisting of large nested logical elements, which are difficult to maintain. 

#### Example
The following is an example of an unparsed document that would not be suitable to send to a site visitor's web browser unprocessed. This example contains two custom elements and makes use of four Elements. The Elements are instantiated using the `<html>`, `<block>`, `<news>` and `<footer>` elements and accomplish the following:
 + The `<html>` element adds a completed `<head>` element when detected as missing. 
 + The `<block>` element is replaced by a fully built HTML5 navigation bar. 
 + The `<news>` element pulls up to 20 news stories from a database and display them with a `<div>` containing thumbnails and a headline. 
 + The `<footer>` element is populated with a copyright notice.

```lhtml
<html>
    <block name="NavBar" style="light"/>
    <h1>News &amp; Events</h1>
    <news>
        <arg name="limit">20</arg>
        <arg name="style">thumbnail</arg>
    </news>
    <footer/>
</html>
```
Notice the `<h1>` element is part of the static page content; it remains uninitiated and unaltered.

### Arguments 
During runtime the processor finds specified elements and instantiates them as objects (Elements). An element MAY feature arguments. These arguments are made available and accessible to the Element as properties. The arguments MAY be defined using the element's attributes or child `<arg>` elements.

#### Arguments from Derived Attributes
Arguments can be added as attributes within a dynamic element. The processor MUST pass all dynamic element's attributes as arguments. Storing and passing arguments as element attributes has its limits, as too much content can decrease readability. 

#### Example
In the following example, `block` declares three arguments using attributes.
+ `name` set to the value "Test"; 
+ `min` set to a value of 0; and 
+ `limit` set to a value of 1. 

```lhtml
<block name="Test" min="0" limit="1"/>
```

#### Argument Elements
Arguments can be declared using a children `<arg/>` elements. When using this method the `<arg>` name attribute specifies the argument's name and the inner contents specify the value.

#### Example
In the following example, `block` features three arguments, two of which are declared using argument elements.
+ `name` set to the value "Test"; 
+ `min` set to a value of 0; and 
+ `limit` set to a value of 1. 

```lhtml
<block name="Test">
    <arg name="min">0</arg>
    <arg name="limit">1</arg>
</block>
```

#### Argument Type Definition
A processor implementation may be written in a language that makes use of strict types. For standardization, it is RECOMMENDED to define these data types within the `<arg>` element's "type" attribute. The various type values available are not defined within this standard because they may vary depending on the language. 
 
##### Example

```lhtml
<partial name="UserMessage">
    <arg name="id" type="int">0</arg>
    <arg name="msg" type="string">Hello</arg>
    <arg name="metadata" type="json">{"content":"50"}</arg>
</partial>
```

##### Passing Array Arguments
An argument array can be made using multiple similar arg elements with the same name. The following arg `type` contains an array with three values: 0 => pickles, 1 => ketchup, and 2 => mustard.
```lhtml
<widget name="Sandwich">
    <arg name="type">pickles</arg>
    <arg name="type">ketchup</arg>
    <arg name="type">mustard</arg>
</widget>
```

### Dialect
The presence of custom elements and attributes in turn alters and shapes the project's dialect. That is why it is important to thoughtfully design these language changes. A decisive factor in success of a dialect (and thus the project's success) is its effectiveness to serve as a message to communicate between stakeholders. A dialect's design is RECOMMENDED to carry a message that allows project stakeholders to effectively communicate. These stakeholders MAY include any of the following:

+ Backend developer;
+ Template designers;
+ Frontend developers;
+ UX/UI designers;
+ WYSIWYG users.

The present of these elements allows internal stakeholders to communicate design. Elements and arguments can be passed from front end developers to backend end developers 

### Processor Standards
LHTML is a modular language for emergent purposes. The processor defines how to use the document language to build dynamic HTML5. A document is past into a processor (also referred to as "program", "interpreter", "parser", etc.) that is responsible for building the HTML5. This standard focuses primarily on how configured elements and attributes serve as instructions to instantiate elements, perform coordinated logical functions, and replace themselves with rendered content. 

How a document is process is determined by both the builder, elements, and configuration. 

#### Builders
The same LHTML document may be built in different ways depending on the specified builder. The builders may handle configurations differently. A processor may feature multiple builders, e.g. 
+ A WYSIWYG builder;
+ A search engine index builder;
+ A web browser builder.

When an LHTML document is built for the site visitor it SHOULD adhere to HTML5 standards without exception. 

#### Configuration
The configuration contains the project's settings that are passed to one of the processor's builders. The configurations are stored in a config, which is often a stand-alone file. 

#### Example
The following example demonstrates one potential JSON config.
```json   
{
  "version": 3,
  "elements": [
    {
      "xpath": "//bitwise",
      "class_name": "LivingMarkup\\Test\\Bitwise"
    }
  ],
  "routines": [
    {
      "method": "beforeLoad",
      "description": "Execute before object data is loaded"
    },
    {
      "method": "onLoad",
      "description": "Execute when object data is loading"
    },
    {
      "method": "afterLoad",
      "description": "Execute after object data is loaded"
    },
    {
      "method": "beforeRender",
      "description": "Execute before object is rendered"
    },
    {
      "method": "onRender",
      "description": "Execute while object is rendering",
      "execute": "RETURN_CALL"
    },
    {
      "method": "afterRender",
      "description": "Execute after object is rendered"
    }
  ]
}
```

##### Element Types
The processor's config SHOULD be responsible for determining how to instantiate Elements. A valid config MUST be capable of providing instruction to remove all elements and attributes not permitted by the HTML5 spec. This config MUST describe each element using the following fields:

| Field | Summary |
| --- | ---| 
| `name` | Machine readable identifier for the element. |
| `xpath` | Find elements within the document using an XPath expression. |
| `class_name` | Determines what class to instantiate the element. |
 
##### `xpath:`
The xpath within the config is used to find elements to instantiate as Element. Only Elements defined in the processor config that are also found within the document should be instantiated.  

| Type | Example | Description |
| --- | --- | --- |
| Native | `<head/>`  | A native element is instantiated using an preexisting HTML5 element. This is often the case when the element exist within the page but needs to be corrected or auto completed during parsing. | 
| Custom | `<block/>` | A custom element is instantiated using a element not defined within the HTML5 spec. This is useful for namespacing custom elements that WYSIWYG users can drop into a page or when defining new content types. It is also a way of adding a term to communicate a feature to internal web stakeholders. |

The processor should ignore any elements not found using an XPath from the config. These elements SHOULD not be instantiated and SHOULD remain unaltered. 

##### `class_name:`
The same element can be instantiated different ways. During parsing, a element's `class_name` determines what class the element is instantiated as. The Element's `class_name` may use the element's attributes as variables to resolve the class.

Depending on the configuration, if the process cannot find classes it MAY replace the elment with a HTML5 comment indicating an error has occurred. 

###### Example
The config determines the class that a element is instantiated as. A config with a different class_name declared will instantiate the same element differently. Take the following for example:

```lhtml
<block name="Test"/>
```

| class_name | Element Class Instantiated | 
| --- | --- |
| "Elements/Block/{name}" | `Elements/Block/Test` |
| "Elements/Block" | `Elements/Block` | 
| "TestElement" | `TestElement` |

#### Elements
Elements are the worker bees of LHTML. They are objects that complete logic to modify the document. They receive instruction from the documents args and the configuration. Potential uses of LHTML elements include, but are not limited to:

+ reduce architectural debt and clutter of redundant elements present across multiple pages;
+ extend backend features to frontend WYSIWYG users;
+ transform existing elements to ensure compliance;
+ increase productivity by allowing for HTML5 short hand;
+ separate complex page logic from the template;
+ embed simple conditional logic;
+ enable the use of dynamic content (such as variables); and
+ custom elements that are used to instantiate elements.

##### Nested Elements
A dynamic element can be nested inside another dynamic element. Therefore, a element can be nested inside another element.
 
###### Example
The following shows an example of a `var` (short for variable) element nested inside a `partial` element.
```lhtml
<partial name="UserProfile">
    <var name="fist_name"/>
</partial>
```

#### Element Ancestor Properties
All Elements MUST be able to access their own private variables. In addition, all Elements can access their ancestors element's public variables. 

##### Example
The following example demonstrates element's ability to access their ancestor elements.
* ElementA can access no other elements' public properties. 
* ElementB can access ElementA's public properties.
* ElementC can access ElementA's and ElementB's public properties.
* ElementD can access ElementA's public properties.
* ElementE can access ElementA's and ElementD's public properties.

```HTML
 <block name="ElementA">
 	<block name="ElementB">
 		<block name="ElementC"/>
 	</block>
 	<block name="ElementD">
 		<block name="ElementE">
 	</block>
 </block>
```

##### Routines
The config defines routines which are method calls to be orchestrated against all the elements instantiated. These methods MAY differ project to project. It stands to reason that one of the last methods called will be to render the Element's output and its content will replace the original element entirely. 

Method calls to instantiated objects MAY occur in either forward or reverse order depending on the orchestration needs. 

###### Example
The following examples demonstrate the two different orders, forward and reverse, in which Element methods can be called. The processor SHOULD make render calls (e.g. onRender) in reverse. In this example, the order of element method calls are indicated by process_id attribute.

Forward - Recursive iterate forward. 
```lhtml
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
```lhtml
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
+ [LivingMarkup](https://github.com/ouxsoft/LivingMarkup) is a PHP implementation.

## Reporting issues

Please report issues and open new tickets:

+ [Report Issues](https://github.com/ouxsoft/lhtml/issues)

## Contributing

To contribute, sign into your Github account, navigate to the [README.md](https://github.com/ouxsoft/lhtml/blob/master/README.md), click the [edit button](https://github.com/ouxsoft/lhtml/edit/master/README.md), and commit your changes.

## Acknowledgments

Thanks to Matthew Heroux for inventing LHTML, without it none of this would exist.

Special thanks to Kseniya Gorbunova, Kelly Heroux, and Michael Riley for their useful comments that have led to changes to this specification.
