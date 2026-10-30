id:: 6ae1de1b-caa6-409f-a07f-44a5212c5e3d

- # YAML, JSON, Avro, and Protobuf
  - Google Gemini: "YAML JSON Avro Protobuf"
  - ## Overview
    - These four data serialization formats represent two distinct philosophies:
      - text-based formats designed for human readability and flexibility (YAML and JSON), and
      - binary formats optimized for raw performance, storage efficiency, and strict schemas (Avro and Protobuf).
  - ## Formal names
    - | Short name | Formal name |
      | ---- | ---- |
      | YAML | YAML Ain't Markup Language |
      | JSON | JavaScript Object Notation |
      | Avro | Apache Avro |
      | Protobuf | Protocol Buffers |
  - ## Comparison
    - | Feature | YAML | JSON | Avro | Protobuf |
      | ---- | ---- | ---- | ---- | ----|
      | Type | Text | Text | Binary | Binary |
      | Human Readable | Excellent | Moderate | No | No |
      | Schema Requirement | Optional | Optional (JSON Schema) | Required (JSON format) | Required (`.proto` file) |
      | Data Size on Wire | Large | Medium-Large | Very Small | Smallest |
      | Speed (Parse, Serialize) | Slow | Moderate | Flat | Fastest |
      | Schema Evolution | Manual | Manual | Automatic (Writer, Reader) | Tag-based (Backward, Forward) |
      | Primary Use Case | App Configuration | Web APIs, App Data | Big Data (Kafka, Hadoop) | Microservices (gRPC) |
  - ## Format Profiles
    - The Text Formats (Human-Centric)
      - | Format Profile | YAML | JSON |
        | ---- | ---- | ---- |
        | The Vibe | Minimalist and clean. It drops brackets and braces in favor of strict line indentation. | The universal language of the web. It uses key-value pairs wrapped in curly braces. |
        | Strengths | Incredibly easy for humans to read and write. Supports complex structures, comments, and references. | Ubiquitous support across virtually every programming language and browser tool. No pre-compilation or schemas required by default. |
        | Weaknesses | Indentation mistakes can break compilation. Parsing is relatively slow and resource-heavy. | Verbose. It repeats field names in every single record, inflating the payload size. It also does not natively support comments. |
        | Best For | Configuration files (e.g., Docker, Kubernetes, GitHub Actions) | Public web APIs, browser-to-server communication, and rapid prototyping. |
    - The Binary Formats (Machine-Centric)
      - | Format Profile | Avro | Protobuf |
        | ---- | ---- | ---- |
        | The Vibe | Dynamic binary serialization. The schema travels with the data or is fetched dynamically, meaning the binary payload contains no field names or tags—just pure data values. | Strict and blazing fast. You define data shapes in a `.proto` file, which is compiled into strongly typed code for your language of choice. |
        | Strengths | Brilliant at "schema evolution". A reader can process old data using a new schema seamlessly because Avro reconciles differences automatically. | Extreme performance. Instead of field names, it strips data down to minimal tag numbers (e.g., field `1`, field `2`), making payloads highly compact. |
        | Weaknesses | The binary payload is completely unusable unless you have the matching schema layout. | Requires a code generation compilation step whenever the data model changes. Hard to instantly debug without custom tooling. |
        | Best For | High-throughput streaming pipelines, big data storage, and analytics (Apache Kafka ecosystems). | High-performance, low-latency microservice architectures via gRPC. |
