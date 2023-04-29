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
