id:: 6addb8e8-5f08-4720-9ba8-b67aa01f5afb

- [MuleSoft Vibes](https://docs.mulesoft.com/anypoint-code-builder/mulesoft-vibes) is a purpose-built assistant for the development lifecycle.
  - MuleSoft Vibes Is available directly in Anypoint Code Builder.
  - Enables you to build, deploy, and manage your projects using natural language messages.
  - You can also use **skills**, reusable instruction sets that MuleSoft Vibes activates when your task matches a skill’s description.
  - Large language models (LLMs) power these features and operate within the **Salesforce Shared Trust Boundary**.
- Through integration with the **MuleSoft MCP Server**, MuleSoft Vibes enables:
  - Developing API specifications
  - Creating and configuring applications
  - Creating agent networks
  - Searching for assets
  - Deploying and managing instances
  - Securing your applications
  - Managing custom metadata
- MuleSoft Vibes operates in two modes:
  - **Plan Mode** MuleSoft Vibes analyzes your message and produces a step-by-step plan describing how it will accomplish the task. No actions are executed automatically in this mode.
  - **Act Mode** MuleSoft Vibes attempts to perform the actions outlined in the plan by using the MuleSoft DX MCP Server tools to modify files, create resources, or execute changes to your project.
- Generate Code in MuleSoft Vibes
  - MuleSoft Vibes uses MuleSoft-optimized AI pipelines to generate high-quality code for API specifications and integration flows. The pipelines are exposed to MuleSoft Vibes through these MCP tools:
    - `generate_mule_flow` – Generates Mule integration flows.
    - `generate_api_spec` – Generates API specifications from natural language.
  - Because these tools run on pipelines optimized specifically for MuleSoft use cases, users typically receive higher-quality code outputs (on average, **60% better**) compared to generic generation.
- Provide MuleSoft Vibes with **Context**
  - You can improve the accuracy of MuleSoft Vibes output by providing additional context files or inputs. Examples include:
    - requirement files (`requirements.txt`),
    - configuration folders,
    - logs,
    - URLs, or
    - terminal output.
  - MuleSoft Vibes supports multi-modal context in the message area. Drag **images** directly into a conversation to provide visual context alongside text inputs.
- [Workflows and Rules](https://docs.mulesoft.com/anypoint-code-builder/vibes-workflows-commands)
  - **Rules** define natural-language **constraints** that MuleSoft Vibes follows when generating content.
    - Use rules to
      - enforce organizational conventions,
      - ensure consistent error handling, and
      - maintain coding standards across your workspace.
    - You can use rules to guide how MuleSoft Vibes generates or validates content. For example:
      - **Naming convention rule**: Instruct MuleSoft Vibes to ensure that all API names follow your organization’s naming standards.
      - **Validation rule**: Require that every API specification includes version information before generating related files.
      - **Security rule**: Ask MuleSoft Vibes to flag endpoints that expose sensitive data without authentication.
    - Example rule text:
      - > When generating RAML files, enforce this naming pattern:
        > <domain>-<capability>-api.
        > If the API title or baseUri doesn't follow the pattern,
        > return a warning and suggest a corrected value.
  - **Workflows** create **predefined multi-step tasks** that MuleSoft Vibes executes automatically.
    - Workflows let you streamline repeated development steps and can be triggered using slash commands (for example, `/workflow-name`) in the Mulesoft Vibes panel.
    - Workflows define a sequence of steps that MuleSoft Vibes executes to complete a task. For example:
      - **API design validation workflow**: Validate an API specification, check naming conventions, verify security requirements, and generate a summary of issues.
      - **API creation workflow**: Generate an API specification, apply validation rules, suggest improvements, and save the final version to the workspace.
      - **Troubleshooting workflow**: Analyze an error, identify the root cause, propose a fix, and update the configuration if needed.
    - Example workflow definition:
      - > Name: api-design-review
        > Description: Run governance checks before publishing a RAML spec.
        > Steps:
        > 1. Validate RAML syntax and required metadata.
        > 2. Check naming conventions from workspace rules.
        > 3. Check for unsecured endpoints that expose sensitive fields.
        > 4. Summarize findings with required fixes and suggested improvements.
    - Workflows that you define also appear as commands under **Workflow Commands**. You can trigger them the same way by typing `/` followed by the workflow name.
    - MuleSoft Vibes includes several built-in commands:
      - | Built-in command | Description |
        | ---- | ---- |
        | `/newrule` | Create a new rule based on the current conversation. |
        | `/newchat` | Start a new chat that carries over context from your current task. |
        | `/reportbug` | Report a malfunctioning output by creating a GitHub issue with the MuleSoft Vibes format. |
- [Creating API Specs with MuleSoft Vibes](https://docs.mulesoft.com/anypoint-code-builder/vibes-api-ai-create-spec)
  - [Order Management Example](https://docs.mulesoft.com/anypoint-code-builder/gen-ai-examples#order-management):
    - Your company manages a growing e-commerce platform and needs a way to streamline the handling of customer orders. You want to build an API specification that allows your backend systems to create, retrieve, update, and delete orders, ensuring consistent data models and robust error handling across all operations.
  - To generate a specification for this task, enter a message like:
    - > Generate an API specification for an order management system for an e-commerce platform. The API should include endpoints to create, retrieve, update, and delete orders, along with detailed definitions for request and response payloads, versioning details, and error handling mechanisms.
  - This message generates an API specification using the same specification language as the file that was in focus:
    - ```
      openapi: 3.0.0
      info:
        title: Order Management API
        version: 1.0.0
        description: API for managing orders in an e-commerce platform.
      servers:
        - url: https://api.ecommerce.com/v1
      paths:
        /orders:
          post:
            summary: Create a new order
            operationId: createOrder
            requestBody:
              required: true
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/Order'
            responses:
              '201':
                description: Order created successfully
              '400':
                description: Bad request
              '401':
                description: Unauthorized
              '500':
                description: Internal server error
            security:
              - bearerAuth: []
          get:
            summary: Retrieve all orders
            operationId: getOrders
            responses:
              '200':
                description: A list of orders
                content:
                  application/json:
                    schema:
                      type: array
                      items:
                        $ref: '#/components/schemas/Order'
              '401':
                description: Unauthorized
              '500':
                description: Internal server error
            security:
              - bearerAuth: []
        /orders/{orderId}:
          get:
            summary: Retrieve a specific order
            operationId: getOrder
            parameters:
              - name: orderId
                in: path
                required: true
                schema:
                  type: string
            responses:
              '200':
                description: Order details
              '404':
                description: Order not found
          put:
            summary: Update an existing order
            operationId: updateOrder
            parameters:
              - name: orderId
                in: path
                required: true
                schema:
                  type: string
            requestBody:
              required: true
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/Order'
            responses:
              '200':
                description: Order updated successfully
              '404':
                description: Order not found
          delete:
            summary: Delete an order
            operationId: deleteOrder
            parameters:
              - name: orderId
                in: path
                required: true
                schema:
                  type: string
            responses:
              '204':
                description: Order deleted successfully
              '404':
                description: Order not found
      components:
        securitySchemes:
          bearerAuth:
            type: http
            scheme: bearer
            bearerFormat: JWT
        schemas:
          Order:
            type: object
            properties:
              id:
                type: string
              customerId:
                type: string
              items:
                type: array
                items:
                  $ref: '#/components/schemas/OrderItem'
              totalAmount:
                type: number
                format: float
              status:
                type: string
                enum: [pending, processing, completed, cancelled]
          OrderItem:
            type: object
            properties:
              productId:
                type: string
              quantity:
                type: integer
              price:
                type: number
                format: float
      ```
- Generating Flows with MuleSoft Vibes
  - [New Case Notification Example](https://docs.mulesoft.com/anypoint-code-builder/int-ai-gen-flow-examples#implement-a-new-case-notification)
    - Your company receives numerous customer inquiries and support requests daily. You want to implement email notifications whenever a new case is created in Salesforce so you can respond quickly to customers and ensure timely case resolution to increase customer satisfaction.
  - To generate a flow for this task, enter a message like:
    - > Generate a flow that sends an email when a case is created in Salesforce.
  - This message generates XML code for the new flow, similar to this:
    - ```
      <flow name="sendEmailOnNewCaseFlow">
          <salesforce:new-object-listener objectType="Case" doc:name="On New Object" doc:id="8b5dec5e-31a2-4b78-9188-b681891898be" config-ref="Salesforce_Sfdc_config">
              <scheduling-strategy>
                  <fixed-frequency />
              </scheduling-strategy>
          </salesforce:new-object-listener>
          <salesforce:query-result-list doc:name="Query result list" doc:id="79da7485-339c-4ab6-956a-0684210b2ded" config-ref="Salesforce_Sfdc_config"/>
          <salesforce:query-result-stream-by-id doc:name="Query result stream by id" doc:id="bdf16fc8-017f-439b-b1fc-3bff23d881d8" config-ref="Salesforce_Sfdc_config"/>
          <ee:transform doc:name="Transform Message"  >
              <ee:message >
                 <ee:set-payload ><![CDATA[%dw 2.0
      output application/json
      ---
      payload map {
          CaseNumber: $.CaseNumber,
          Subject: $.Subject,
          Description: $.Description,
          Status: $.Status,
          Priority: $.Priority
      }]]></ee:set-payload>
              </ee:message>
          </ee:transform>
          <email:send config-ref="Email_SMTP" fromAddress="user1@domain.com" subject="New Case Created">
              <email:to-addresses >
                  <email:to-address value="user3@domain.com" />
                  <email:to-address value="user2@domain.com" />
              </email:to-addresses>
              <email:body contentType="text/html" >
                  <email:content ><![CDATA["<h1>New Case Created:</h1>"]]></email:content>
              </email:body>
          </email:send>
      </flow>
      ```
  - After reviewing and inserting the generated flow into your project:
    - Lines `24`, `25` and `26`: Replace the email addresses to use in your use case.
      - You can add the email address to your configuration properties file.
    - Line `4`: Enter a value for `fixed-frequency` attribute.
    - Add the required configuration for your Salesforce instance, such as:
      - ```
        <salesforce:sfdc-config name="Salesforce_Config">
            <salesforce:basic-connection
                username="${salesforce.username}"
                password="${salesforce.password}"
                securityToken="${salesforce.token}" />
        </salesforce:sfdc-config>
        ```
    - Add the required configuration for your email inbox, such as:
      - ```
        <email:smtp-config name="Email_SMTP">
            <email:smtps-connection host="${email.host}" user="${email.username}" password="${email.password}">
                <tls:context>
                    <tls:trust-store insecure="true" />
                </tls:context>
            </email:smtps-connection>
        </email:smtp-config>
        ```