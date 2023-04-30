# XML
XML, or Extensible Markup Language, is a technology introduced mainly for 
business data specification and integration. XML is a simplified descendant of 
SGML, or Standard Generalized Markup Language. For different types of business data you may need to define different 
concrete markup languages to define their data structures. Each of these concrete 
markup languages needs to follow the general syntax structure of XML but uses
a set of tag and attribute names predefined with XML syntax specification 
mechanisms like DTD (Data Type Definition) or XML Schema

## XML Document Example

    <?xml version="1.0" encoding="UTF-8"?>
    <!-- This XML document describes a DVD library -->
    <library>
      <dvd id="1">
        <title>Gone with the Wind</title>
        <format>Movie</format>
        <genre>Classic</ genre >
      </dvd>
      <dvd id="2">
        <title>Star Trek</title>
        <format>TV Series</format>
        <genre>Science fiction</genre>
      </dvd>
    </library>
   
## Using Special Characters (Entity References)

`&amp` for `&`

`&lt` for `<`

`&gt` for `>`

`&quot` for `"`

`&apos` for `'`

## DTD (Document Type Definition)

DTD (Document Type Definition) is the first mechanism for defining XML 
dialects. As a matter of fact, it is part of the XML specification v1.0 so all XML 
processors must support it.

### DTD example

    <?xml version="1.0" encoding="UTF-8"?>
    <!ELEMENT library (dvd+)>
    <!ELEMENT dvd (title, format, type)>
    <!ELEMENT title (#PCDATA)>
    <!ELEMENT format (#PCDATA)>
    <!ELEMENT genre (#PCDATA)>
    <!ATTLIST dvd id CDATA #REQUIRED>

### Declaring Elements

    <!ELEMENT elementName (elementContent)>
    
### Elements with data

    <!ELEMENT elementName (#CDATA)>
    <!ELEMENT elementName (#PCDATA)>
    <!ELEMENT elementName (ANY)>
    
`#CDATA` - The element contains character data that is not supposed to be parsed by a parser for nested elements.

`#PCDATA` - The element contains data that is going to be parsed by a parser for nested elements.

`ANY` - Declares an element with any content as its value.

### Elements with children (Sequence)

        <!ELEMENT elementName (childElementNames)> // Syntax
        <!ELEMENT dvd (title, format, type)>       // Example
        <!ELEMENT elementName (childName*)>        // Zero or more occurences
        <!ELEMENT elementName (childName?)>        // Zero or one occurence
        !ELEMENT section (section1 | section2)?    // Using pipe sym for alternative elements
        
### Declaring Attributes

In DTD, XML element attributes are declared with an ATTLIST declaration.

    <!ATTLIST elementName attributeName attributeType defaultValue>
    
![image](https://user-images.githubusercontent.com/33444140/235298043-26ed2c03-c133-414b-9baf-2b6da8cc1a41.png)

![image](https://user-images.githubusercontent.com/33444140/235298061-b5ba4775-1e1b-4a3f-ab24-999cdb75a922.png)

### DTD example :

    !ELEMENT circle EMPTY>
    <!ATTLIST circle radius CDATA "1">
    
## Declaring Entity names

An entity name can be declared as a nickname or shortcut for a character or a 
string. It is mainly used to represent special characters that must be specified with 
Unicode, or long strings that repeat multiple times in XML documents.

    <!ENTITY entityName "entityValue">  // Syntax
    <!ENTITY euro "&#8364;">            // Example 1
    <!ENTITY cs "Computer Science">     // Example 2

##  Associating DTD Declarations to XML Documents

To specify that an XML document is an instance of an XML dialect specified by 
a set of DTD declarations, it can be done in two ways:

### Method 1: DTD declarations are to be included in your XML document

    <!DOCTYPE rootElementTag [DTD-Declarations]>
    
### Example

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE library [
    <!ELEMENT library (dvd+)>
    <!ELEMENT dvd (title, format, genre)>
    <!ELEMENT title (#PCDATA)>
    <!ELEMENT format (#PCDATA)>
    <!ELEMENT genre (#PCDATA)>
    <!ATTLIST dvd id CDATA #REQUIRED>
    ]>
    <library>
        <dvd id="1">
            <title>Gone with the Wind</title>
            <format>Movie</format>
            <genre>Classic</genre>
        </dvd>
        <dvd id="2">
            <title>Star Trek</title>
            <format>TV Series</format>
            <genre>Science fiction</genre>
        </dvd>
    </library>
    
### Method 2: To link a DTD declaration file to an XML document

    <!DOCTYPE rootElementTag SYSTEM DTD-URL>
    
### Example :

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE library SYSTEM "dvd.dtd">
    <library>
        <dvd id="1">
            <title>Gone with the Wind</title>
            <format>Movie</format>
            <genre>Classic</genre>
        </dvd>
        <dvd id="2">
            <title>Star Trek</title>
            <format>TV Series</format>
            <genre>Science fiction</genre>
        </dvd>
    </library>
    
## XML Schema

While DTD is part of XML specification and supported by any XML processors, it is weak in its expressiveness for defining complex data structures. XML Schema is an alternative industry standard for defining XML dialects.

### Example :

    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema 
        xmlns:xs="http://www.w3.org/2001/XMLSchema">
        <xs:element name="library">
            <xs:complexType>
                <xs:sequence>
                     <xs:element name="dvd" minOccurs="0" maxOccurs="unbounded">
                        <xs:complexType>
                            <xs:sequence>
                                <xs:element name="title" type="xs:string"/>
                                <xs:element name="format" type="xs:string"/>
                                <xs:element name="genre" type="xs:string"/>
                            </xs:sequence>
                            <xs:attribute name="id" type="xs:integer" use="required"/>
                        </xs:complexType>
                      </xs:element>
                </xs:sequence>
            </xs:complexType>
          </xs:element>
    </xs:schema>
    
In the above example "xs" is an XML prefix representing namespace `http://www.w3.org/2001/XMLSchema` In an XML document, if an element is qualified by its namespace prefix, as “xs:element” for element `element` declared in namespace 
“http://www.w3.org/2001/XMLSchema”, its attributes and nested elements by default belong to the same namespace.

### A mixed complex type element can contain attributes, elements and text.

### Example:

        <xs:element name="letter">
            <xs:complexType mixed="true">
                <xs:sequence>
                    <xs:element name="name" type="xs:string"/>
                    <xs:element name="orderID" type="xs:positiveInteger"/>
                    <xs:element name="shipDate" type="xs:date"/>
                </xs:sequence>
            </xs:complexType>
        </xs:element>

## Referencing XML Schema Specification in an XML Document

### Without Target Namespace

    <rootTag
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:noNamespaceSchemaLocation="schemaFileURL"
    >
    
### With Namespace

Attribute - `schemaLocation`

    <rootTag
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="namaspaceString schemaFileURL"
    >

## XML Parsing and Validation with SAX and DOM

### SAX :

* SAX works as a pipeline. It reads in the input XML document sequentially, and fires events when it detects the start or end of language features like elements and attributes. 
* The application adopting a SAX parser needs to write an event handler class that has 
a processing method for each of the event types, and the methods are invoked by 
the SAX parser when corresponding types of events are fired.
* SAX is very efficient for some types of applications that don‟t need to search information 
backwards in an XML document

### DOM :

* DOM parser builds a complete tree data structure in the computer memory so it can be more convenient for detailed document analysis and language transformation. 
* Even though DOM parsers use more computer memory, it is the main type of XML parser that supports the Ajax technology.

Both SAX and DOM can work in validation mode.
