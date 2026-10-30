id:: 6ad57405-60fe-4b29-b655-6c983343cb82

- Google Gemini
  - Prompt: "MuleSoft api experience process system; connectors"
- **The Three API Layers**
  - **API-led connectivity** is the core architectural framework in MuleSoft. It organizes your integration ecosystem into three distinct, reusable layers—System, Process, and Experience APIs—while using Anypoint Connectors to safely abstract technical complexity.
  - MuleSoft the Three API Layers by Pooja Kamath
  - | API Layer | Primary Role | Consumer | Design Traits |
    |---|---|---|---|
    | Experience APIs | Formats and configures data for a specific front-end channel or user interface (e.g., mobile apps, web browsers, or IoT devices). | Public-facing apps or front-end developers. | Lightweight; optimizes payloads for speed; manages end-user authentication and security policies. |
    | Process APIs | Implements the business logic, combines or orchestrates data from multiple **System APIs**, and performs data transformations or routing. | Private; standardizes organizational functions (e.g., "Check Inventory" or "Create Order"). | Technology-agnostic; decoupled from the exact database or ERP type used at the System layer. |
    | System APIs | Unlocks raw data from underlying **core systems of record** (e.g., Salesforce, SAP, databases) while shielding the rest of the architecture from system changes. | Private; typically consumed only by **Process APIs** or internal developer tooling. | Modifies data minimally; enforces heavy backend security; uses system-specific formats. |
- **The Role of Connectors**
  - [Anypoint Connectors](https://docs.mulesoft.com/connectors/introduction/introduction-to-anypoint-connectors) are reusable Mule runtime engine extensions that drastically minimize the amount of code required to interact with third-party software, databases, or communication protocols. Instead of building complex API connections from scratch, you configure graphical parameters inside the connector.
  - Connectors fall into three operational roles in a flow:
    - **Inbound Endpoint (Source)**: Triggers a Mule flow whenever a specific event occurs (e.g., a file is uploaded to an SFTP server, or an HTTP call is received).
    - **Message Processor**: Executes actions or operations inside the mid-section of a flow (e.g., querying data, updating records).
    - **Outbound Endpoint**: Transmits the processed payload out to the destination target system.
  - How Connectors Fit the API-Led Model? Connectors operate continuously across the layers but change based on the objective:
    - **At the System API Layer**: You rely on App-Specific Connectors (like the **Salesforce Connector** or **SAP Connector**) and Database/Protocol Connectors (like Database, FTP, File, or JMS) to fetch raw data.
    - **Between Layers**: The **HTTP Request Connector** is the primary vehicle used when an Experience API needs to make a REST call down to a Process API, or when a Process API requests data from a System API.
    - **Customization**: If you are building integration flows for custom or proprietary systems that do not have an existing extension in the Anypoint Exchange, developers use the **Mule SDK** to build custom connectors tailored specifically to their organization's needs.
- **A MuleSoft Flow Example**
  - [Consume Data from an API](https://docs.mulesoft.com/mule-runtime/latest/rest-api-examples) in the MuleSoft documentation shows how to consume the REST API in Standalone XML.
  - In the XML, you configure
    - the HTTP Listener connector
      - ```
        <http:listener-connection host="0.0.0.0" port="8081" />
        ```
    - the HTTP Request connector
      - ```
        <http:request-connection host="baconipsum.com" port="80"/>
        ```
    - the File Write component
      - ```
        <file:write path="/Users/max/output.txt"/>
        ```
  - The following diagram in the Mule Palette in Anypoint Studio shows the flow of the example:
    - MuleSoft REST API example
  - The following code shows how to define the flow in Standalone XML:
    - ```
      <?xml version="1.0" encoding="UTF-8"?>
      
      <mule xmlns:file="http://www.mulesoft.org/schema/mule/file"
        xmlns:http="http://www.mulesoft.org/schema/mule/http"
        xmlns:ee="http://www.mulesoft.org/schema/mule/ee/core"
        xmlns="http://www.mulesoft.org/schema/mule/core"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.mulesoft.org/schema/mule/core
          http://www.mulesoft.org/schema/mule/core/current/mule.xsd
          http://www.mulesoft.org/schema/mule/ee/core
          http://www.mulesoft.org/schema/mule/ee/core/current/mule-ee.xsd
          http://www.mulesoft.org/schema/mule/http
          http://www.mulesoft.org/schema/mule/http/current/mule-http.xsd
          http://www.mulesoft.org/schema/mule/file
          http://www.mulesoft.org/schema/mule/file/current/mule-file.xsd">
      
        <http:listener-config name="HTTP_Listener_config">
          <http:listener-connection host="0.0.0.0" port="8081" />
        </http:listener-config>
      
        <http:request-config name="HTTP_Request_configuration" basePath="/api">
          <http:request-connection host="baconipsum.com" port="80"/>
        </http:request-config>
      
        <flow name="consumeRestAPI">
          <http:listener config-ref="HTTP_Listener_config" path="/"/>
          <http:request method="GET" config-ref="HTTP_Request_configuration" path="/">
            <http:query-params ><![CDATA[#[output application/java
              ---
              {
                "type" : message.attributes.queryParams['type'],
                "sentences" : message.attributes.queryParams['sentences']
              }]]]>
            </http:query-params>
          </http:request>
          <file:write path="/Users/max/output.txt"/>
        </flow>
      </mule>
      ```
  - The HTTP Request operation `http:request` in the flow (the 27th row in the XML) is configured to query the remote REST API at `http://baconipsum.com/api`.
    - Visit the [Bacon Ipsum](https://baconipsum.com/) page to see how the API can be used.
    - In a web browser window, trigger the flow of the application by querying the HTTP Listener on localhost port 8081:
      - > http://localhost:8081?type=meat-and-filler
    - Then the MuleSoft flow writes the following output:
      - ```
        ["Doner ullamco ea non, porchetta ... ..."]
        ```
    - The HTTP Request operation uses **DataWeave expressions** to extract the query parameters from the properties in the message.
      - ```
        "type" : message.attributes.queryParams['type'],
        "sentences" : message.attributes.queryParams['sentences']
        ```
    - Then the HTTP Request Connector constructs the full URL for the remote API, including the query parameters:
      - > http://baconipsum.com/api/?type=meat-and-filler