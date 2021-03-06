# node-js2xmlparser #

## Overview ##

js2xmlparser is a Node.js module that parses JavaScript objects into XML.

## Features ##

Since XML is a data-interchange format, js2xmlparser is designed primarily for JSON-type objects, arrays and primitive
data types, like many of the other JavaScript to XML parsers currently available for Node.js.

However, js2xmlparser is capable of parsing any object, including native JavaScript objects such as Date and RegExp, by
taking advantage of each object's toString function. Functions are a special case where the return value of the function
itself is used instead of the toString function, if available.

js2xmlparser also supports a number of constructs unique to XML:

* attributes (through a unique attribute property in objects)
* mixed content (through a unique value property in objects)
* multiple elements with the same name (through arrays)

js2xmlparser can also pretty-print the XML it outputs with the option of customizing the indent string.

## Installation ##

The easiest way to install js2xmlparser is to use npm: `npm install js2xmlparser`.

Alternatively, you may download the source from GitHub and copy it to a folder named "js2xmlparser" within your
"node_modules" directory.

## Usage ##

The js2xmlparser module contains one function which takes the following arguments:

* `root` - string containing the root element of the XML
* `data` - object or JSON string to be converted to XML
* `options` - object containing options (optional)
    * `declaration` - XML declaration options object (optional)
        * `include` - boolean representing whether an XML declaration is included (optional, default: true)
        * `encoding` - string representing the XML encoding for the corresponding attribute in the declaration; a value
          of null represents no encoding attribute (optional, default: "UTF-8")
    * `attributeString` - string containing the attribute property (optional, default: "@")
    * `valueString` - string containing the value property (optional, default: "#")
    * `prettyPrinting` - pretty-printing options object (optional)
        * `enabled` - boolean representing whether pretty-printing is enabled (optional, default: true)
        * `indentString` - string representing the indent (optional, default: "\t")

## Example ##

The following example illustrates the basic usage of js2xmlparser:

    var js2xmlparser = require("js2xmlparser");

    var data = {
        "firstName": "John",
        "lastName": "Smith"
    };

    console.log(js2xmlparser("person", data));

    > <?xml version="1.0" encoding="UTF-8"?>
    > <person>
    >     <firstName>John</firstName>
    >     <lastName>Smith</lastName>
    > </person>

Here's a more complex example that builds on the first:

    var js2xmlparser = require("js2xmlparser");

    var data = {
        "firstName": "John",
        "lastName": "Smith",
        "dateOfBirth": new Date(1964, 07, 26),
        "address": {
            "@": {
                "type": "home"
            },
            "streetAddress": "3212 22nd St",
            "city": "Chicago",
            "state": "Illinois",
            "zip": 10000
        },
        "phone": [
            {
                "@": {
                    "type": "home"
                },
                "#": "123-555-4567"
            },
            {
                "@": {
                    "type": "cell"
                },
                "#": "456-555-7890"
            }
        ],
        "email": function() {return "john@smith.com";}
    }

    console.log(js2xmlparser("person", data));

    > <?xml version="1.0" encoding="UTF-8"?>
    > <person>
    >     <firstName>John</firstName>
    >     <lastName>Smith</lastName>
    >     <dateOfBirth>Wed Aug 26 1964 00:00:00 GMT-0400 (Eastern Daylight Time)</dateOfBirth>
    >     <address type="home">
    >         <streetAddress>3212 22nd St</streetAddress>
    >         <city>Chicago</city>
    >         <state>Illinois</state>
    >         <zip>10000</zip>
    >     </address>
    >     <phone type="home">123-555-4567</phone>
    >     <phone type="cell">456-555-7890</phone>
    >     <email>john@smith.com</email>
    > </person>