- # Schema Evolution
  - Google Gemini: "What is Schema Evolution?"
  - ## Overview
    - Schema evolution is the ability of a data system to handle changes to its data structure—like adding, deleting, or renaming fields—over time without breaking existing applications or corrupting historical data.
    - When your data model changes, you inevitably create two versions of reality:
      1. The Writer: The system producing the data (using a New or Old schema).
      2. The Reader: The system consuming the data (using an Old or New schema).
    - Schema evolution rules dictate how cleanly these two systems can talk to each other when their versions do not match.
  - ## The Three Types of Compatibility
    - To safely evolve a schema, your changes must fall into one or more of these compatibility categories:
      - Backward Compatibility:
        - A new schema can read old data.
        - Example: You add a new field to your database. Old records don't have this field, so your new code must assign it a default value to avoid crashing.
      - • Forward Compatibility:
        - An old schema can read new data.
        - Example: A microservice hasn't been updated yet, but it receives a new payload with an extra field. A forward-compatible system allows the old service to simply ignore the new field and process the rest normally.
      - Full Compatibility:
        - The schema is both backward and forward compatible. Old code can read new data, and new code can read old data.
  - ## How Different Formats Handle Evolution
    - Apache Avro (Dynamic Reconciliation)
      - Avro is widely considered the gold standard for schema evolution.
      - How it works: When a reader processes data, it looks at the schema the data was written with (writer schema) and compares it to its own schema (reader schema).
      - The magic: Avro automatically translates between the two on the fly. If a field was removed, Avro drops it. If a field was added, Avro fills it with the defined default value.
    - Protocol Buffers (Tag-Based)
      - Protobuf relies on numeric tags instead of field names (e.g., `optional string email = 3;`).
      - How it works: If you rename `email` to `user_email`, Protobuf doesn't care because the underlying binary data only sees tag `3`.
      - The catch: You can never change the tag number of an existing field, and you cannot reuse a deleted tag number.
    - JSON & YAML (Implicit / Manual)
      - Text formats do not natively enforce structural evolution rules.
      - How it works: If an API adds a field, JSON readers usually just ignore it (forward compatible). However, if an API removes a field that code relies on, the application will crash with a `NullPointerException` or undefined error unless you manually write defensive code to handle it.
