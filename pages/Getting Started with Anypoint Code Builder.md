id:: 6adeb7bd-9b5a-43f2-b72b-5b47d4dbc0c2

- [Anypoint Code Builder Overview](https://docs.mulesoft.com/anypoint-code-builder/)
  - To get the Anypoint Code Builder experience, [install the Anypoint Extension Pack](https://docs.mulesoft.com/anypoint-code-builder/start-acb#install-pack) into:
    - Visual Studio Code (recommended)
    - [Cursor](https://cursor.com/), Trae, Devin
  - Anypoint Extension Pack contains these extensions:
    - Anypoint Code Builder - API Extension
    - Anypoint Code Builder - APIkit Extension
    - Anypoint Code Builder - DataWeave Extension
    - Anypoint Code Builder - Dependencies Extension
    - Anypoint Code Builder - Integration Extension
    - Anypoint Code Builder - MUnit Extension
    - Anypoint Code Builder - Platform Extension
  - [Sign in to Anypoint Platform](https://docs.mulesoft.com/anypoint-code-builder/start-acb#login-ide)
    - Some Anypoint Code Builder features require you to sign in directly to Anypoint Platform.
    - Anypoint Platform requires a user account. You can use an existing account or create one from Anypoint Platform.
      - In Anypoint Code Builder, you can change your Anypoint Platform control plane to one of these options:
        - US Cloud (default)
        - Japan Cloud
        - Other regions (EU, UK, etc)
        - Private Cloud Edition
      - Anypoint Platform instances do not share user accounts, projects, or other data.
    - An administrator with an Anypoint Platform organization account must provide you with the necessary permissions. [Required permissions](https://docs.mulesoft.com/anypoint-code-builder/start-configure-permissions#permissions) include:
      - [Anypoint Exchange permissions](https://docs.mulesoft.com/access-management/permissions-by-product#exchange) so that the users can download and publish assets:
        - Viewer
        - Creator
      - [Runtime Manager permissions](https://docs.mulesoft.com/access-management/permissions-by-product#runtime-manager) to enable services related to CloudHub and CloudHub 2.0, such as deploying applications:
        - Read applications
        - Create applications
        - Delete applications
    - To use MuleSoft Vibes, you need to meet [MuleSoft prerequisites](https://docs.mulesoft.com/anypoint-code-builder/mulesoft-vibes#before-you-begin-vibes):
      - Node.js 20 LTS or later
      - The latest version of `jq`
      - The **Mule Developer Generative AI User** and **Anypoint Code Builder Developer** permissions
      - An Einstein-enabled Saleforce organization
        - Connect your Anypoint Platform organization to a Salesforce organization that has generative AI enabled,  if you have.
        - If you’re a MuleSoft customer without an existing Salesforce organization, contact your MuleSoft account team to provision an Einstein-enabled Salesforce organization.
  - [Workspaces](https://docs.mulesoft.com/anypoint-code-builder/start-workspaces)
    - When you create a project, Anypoint Code Builder stores it in your [home directory](https://docs.mulesoft.com/anypoint-code-builder/start-add-folders) by default.
      - You can create other folders for your project.
      - Don't create the project within another project directory.
    - Use a **multi-root workspace** when you work on an API-led architecture that spans an experience API, one or more process APIs, and one or more system APIs.
      - By grouping these related projects in a single workspace, you can configure a multi-project run configuration that deploys all projects to the same local runtime, so you can validate service-to-service calls across the stack.
    - **Create a project in a Workspace**
      1. Start the project creation process by running the appropriate command from the Command Palette, such as:
        - `MuleSoft: Design an API`—Start an API design project.
        - `MuleSoft: Develop an Integration`—Start an integration project.
        - `MuleSoft: Implement an API Specification`—Import an API spec from Exchange and scaffold it into an implementation project.
        - `MuleSoft: Implement this local API`—Scaffold a local API spec project into an implementation project.
        - `MuleSoft: Create an Agent Network Project`—Create an Agent Network project.
      2. In the project creation form, select **Add to workspace**.
        - You can create a new workspace or add the project to an existing workspace.
    - Anypoint Code Builder organizes actions based on their scope:
      - | Action Type | Location | Such as |
        | ---- | ---- | ---- |
        | Global actions | Always available from the Command Palette. These actions don’t depend on a specific project. | Creating a new project, importing assets from Exchange, or opening Anypoint Code Builder settings. |
        | Project-specific actions | Available from the context menu (right-click) in the Explorer view. These actions apply to a specific project. | Running, debugging, deploying, or exporting a project. |
      - When working with multiple projects in a workspace, select the target project before running project-specific actions.
    - Workspace File
      - VS Code stores workspace configuration in a file with the `.code-workspace` extension. This file maps the folders for your projects.
  - [Command Reference](https://docs.mulesoft.com/anypoint-code-builder/ref-acb-commands)
    - Anypoint Code Builder provides a set of commands through the Command Palette.
    - | Use case path | Code Builder command | Description |
      | ---- | ---- | ---- |
      | Integration | `MuleSoft: Deploy Mule Project to CloudHub` | Deploy an integration or implementation to CloudHub. |
      | | `MuleSoft: Develop an Integration` | Start an integration project. |
      | | `MuleSoft: Export Mule Project` | Create a shareable or deployable JAR from your project. |
      | | `MuleSoft: Download JDK` | Download a supported Java Development Kit version for local development. |
      | | `MuleSoft: Download Mule Runtime` | Download a Mule runtime version for local development. |
      | | `MuleSoft: Implement an API Specification` | Import an API spec from Exchange and scaffold it into an implementation project. |
      | | `MuleSoft: Import Asset from Exchange` | Import a connector, REST or GraphQL library, or DataWeave library from Exchange. |
      | | `MuleSoft: Import a Mule Project` | Import a JAR file for a Mule project and load it into a workspace. |
      | | `MuleSoft: Publish Mule Project to Exchange` | Publish an integration project to Anypoint Exchange. |
      | | `MuleSoft: Save and Hot-deploy to Local Runtime` | Save and hot-deploy to a local Mule runtime. |
      | | `MuleSoft: Run Mule Application` | Run a Mule application locally. |
      | | `MuleSoft: Run Multi-Project Configuration` | Run a saved multi-project launch configuration from your workspace. |
      | | `MuleSoft: Manage Custom Metadata` | Open custom metadata management for a Mule project. |
      | | `MuleSoft: Open Mule Project Properties` | Open Mule project properties. |
      | | `MuleSoft: Toggle Canvas Layout (Classic/Descriptive)` | Toggle the canvas layout between classic and descriptive views. |
      | API and Governance | `MuleSoft: Add Fragment dependency from Exchange` | Add an API spec fragment from Exchange to the project directory to include it in your spec. |
      | | `MuleSoft: Show API Console` | Open the API Console from an API design project. |
      | | `MuleSoft: Design an API` | Start an API design project. You can also start the project from the UI. |
      | | `MuleSoft: Implement this local API` | Scaffold a local API spec project into an implementation project. |
      | | `MuleSoft: Import API Specification from MuleSoft VCS` | Import an API spec from MuleSoft VCS. |
      | | `MuleSoft: Open API Project Properties` | Open API project properties. |
      | | `MuleSoft: Publish API Project to Exchange` | Publish an API project to Anypoint Exchange. |
      | | `MuleSoft: Re-scaffold this local API` | Re-scaffold an API after updating a local API spec. |
      | | `MuleSoft: Show API Governance` | Open the API Governance panel in an API design project. |
      | | `MuleSoft: Change Ruleset Version` | Change a governance ruleset version in an API design project. |
      | | `MuleSoft: Remove Governance Ruleset` | Remove a governance ruleset from an API design project. |
      | | `MuleSoft: Run Governance Validation with all Rulesets and Rules` | Run governance validation across all rulesets and rules in the project. |
      | | `MuleSoft: Sync Centralized Rulesets` | Sync centralized governance rulesets. |
      | | `MuleSoft: Sync and Run Validation` | Sync centralized rulesets and run governance validation. |
      | | `MuleSoft: View API Conformance Status on Exchange` | View API conformance status in Exchange. |
      | Agent Network | `MuleSoft: Create an Agent Network Project` | Create an Agent Network project. |
      | | `MuleSoft: Add Exchange Assets to Agent Network Project` | Add Exchange assets to an Agent Network project. |
      | | `MuleSoft: Deploy Agent Network` | Deploy an Agent Network project. |
      | | `MuleSoft: Publish Agent Network Assets` | Publish Agent Network assets. |
      | | `MuleSoft: Set Up Agent Network Gateways` | Set up Agent Network gateways. |
      | Others | `File: Open Folder` | Open a project folder from the Command Palette. |
      | | `View: Show Anypoint Code Builder` | Open the Anypoint Code Builder panel. |