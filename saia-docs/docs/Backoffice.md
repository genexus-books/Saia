---
sidebar_label: 'Backoffice'
sidebar_position: 10
---

# GeneXus Enterprise AI Backoffice

Table of contents
=================

* [Introduction](#introduction)
* [Projects](#projects)
* [Members](#members)
* [Assistants](#assistants)
   * [Creating a Prompt Assistant](#creating-a-prompt-assistant)
   * [Creating a Chat Assistant](#creating-a-chat-assistant)
* [Search documents](Documents.md)
* [Observability](#observability)
  * [Requests](#requests)   
* [API Tokens](#api-tokens)

## Introduction

Welcome to GeneXus Enterprise AI. This backoffice provides organization administrators with a range of essential options to configure projects and manage key settings. Through the GeneXus Enterprise AI backoffice, you can easily customize projects, define and test assistants, define search domains, generate API tokens, manage members, and adjust observability options. 

This documentation will guide you through the several features and functionalities of the GeneXus Enterprise AI backoffice.

When accessing the GeneXus Enterprise AI backoffice, a dashboard is shown on the home screen. This dashboard displays essential indicators related to usage and costs for the selected project. 

At the right top of the header, you can select the project you want to view. After selecting a project, the information shown on the dashboard is filtered as well as all the options offered in the left menu. In addition, next to the project name you can find a user menu to access user-specific tasks and a configuration menu that allows you to customize settings such as dark mode or font size.

On the left side of the screen, you can find the backoffice menu, which grants access to different options based on your credentials. This menu enables you to navigate through the several features and functionalities offered by the GeneXus Enterprise AI backoffice.

![image](https://github.com/genexus-books/Saia/blob/209a33f6176b4594f201887077fa7b52c57b73c6/saia-docs/assets/images/gx-eai-BackOffice.png?raw=true)

## Projects
Projects are the core entities where several configurations and settings are defined. Within each project, you can define assistants, generate API tokens for API access, and carry out document management functionalities. You can also add, update, or delete projects within the organization.

The following sections will guide you on how to perform project-related operations within the GeneXus Enterprise AI backoffice, empowering you to efficiently manage projects and their associated settings.

### Adding a project
To define a new project, follow the steps below:

1. Access the GeneXus Enterprise AI backoffice interface and log in with your organization administrator credentials.

2. Once logged in, select "Manage Projects" in the left menu.

3. Click on the "New Project" button.

4. In the project creation form, provide the following information:

   - Project Name: Enter a meaningful and descriptive name for the project.
   - Project Description: Briefly summarize the purpose or objective of the project.

5. After entering the necessary information, click on the "Confirm" button to save the project details.

Congratulations! You have successfully created a new project using the GeneXus Enterprise AI backoffice. Now, you can proceed to configure assistants, define API tokens, and manage document management settings for this project.

Note: It is recommended to choose project names and descriptions that accurately reflect the intended purpose and scope of the project. This facilitates better organization and improves clarity for all users involved in project management.

### Set as Active
You have the option to set a certain project as the active project. 

The "Set as Active" function provides the same action as selecting a certain project in the header combo box.

To set a project as active, select "Manage Projects" in the left menu, the projects will be displayed and for the desired project you only have to click on the "Set as Active" button.
 
Upon setting a project as active, all subsequent actions and filters within the GeneXus Enterprise AI backoffice will be applied specifically to that project. 

## Members
The Members option, available in the GeneXus Enterprise AI backoffice left menu, enables you to add new members to the selected project. 

Adding members allows you to give access and involvement to a new user in the project's activities.

To invite a new member, follow the steps below:

1. Access the GeneXus Enterprise AI backoffice interface and log in with your organization administrator credentials.
2. Once logged in, select the project to which you want to add members.
4. Select the "Members" option offered in the left menu.
5. Press the "Insert" button.
6. Enter the email address of the user you want to invite as a new member.
7. Click on the "Invite Member" to send the invitation.

Once the invited user enters the back office, they will gain access to the project. Keep in mind that the invitation is valid for 24 hours, after which it expires and a new one must be generated.

Note: Make sure to provide the correct email address of the user you want to invite.

## Assistants
You can define two types of assistants: 

- Prompt assistants for text completion
- Chat assistants for interactive conversations

This section will guide you through the process of creating both types of assistants.

### Prompt Assistant creation

To create a Prompt assistant, follow these steps:

1. Access the GeneXus Enterprise AI backoffice interface and log in with your organization administrator credentials.

2. Select the "Assistants" option offered in the left menu.

3. Click on the "CREATE PROMPT ASSISTANT" link.

4. Complete the required details:

![image](https://github.com/genexus-books/Saia/blob/209a33f6176b4594f201887077fa7b52c57b73c6/saia-docs/assets/images/gx-eai-AssistantEditor.png?raw=true)

   - Name: Give a unique name to your Prompt assistant.
   - Prompt: Specify the initial prompt or context for the assistant to generate text completion.
   - Mode: in this case is Text
   - Provider: Select the desired AI provider, such as OpenAI, Google VertexAI, etc.
   - Model: Choose the specific AI model to be used for text completion.
   - Temperature: This setting determines the level of randomness in the response. A value of zero results in a repetitive and predictable response.
   - Max Tokens: Configure the maximum number of tokens to return in the generated text completion, based on the chosen AI model.

5. Optionally, test the assistant before saving it. This helps you evaluate the response generated by the assistant based on the provided initial context prompt and settings. You can use the User Input field and then press the Test button to get in the Response field your result. 

6. Once you are satisfied with the response, save the first version of your Prompt assistant.

### Chat Assistant creation

To create a Chat assistant, follow these steps:

1. Access the GeneXus Enterprise AI backoffice interface and log in with your organization administrator credentials.

2. Go to the "Assistants" section.

3. Click on the option to Create Chat Assistant.

4. Fill in the required details:

   - Name: Give a unique name to your Chat assistant.
   - System: Specify the initial instruction or response that the assistant should provide during conversations with users.
   - AI Provider: Select the desired AI provider, such as OpenAI, Google VertexAI, etc.
   - AI Model: Choose the specific AI model to be used for the chat interactions.

5. Optionally, test the assistant before saving it. This allows you to verify the response generated by the assistant based on the initial system instruction and selected AI settings.

6. Once you are satisfied with the response, save the first version of your Chat assistant. Later on, you can come back to the [RAG Assistants section](./RAG/RAGAssistantsSection.md) to tweak the configuration.

7. Go to the [Documents](./Documents.md) section to start uploading files.

Note: When creating prompt or chat assistants, ensure that the instructions, prompts, or initial context provided are clear and concise to obtain accurate and meaningful responses from the AI models.


## Observability
GeneXus Enterprise AI stores and tracks every request made through its API layers, providing organizations with complete visibility into the usage of assistants, AI models, and the associated cost for each request. This allows organizations to monitor and analyze resource usage, make informed decisions about resource allocation and optimize usage for cost-efficiency. 

With clear visibility into both usage and cost, organizations can effectively manage their AI-driven workflows, control expenses, and maximize their return on investment. 

By taking advantage of these features, organizations can maintain control over their AI infrastructure, identify areas for improvement, and make data-driven decisions to enhance operational efficiency.

### Requests
In the left menu of GeneXus Enterprise AI, the "Requests" option gives you access to a comprehensive trace that provides complete observability for each request made. 

The trace allows you to easily filter requests by model, assistant, API token, datetime range, and status, enabling you to quickly identify specific requests of interest. Furthermore, by clicking on the Module column for a particular request, you can access detailed information about that request.

Within the request details, you can view the input and output data, the specific model used for the request, the associated cost, and the timestamp indicating when the request was executed. This level of detail enables you to gain insights into the specific details of each request, facilitating troubleshooting, analysis, and optimization of your AI workflows.

The ability to access and review the complete details of each request empowers you to understand the underlying data and processes, making it easier to identify and address any issues or areas for improvement. With this comprehensive observability feature, organizations can ensure the accuracy, efficiency, and cost-effectiveness of their AI-driven workflows in GeneXus Enterprise AI.

## API Tokens
API Tokens play a crucial role in executing GeneXus Enterprise AI APIs. These tokens are required to access and use the functionality provided by the APIs. 

There are two types of API Tokens: Organization API Tokens and Project API Tokens.

### Organization API Tokens
Certain operations require API Tokens with a higher scope, such as access to Project creation, updating and deletion.

Users with the necessary privileges can manage this type of API Tokens in order to only work with OrganizationAPI endpoints. These API Tokens are not intended to work at the project level and cannot be used to reference assistants or AI models.

### Project API Tokens
For each project, you can define multiple Project API Tokens. This allows for granular control and tracking of usage, as well as the management of access permissions for specific assistants or models available through the GeneXus Enterprise AI API.

By defining API Tokens for each assistant, you can conveniently monitor the usage of assistants individually and gain insights into their performance and resource usage. Moreover, the ability to assign API Tokens to specific projects and assistants allows for fine-grained access control, ensuring that only authorized individuals or systems can execute requests on behalf of the defined assistants or models.

With this level of granularity, organizations can effectively manage access permissions, track usage patterns, and maintain control over their assistants and models within the GeneXus Enterprise AI API.