- # YAML
  - [YAML 1.2.1 (2009-10-01)](https://yaml.org/spec/1.2.1/)
  - ## Abstract
    - “YAML Ain’t Markup Language” (abbreviated YAML) is a human-friendly, cross language, Unicode based data serialization language designed around the common native data types of agile programming languages. It is broadly useful for programming needs ranging from configuration files to Internet messaging to object persistence to data auditing.
  - ## Introduction
    - Relation to JavaScript Object Notation (JSON)
      - JSON’s foremost design goal is simplicity and universality. Thus, JSON is trivial to generate and parse, at the cost of reduced human readability. It also uses a lowest common denominator information model, ensuring any JSON data can be easily processed by every modern programming environment.
      - In contrast, YAML’s foremost design goals are human readability and support for serializing arbitrary native data structures. Thus, YAML allows for extremely readable files, but is more complex to generate and parse. In addition, YAML ventures beyond the lowest common denominator data types, requiring more complex processing when crossing between different programming environments.
    - Relation to the eXtensible Markup Language (XML)
      - XML was designed to be backwards compatible with the Standard Generalized Markup Language (SGML), which was designed to support structured documentation. XML therefore had many design constraints placed on it that YAML does not share. XML is a pioneer in many domains.
      - YAML is primarily a data serialization language and the result of lessons learned from XML and other technologies.
  - ## Block-style Collections
    - YAML’s block collections use indentation for scope and begin each entry on its own line.
    - Sequence of Scalars
      - Block sequences indicate each entry with a dash and space ( “- ”).
        - ```
          - Mark McGwire
          - Sammy Sosa
          - Ken Griffey
          ```
    - Mapping Scalars to Scalars
      - Mappings use a colon and space (“: ”) to mark each key: value pair.
        - ```
          hr:  65        # Home runs
          avg: 0.278     # Batting average
          rbi: 147       # Runs Batted In
          ```
    - Mapping Scalars to Sequences
      - Use indentation for scope of the value of a mapping
        - ```
          american:
            - Boston Red Sox
            - Detroit Tigers
            - New York Yankees
          national:
            - New York Mets
            - Chicago Cubs
            - Atlanta Braves
          ```
    - Sequence of Mappings
      - Use indentation for scope of each node in a sequence
        - ```
          -
            name: Mark McGwire
            hr:   65
            avg:  0.278
          -
            name: Sammy Sosa
            hr:   63
            avg:  0.288
          ```
      - Within a block collection, key: value pairs can start immediately following the dash, colon, or question mark.
        - ```
          ---
          # Products purchased
          - item    : Super Hoop
            quantity: 1
          - item    : Basketball
            quantity: 4
          - item    : Big Shoes
            quantity: 1
          ```
  - ## Flow-style Collections
    - YAML also has flow styles, using explicit indicators rather than indentation to denote scope.
    - Block Sequence of Flow Sequences
      - The flow sequence is written as a comma separated list within square brackets.
        - ```
          - [name        , hr, avg  ]
          - [Mark McGwire, 65, 0.278]
          - [Sammy Sosa  , 63, 0.288]
          ```
    - Block Mapping of Flow Mapping
      - In a similar manner, the flow mapping uses curly braces.
        - ```
          Mark McGwire: {hr: 65, avg: 0.278}
          Sammy Sosa: {
              hr: 63,
              avg: 0.288
            }
          ```
  - ## Structure
    - YAML uses three dashes (“---”) to separate directives from document content. This also serves to signal the start of a document if no directives are present.
      - Two Documents in a Stream (each with a leading comment)
        - ```
          # Ranking of 1998 home runs
          ---
          - Mark McGwire
          - Sammy Sosa
          - Ken Griffey
          
          # Team ranking
          ---
          - Chicago Cubs
          - St Louis Cardinals
          ```
      - Single Document with Two Comments
        - ```
          ---
          hr: # 1998 hr ranking
            - Mark McGwire
            - Sammy Sosa
          rbi:
            # 1998 rbi ranking
            - Sammy Sosa
            - Ken Griffey
          ```
    - Three dots ( “...”) indicate the end of a document without starting a new one, for use in communication channels.
      - Two Documents in a Stream (each with three dots)
        - ```
          ---
          time: 20:03:20
          player: Sammy Sosa
          action: strike (miss)
          ...
          ---
          time: 20:03:47
          player: Sammy Sosa
          action: grand slam
          ...
          ```
    - Repeated nodes (objects) are first identified by an anchor (marked with the ampersand—“&”), and are then aliased (referenced with an asterisk—“*”) thereafter.
      - Node for "Sammy Sosa" appears twice in this document
        - ```
          ---
          hr:
            - Mark McGwire
            - &SS Sammy Sosa   # This node is labeled SS
          rbi:
            - *SS              # Subsequent occurrence
            - Ken Griffey
          ```
    - A question mark and space (“? ”) indicate a complex mapping key.
      - Mapping between Sequences
        - ```
          ? - Detroit Tigers
            - Chicago cubs
          :
            - 2001-07-23
          
          ? [ New York Yankees,
              Atlanta Braves ]
          : [ 2001-07-02, 2001-08-12,
              2001-08-14 ]
          ```
  - ## Scalars
    - Scalar content can be written in block notation, using a literal style (indicated by “|”) where all line breaks are significant.
      - In literals, newlines are preserved
      - ```
        --- |
          \//||\/||
          // ||  ||__
        ```
    - Alternatively, they can be written with the folded style (denoted by “>”) where each line break is folded to a space unless it ends an empty or a more-indented line.
      - In the folded scalars, newlines become spaces
      - ```
        --- >
          Mark McGwire's
          year was crippled
          by a knee injury.
        ```
    - Folded newlines are preserved for "more indented" and blank lines.
      - ```
        >
         Sammy Sosa completed another
         fine season with great stats.
        
           63 Home Runs
           0.288 Batting Average
        
         What a year!
        ```
    - Indentation determines scope.
      - ```
        name: Mark McGwire
        accomplishment: >
          Mark set a major league
          home run record in 1998.
        stats: |
          65 Home Runs
          0.278 Batting Average
        ```
    - YAML’s flow scalars include the plain style (most examples thus far) and two quoted styles.
      - The double-quoted style provides escape sequences.
      - ```
        unicode: "Sosa did fine.\u263A"
        control: "\b1998\t1999\t2000\n"
        hex esc: "\x0d\x0a is \r\n"
        ```
      - The single-quoted style is useful when escaping is not needed.
      - ```
        single: '"Howdy!" he cried.'
        quoted: ' # Not a ''comment''.'
        tie-fighter: '|\-*-/|'
        ```
      - All flow scalars can span multiple lines; line breaks are always folded.
      - ```
        
        
        plain:
          This unquoted scalar
          spans many lines.
        
        quoted: "So does this
          quoted scalar.\n"
        ```
  - ## Tags
    - In YAML, untagged nodes are given a type depending on the application.
      - ```
        # integers
        canonical integer: 12345
        decimal: +12345
        octal: 0o14
        hexadecimal: 0xC
        
        # floating points
        canonical floating point: 1.23015e+3
        exponential: 12.3015e+02
        fixed: 1230.15
        negative infinity: -.inf
        not a number: .NaN
        
        # timestamps
        canonical timestamp: 2001-12-15T02:59:43.1Z
        iso8601: 2001-12-14t21:59:43.10-05:00
        spaced: 2001-12-14 21:59:43.10 -5
        date: 2002-12-14
        
        # miscellaneous
        null:
        booleans: [ true, false ]
        string: '012345'
        ```
    - Explicit typing is denoted with a tag using the exclamation point (“!”) symbol.
      - ```
        ---
        not-date: !!str 2002-04-28
        
        picture: !!binary |
         R0lGODlhDAAMAIQAAP//9/X
         17unp5WZmZgAAAOfn515eXv
         Pz7Y6OjuDg4J+fn5OTk6enp
         56enmleECcgggoBADs=
        ```
    - Unordered sets
      - ```
        # Sets are represented as a mapping where
        # each key is associated with a null value
        --- !!set
        ? Mark McGwire
        ? Sammy Sosa
        ? Ken Griff
        ```
    - Ordered mappings
      - ```
        # Ordered maps are represented as a sequence of mappings,
        # with each mapping having one key
        --- !!omap
        - Mark McGwire: 65
        - Sammy Sosa: 63
        - Ken Griffy: 58
        ```
    - Global tags are URIs and may be specified in a tag shorthand notation using a handle.
      - ```
        %TAG ! tag:clarkevans.com,2002:
        --- !shape
          # Use the ! handle for presenting
          # tag:clarkevans.com,2002:circle
        - !circle
          center: &ORIGIN {x: 73, y: 129}
          radius: 7
        - !line
          start: *ORIGIN
          finish: { x: 89, y: 102 }
        - !label
          start: *ORIGIN
          color: 0xFFEEBB
          text: Pretty vector drawing.
        ```
    - Application-specific local tags may also be used.
      - ```
        application specific tag: !something |
         The semantics of the tag
         above may be different for
         different documents.
        ```
  - ## Full Length Examples
    - Invoice
      - ```
        --- !<tag:clarkevans.com,2002:invoice>
        invoice: 34843
        date   : 2001-01-23
        bill-to: &id001
            given  : Chris
            family : Dumars
            address:
                lines: |
                    458 Walkman Dr.
                    Suite #292
                city    : Royal Oak
                state   : MI
                postal  : 48046
        ship-to: *id001
        product:
            - sku         : BL394D
              quantity    : 4
              description : Basketball
              price       : 450.00
            - sku         : BL4438H
              quantity    : 1
              description : Super Hoop
              price       : 2392.00
        tax  : 251.42
        total: 4443.52
        comments:
            Late afternoon is best.
            Backup contact is Nancy
            Billsmer @ 338-4338.
        ```
    - Log File
      - ```
        ---
        Time: 2001-11-23 15:01:42 -5
        User: ed
        Warning:
          This is an error message
          for the log file
        ---
        Time: 2001-11-23 15:02:31 -5
        User: ed
        Warning:
          A slightly different error
          message.
        ---
        Date: 2001-11-23 15:03:17 -5
        User: ed
        Fatal:
          Unknown variable "bar"
        Stack:
          - file: TopClass.py
            line: 23
            code: |
              x = MoreObject("345\n")
          - file: MoreClass.py
            line: 58
            code: |-
              foo = bar
        ```
- # JSON
  - [ECMA-404 The JSON Data Interchange Standard](https://www.json.org/json-en.html)
  - JSON (JavaScript Object Notation) is a lightweight data-interchange format. It is easy for humans to read and write. It is easy for machines to parse and generate.
  - JSON is built on two structures:
    - A **collection of name/value pairs**. In various languages, this is realized as an object, record, struct, dictionary, hash table, keyed list, or associative array.
    - An **ordered list of values**. In most languages, this is realized as an array, vector, list, or sequence.
  - ## Objects
    - ```
      object
          '{' ws '}'
          '{' members '}'
      
      members
          member
          member ',' members
      
      member
          ws string ws ':' element
      ```
    - An object is an unordered set of name/value pairs.
    - An object begins with "{" (left brace) and ends with "}" (right brace).
    - Each name is followed by ":" (colon) and the name/value pairs are separated by "," (comma).
  - ## Arrays
    - ```
      array
          '[' ws ']'
          '[' elements ']'
      
      elements
          element
          element ',' elements
      ```
    - An array is an ordered collection of values.
    - An array begins with "]" (left bracket) and ends with "]" (right bracket).
    - Values are separated by "," (comma).
  - ## Elements, Values and White Spaces
    - ```
      json
          element
      
      element
          ws value ws
      
      value
          object
          array
          string
          number
          "true"
          "false"
          "null"
      
      ws
          ""
          '0020' ws       # space
          '000A' ws       # linefeed
          '000D' ws       # carriage return
          '0009' ws       # hirozontal tab
      ```
    - A whitespace, denoted as `ws` in this grammar, is a space (`0020`), a linefeed (`000A`), a carriage return (`000D`), or a horizontal tab (`0009`).
  - ## Strings
    - ```
      string
          '"' characters '"'
      
      characters
          ""
          character characters
      
      character
          '0020' . '10FFFF' - '"' - '\'
          '\' escape
      
      escape
          '"'         # quotation mark
          '\'         # reverse solidus
          '/'         # solidus
          'b'         # back space
          'f'         # form feed
          'n'         # linefeed
          'r'         # carriage return
          't'         # horizontal tab
          'u' hex hex hex hex
      
      hex
          digit
          'A' . 'F'
          'a' . 'f'
      ```
  - ## Numbers
    - ```
      number
          integer fraction exponent
      
      integer
          digit
          onenine digits
          '-' digit
          '-' onenine digits
      
      digits
          digit
          digit digits
      
      digit
          '0'
          onenine
      
      onenine
          '1' . '9'
      
      fraction
          ""
          '.' digits
      
      exponent
          ""
          'E' sign digits
          'e' sign digits
      
      sign
          ""
          '+'
          '-'
      ```
  - ## Comparison to YAML
    - Sequence of Scalars
      - In YAML
        - ```
          - Mark McGwire
          - Sammy Sosa
          - Ken Griffey
          ```
      - In JSON
        - ```
          [
            "Mark McGwire",
            "Sammy Sosa",
            "Ken Griffey"
          ]
          ```
    - Mapping Scalars to Scalars
      - In YAML
        - ```
          hr:  65        # Home runs
          avg: 0.278     # Batting average
          rbi: 147       # Runs Batted In
          ```
      - In JSON
        - ```
          {
            "hr": 65,
            "avg": 0.278,
            "rbi": 147
          }
          ```
    - Mapping Scalars to Sequences
      - In YAML
        - ```
          american:
            - Boston Red Sox
            - Detroit Tigers
            - New York Yankees
          national:
            - New York Mets
            - Chicago Cubs
            - Atlanta Braves
          ```
      - In JSON
        - ```
          {
            "american": [
              "Boston Red Sox",
              "Detroit Tigers",
              "New York Yankees"
            ],
            "national": [
              "New York Mets",
              "Chicago Cubs",
              "Atlanta Braves"
            ]
          }
          ```
    - Sequence of Mappings
      - In YAML
        - ```
          -
            name: Mark McGwire
            hr:   65
            avg:  0.278
          -
            name: Sammy Sosa
            hr:   63
            avg:  0.288
          ```
      - In JSON
        - ```
          [
            {
              "name": "Mark McGwire",
              "hr": 65,
              "avg": 0.278
            },
            {
              "name": "Sammy Sosa",
              "hr": 63,
              "avg": 0.288
            }
          ]
          ```
    - Mapping between Sequences
      - In YAML
        - ```
          ? - Detroit Tigers
            - Chicago cubs
          :
            - 2001-07-23
          
          ? [ New York Yankees,
              Atlanta Braves ]
          : [ 2001-07-02, 2001-08-12,
              2001-08-14 ]
          ```
      - In JSON
        - ```
          {
            "Detroit Tigers,Chicago cubs": [
              "2001-07-23T00:00:00.000Z"
            ],
            "New York Yankees,Atlanta Braves": [
              "2001-07-02T00:00:00.000Z",
              "2001-08-12T00:00:00.000Z",
              "2001-08-14T00:00:00.000Z"
            ]
          }
          ```
      - Caviet
        - > Keys in YAML can be a sequence, but keys in JSON must be a string.
    - Compact Nested Mapping
      - In YAML
        - ```
          ---
          # Products purchased
          - item    : Super Hoop
            quantity: 1
          - item    : Basketball
            quantity: 4
          - item    : Big Shoes
            quantity: 1
          ```
      - In JSON
        - ```
          [
            {
              "item": "Super Hoop",
              "quantity": 1
            },
            {
              "item": "Basketball",
              "quantity": 4
            },
            {
              "item": "Big Shoes",
              "quantity": 1
            }
          ]
          ```
  - ## Full Length Examples
    - Invoice
      - ```
        {
          "invoice": 34843,
          "date": "2001-01-23T00:00:00.000Z",
          "bill-to": {
            "given": "Chris",
            "family": "Dumars",
            "address": {
              "lines": "458 Walkman Dr.\nSuite #292\n",
              "city": "Royal Oak",
              "state": "MI",
              "postal": 48046
            }
          },
          "ship-to": {
            "given": "Chris",
            "family": "Dumars",
            "address": {
              "lines": "458 Walkman Dr.\nSuite #292\n",
              "city": "Royal Oak",
              "state": "MI",
              "postal": 48046
            }
          },
          "product": [
            {
              "sku": "BL394D",
              "quantity": 4,
              "description": "Basketball",
              "price": 450
            },
            {
              "sku": "BL4438H",
              "quantity": 1,
              "description": "Super Hoop",
              "price": 2392
            }
          ],
          "tax": 251.42,
          "total": 4443.52,
          "comments": "Late afternoon is best. Backup contact is Nancy Billsmer @ 338-4338."
        }
        ```
    - Log File
      - ```
        {
          "Date": "2001-11-23T20:03:17.000Z",
          "User": "ed",
          "Fatal": "Unknown variable \"bar\"",
          "Stack": [
            {
              "file": "TopClass.py",
              "line": 23,
              "code": "x = MoreObject(\"345\\n\")\n"
            },
            {
              "file": "MoreClass.py",
              "line": 58,
              "code": "foo = bar"
            }
          ]
        }
        ```