id:: 6ad97593-0ac6-425e-8b84-2ea4ab53746c

- [Mule Runtime Engine](https://docs.mulesoft.com/mule-runtime/latest/)
  - **Mule runtime engine (Mule)**:
    - is a lightweight integration engine that runs Mule applications and supports domains and policies. Mule applications, domains, and policies share an XML DSL (domain-specific language).
  - **Mule applications**:
    - connect systems, services, APIs, and devices using API-led connectivity instead of point-to-point integrations. Mule applications provide functionality for message routing, data mapping, orchestration, reliability, security, and scalability.
  - **Mule domains**:
    - enable you to share global configurations that Mule applications need to reuse, such as default error handlers, shared properties, scheduler pools, and component configurations.
  - **Mule policies**:
    - Policies on HTTP-based APIs can enforce security, regulate traffic through Mule applications, and adapt APIs to your business needs.
    - Mule includes an embedded **API Gateway**, which enables you to apply security policies to an API, enrich incoming or outgoing messages, and add capabilities to an API without having to write any code.
    - **API Manager** supports the configuration of automated policies. Custom policy development and configuration are also supported.
  - **Maven**:
    - is a project management utility that Mule implements to enhance project development. Mule provides built-in Maven functionality. For the Mule runtime engine, you can integrate the packaging, testing, and deployment of your Mule applications, domains, and custom policies with your Maven lifecycle using the Mule Maven plugin.
  - **Mule Installation**:
    - [CloudHub 2.0](https://docs.mulesoft.com/cloudhub-2/):
      - CloudHub 2.0 is a fully managed, containerized integration platform as a service (iPaaS) where you can deploy APIs and integrations as lightweight containers in the cloud.
      - CloudHub 2.0 provides for deployments across 12 regions globally, either single or shared tenancy, and dynamically scales infrastructure and built-in services up or down to support elastic transaction volumes.
    - **Standalone Runtimes**:
      - Standalone runtimes are useful for isolated environments where connecting to the internet or [Anypoint Control Plane](https://docs.mulesoft.com/control-planes/) isn’t feasible.
        - > Anypoint Platform control planes are the MuleSoft-managed environments where you design, deploy, and manage APIs and Mule apps. Each Anypoint Platform organization lives in exactly one cloud. Sign in to that control plane and deploy Mule apps to the runtime planes it supports.
      - These environments enable you to host Mule runtime engine on a single instance, such as a physical server, virtual machine, or within a third-party cloud service like AWS or Azure.
      - This setup offers simplicity and control, but it requires that you manually manage infrastructure tasks like high availability, failover, clustering, and load balancing. No connection to Anypoint Control Plane is required, and you manage these Mule instances entirely within your own environment.
      - You can manage standalone runtimes by using cloud control planes (US Cloud, EU Cloud, MuleSoft Government Cloud) or a customer-hosted control plane ([Anypoint Platform Private Cloud Edition](https://docs.mulesoft.com/private-cloud/latest/)—a.k.a. PCE).
    - [Runtime Fabric](https://docs.mulesoft.com/runtime-fabric/latest/):
      - Runtime Fabric is a container service that enables you to run Mule applications and API gateways within a data center or third-party cloud environment that you control and manage.
      - You can install Runtime Fabric on a set of physical servers, virtual machines, or within Amazon Web Services and Microsoft Azure.
      - Runtime Fabric comes bundled with technology such as Docker and Kubernetes, which offer benefits such as high availability, failover, clustering, and load balancing.
      - Runtime Fabric can be managed only by Cloud control planes (US cloud, EU cloud). MuleSoft Government Cloud and Anypoint Platform PCE do not support Runtime Fabric.
- **Building Blocks of Mule Applications**
  - [Components](https://docs.mulesoft.com/mule-runtime/latest/mule-components):
    - [Core components](https://docs.mulesoft.com/mule-runtime/latest/about-components) are important building blocks of flows in a Mule app. Core components provide the logic for processing a Mule event as it travels in a series of linked steps through the app. Examples include the `Scheduler`, `For Each`, and `Logger` components.
    - [Connectors](https://docs.mulesoft.com/connectors/introduction/introduction-to-anypoint-connectors) are reusable extensions to Mule runtime engine (Mule) that enable you to integrate your apps with third-party APIs, databases, and standard integration protocols without writing complex code. You can connect to target systems, authenticate securely, and transform data while connectors handle the technical details.
    - [Modules](https://docs.mulesoft.com/mule-runtime/latest/mule-components#modules) group together components that were created to add flexibility to your applications by allowing you to aggregate values compress data, use Java features, use extra features for processing JSON, and more. The features that modules offer are more at the level of traditional application coding.
  - [Flows](https://docs.mulesoft.com/mule-runtime/latest/about-flows)
    - Mule apps process messages and other parts of Mule events through Mule components, connectors, and modules that are set up within the scope of Flow and Subflow components within an app.
    - An app can consist of a single flow, or it can break up processing into discrete flows and subflows that you add to the app and connect together. Mule apps in production environments typically use multiple flows and subflows to divide the app into functional modules or for error handling purposes.
    - For example, one flow might receive a record and transform data into a given format that another flow processes in some special way.
    - You can connect and trigger their execution with [Flow Reference](https://docs.mulesoft.com/mule-runtime/latest/flowref-about) components or by using the DataWeave `lookup` function within expressions and transformations. The function passes the current event to another flow for further event processing. You can think of a Flow Reference as a function call that accepts an event as input and then returns the modified event.
  - [Subflows](https://docs.mulesoft.com/mule-runtime/latest/about-flows#using-subflows)
    - Referencing subflows results in better performance than referencing a flow.
      - Subflows do not have Mule event sources.
      - Subflow do not have an error handling scope.
      - During design, subflows work as macros that replace the Flow Reference components that call them.
      - When you build the application, all Flow Reference components that call a subflow are replaced by the contents of the referenced subflow.
  - [Error Handling](https://docs.mulesoft.com/mule-runtime/latest/about-flows#error_handling)
    - Each flow (but not subflow) can have its own error handling. One reason for calling a flow through a Flow Reference component is to isolate the error handling to different contexts. For example:
      - A parent flow that receives web client requests might define HTTP-related error handling.
      - If the parent flow then calls a JMS queue for further processing, you can put the JMS event processors in a separate child flow and call that flow with a Flow Reference component.
      - This child flow can then define its own JMS-related error handling.
    - You can place a **Try scope** inside a flow or subflow to isolate error handling inside a flow without the need to create a separate flow or subflow. The trade-off is that error handling through a Try scope occurs inline inside the flow, which makes it harder to reuse between other flows or subflows.
- [Mule Events](https://docs.mulesoft.com/mule-runtime/latest/about-mule-event)
  - A Mule event travels through components inside your Mule app following the configured application logic.
    - A Mule event source triggers the generation of a Mule event and dispatches that event to the flow. Event sources include triggers such as the Scheduler and listeners such as the HTTP Listener and On New or Updated File components.
    - A Mule event that travels through components
    - The above diagram shows how a Mule vent flows through a Mule application:
      1. A trigger reaches the event source.
      2. The event source produces a Mule event.
      3. The Mule event travels sequentially through the components of the flow.
      4. Each component interacts in a pre-defined manner with the Mule event.
    - > The Mule event is immutable, so every change to an instance of a Mule event results in the creation of a new instance.
    - > In some cases when an issue occurs in an event source, the component doesn’t produce a Mule event. For example, invalid or incorrect paths in an HTTP listener configuration prevent the creation of a Mule event. Connectors log such issues in ERROR, WARN, or INFO messages.
  - A Mule event is composed of two objects:
    - A Mule Message, which contains a Payload an its associated attributes.
    - Variables, which are metadata that you use in your flow.
    - Composition of the Mule Event
    - [Message Payload](https://docs.mulesoft.com/mule-runtime/latest/about-mule-message#message-payload)
      - The message payload contains the content or body of a message. For example, the payload can contain
        - the results of an HTTP request,
        - the content of records you retrieve through the Select operation of the Database connector, or
        - the content of a file that you retrieve through a Read operation to the File or FTP connector.
      - For example, the payload of a response to an HTTP request for the JSON content at `https://jsonplaceholder.typicode.com/users` should looks like:
        - ```
          [
            {
              "id": 1,
              "name": "Leanne Graham",
              "username": "Bret",
              "email": "Sincere@april.biz",
              "address": {
                "street": "Kulas Light",
                ....
              }
            },
            {
              "id": 2,
              ....
            }
          ]
          ```
    - [Attributes](https://docs.mulesoft.com/mule-runtime/latest/about-mule-message#attributes)
      - Attributes contain the metadata associated with the body (or payload) of the message. The specific attributes of a message depend on the connector (such as HTTP, FTP, File) associated with the message.
      - For example, the attributes of a response to an HTTP request at `https://jsonplaceholder.typicode.com/users` should looks like:
        - ```
          {
             Status Code=200
             Reason Phrase=OK
             Headers=[
                date=Sun, 20 Jan 2019 19:13:51 GMT
                content-type=text/html;
                charset=UTF-8
                ....
             ]
          }
          ```
    - [Correlation ID](https://docs.mulesoft.com/mule-runtime/latest/correlation-id)
      - When Mule creates a new event, it generates a unique identifier string called a **correlation ID** before sending the event to the next processor in the flow. This ID enables you to correlate different log entries with a particular execution.
      - Consider the following flow:
        - ```
          <http:listener-config name="HTTP_Listener_config">
            <http:listener-connection host="0.0.0.0" port="8081" />
          </http:listener-config>
          <flow name="example">
            <http:listener config-ref="HTTP_Listener_config" path="/test"/>
            <set-payload value="some"/>
            <logger level="WARN" message="#[payload]"/>
          </flow>
          ```
      - Then after receiving a request, the flow produces the following log:
        - ```
          WARN  2021-03-30 14:19:52,862 \
            [[MuleRuntime].uber.04: [test-project-app].example.CPU_LITE @7a3620f2] \
            [processor: example/processors/1; event: 23d22940-917c-11eb-9209-3c22fb13cde7] \
            org.mule.runtime.core.internal.processor.LoggerMessageProcessor: some
          ```
        - In this case, the value `23d22940-917c-11eb-9209-3c22fb13cde7` is the correlation ID. This ID is unique for the corresponding event, so any other log that includes this correlation ID is referring to the same event.
- [Mule Errors](https://docs.mulesoft.com/mule-runtime/latest/mule-error-concept)
  - When a Mule app is running, both the Mule runtime engine and the module and connector operations configured in the app can throw errors that are represented by a Mule error object, which is associated with a Mule event.
  - You can handle error objects with On-Error components and find information about them in the logs.
  - When an error occurs in a Mule app, an Error Handler component routes the error to the first[On-Error Components](https://docs.mulesoft.com/mule-runtime/latest/on-error-scope-concept)configuration that matches the Mule error, such as `HTTP:NOT_FOUND`, `DB:CONNECTIVITY`, or `ANY` (the default for all Mule errors). If no error handling is configured for the error, the app follows a [default error handling](https://docs.mulesoft.com/mule-runtime/latest/error-handling#default_error_handling) process.