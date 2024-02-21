---
sidebar_label: 'Backoffice'
sidebar_position: 12
---

# GeneXus Enterprise AI Backoffice

Table of contents
=================

* [Introduction](#introduction)
* [Dashboard](#dashboard)
* [Projects](#projects)
* [Assistants](#assistants)
   * [Creating a Prompt Assistant](#creating-a-prompt-assistant)
   * [Creating a Chat Assistant](#creating-a-chat-assistant)
* [RAG Assistants](#rag-assistants)
* [Playground](#Playground)
* [Observability](#observability)
  * [Requests](#requests)   
* [API Tokens](#api-tokens)
* [Members](#members)

## Introduction

Welcome to GeneXus Enterprise AI. This backoffice provides organization administrators with a range of essential options to configure projects and manage key settings. Through the GeneXus Enterprise AI backoffice, you can easily customize projects, define and test assistants, generate API tokens, manage members, and adjust observability options. 

This documentation will guide you through the various features and functionalities of the GeneXus Enterprise AI backoffice.

# Dashboard

When accessing the GeneXus Enterprise AI backoffice, a dashboard is shown on the home screen. This dashboard displays essential indicators related to usage and costs for the selected project. 

![image](https://github.com/genexus-books/Saia/blob/cde4bf5b860f8d4676dff97688adaddf3599df3e/saia-docs/assets/images/Dashboard.png?raw=true)

At the right top of the header, you can select the project you want to view. After selecting a project, the information shown on the dashboard is filtered, as are all the options offered in the left menu.

On the left side of the screen, you can find the backoffice menu, which grants access to different options based on your credentials. This menu enables you to navigate the various features and functionalities offered by the GeneXus Enterprise AI backoffice.

![image](https://github.com/genexus-books/Saia/blob/cde4bf5b860f8d4676dff97688adaddf3599df3e/saia-docs/assets/images/Dashboard1.png?raw=true)

The dashboard offers a comprehensive and flexible view of the project execution, as it allows changing the Start date and End date. In this way, it facilitates the analysis of all metrics and measurements on the dates set.

On this screen, you will find essential information such as Total Cost and Average Cost by Request, shown with dollar symbols and two-decimal precision. Additional metrics such as Average Request Time in milliseconds (ms) are also provided, along with a detailed breakdown of Total Requests, Total Requests Errors, and Overall Error Rate percentage.

For an even more comprehensive overview, a graphical timeline view is displayed, with daily evolution of cost and daily requests per project over the specified time.
The timeline graphs are flexible and can be adjusted to a specific time period using a slider conveniently located below each graph. This allows you to customize them by simply sliding the control to focus on particular intervals. 

![image](https://github.com/genexus-books/Saia/blob/cde4bf5b860f8d4676dff97688adaddf3599df3e/saia-docs/assets/images/Dashboard2.png?raw=true)

The Pivot Activity by Assistants table allows you to filter and explore the contribution of different assistants and models to the project. Detailed data on the cost and number of requests associated with each assistant and model provides valuable insight into individual performance.

![image](https://github.com/genexus-books/Saia/blob/cde4bf5b860f8d4676dff97688adaddf3599df3e/saia-docs/assets/images/Dashboard3.png?raw=true)

The "Drop filters here" option makes it possible to set filters that affect the entire table. To do so, drag elements from the rows or columns and select a value. For example, by dragging "Assistant" to "Drop filters here", the table will be displayed as shown and you can select the values [all] or N/A: 

![image](https://github.com/genexus-books/Saia/blob/cde4bf5b860f8d4676dff97688adaddf3599df3e/saia-docs/assets/images/Dashboard4.png?raw=true)

In addition, the "Assistant" and "Model" columns can be customized by clicking on the cog icon. The "Assistant" column offers the following configuration options:

  * **Sorting:** Ascending, Descending
  * **Subtotals:** Add or remove subtotals to summarize information.
  * **Restore default view:** Revert to the original view settings.
  * **Move to Column:** Change the arrangement of column-specific items.
  * **Search:** Make quick searches for specific models.
 
On the other hand, in the "Model" column, the configuration options when clicking on the cog icon are:
Sorting: Ascending, Descending

  * **Subtotals:** Add or remove subtotals to summarize information.
  * **Restore default view:** Restore the original view settings.
  * **Move to Column:** Change the arrangement of column-specific items.
  * **Search:** Make quick searches for specific models.
  * **Model-specific list:** Filter information by models according to the user's preferences.
  
Finally, by clicking on the three horizontal lines in the upper right corner of the Pivot Activity by Assistants table, you can access options such as export, column visibility:

  * **Export:** XML, HTML, PDF, XLS, XLSX.
  * **Visible columns:** Customize which columns are visible in the table.
  
The dashboard allows you to explore the users' metrics. To access these metrics, click on "User".

![image](https://github.com/genexus-books/Saia/blob/cde4bf5b860f8d4676dff97688adaddf3599df3e/saia-docs/assets/images/Dashboard5.png?raw=true)

This specific panel is automatically filtered by the organization and project selected.

Note that the filters by date range are independent in each Dashboard, both in "Project" and "User".
The metrics provided in the user Dashboard include:

  * **Total active users:** This is obtained by running a select count distinct on the requests table; more specifically, on the "RequestUserId" field. This provides an accurate total of active users in the system.
  * **Average Cost by User:** Calculate the average cost incurred by each user.
  * **Average Requests by User:** Provide the average number of questions or interactions of each user with the various assistants.
  * **Top Ten Users by Cost:** Identify and display the top ten users with the highest costs, considering the possibility that "RequestUserId" may be null.
  * **Top Ten Users by Request:** List the ten users with the highest number of requests, even considering that "RequestUserId” may be null.
  * **Timeline with Average Cost by User:** Display the time evolution of the average cost per user.
  * **Timeline with Average Requests by User:** Display the temporal variation in the average number of requests per user.
  * **Pivot Activity by Users:** Provide a detailed breakdown by UserId, Assistant, Model, Cost and number of Requests.

## Projects

Projects are the core entities where several configurations and settings are defined. Within each project, you can define assistants, generate API tokens for API access, and carry out document management functionalities. You can also add, update, or delete projects within the organization.

For each project, it is possible to define quotas that limit the consumption of AI models. Additional information is available at [Managing quotas per project](https://docs.saia.ai/ManagingQuotasPerProject).

The following sections will guide you to perform project-related operations within the GeneXus Enterprise AI backoffice, enabling you to efficiently manage projects and their associated settings.

### Adding a project
To define a new project, follow the steps below:

1. Access the GeneXus Enterprise AI backoffice interface and log in with your organization administrator credentials.

2. Once logged in, select "Projects" in the left menu, below the ORGANIZATION OPTIONS section.

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

To set a project as active, go to "Projects" in the left menu. Click on "UPDATE" and, in the box below "Active", select the option to mark it with a check and activate the desired project.

Upon setting a project as active, all subsequent actions and filters within the GeneXus Enterprise AI backoffice will be applied specifically to that project. 

## Assistants
You can define Chat assistants for interactive conversations.

### Chat Assistant creation

To create a Chat assistant, follow these steps:

1. Access the GeneXus Enterprise AI backoffice interface and log in with your organization administrator credentials.

2. Go to the "Assistants" section.

3. Click on the option to "Create Chat Assistant".

4. Fill in the required details:

   - Name: Give a unique name to your Chat assistant.
   - System: Specify the initial instruction or response that the assistant should provide during conversations with users.
   - AI Provider: Select the desired AI provider, such as OpenAI, Google Vertex AI, etc.
   - AI Model: Choose the specific AI model to be used for chat interactions.

5. Optionally, test the assistant before saving it. This allows you to verify the response generated by the assistant based on the initial system instruction and selected AI settings.

6. Once you are satisfied with the response, save the first version of your Chat assistant. 

**Note:** When creating chat assistants, make sure that the instructions, prompts, or initial context provided are clear and concise to obtain accurate and meaningful responses from the AI models.

## RAG Assistants

Please review [RAG Assistants information](./RAG/HowtoRAGAssistants.md).

## Playground

It allows viewing the [Frontend](https://docs.saia.ai/Frontend) and provides a practical view of how end users will interact with the artificial intelligence models defined in the backoffice. This makes it easier to understand the interaction flow and allows adjusting the configuration to achieve a more effective user experience.

## Observability
GeneXus Enterprise AI stores and tracks every request made through its API layers, providing organizations with complete visibility into the usage of assistants, AI models, and the associated cost for each request. This allows organizations to monitor and analyze resource usage, make informed decisions about resource allocation, and optimize usage for cost-efficiency. 

With clear visibility into both usage and cost, organizations can effectively manage their AI-driven workflows, control expenses, and maximize their return on investment. 

By taking advantage of these features, organizations can maintain control over their AI infrastructure, identify areas for improvement, and make data-driven decisions to enhance operational efficiency.

### Requests
In the left menu of GeneXus Enterprise AI, the "Requests" option gives you access to a comprehensive trace that provides complete observability for each request made. 

The trace allows you to easily filter requests by model, assistant, API token, datetime range, and status, enabling you to quickly identify specific requests of interest. Furthermore, by clicking on the Module column for a particular request, you can access detailed information about that request.

Within the request details, you can view the input and output data, the specific model used for the request, the associated cost, and the timestamp indicating when the request was executed. This level of detail gives you insights into the specific details of each request, facilitating the troubleshooting, analysis, and optimization of your AI workflows.

The ability to access and review the complete details of each request enables you to understand the underlying data and processes, making it easier to identify and address any issues or areas for improvement. With this comprehensive observability feature, organizations can ensure the accuracy, efficiency, and cost-effectiveness of their AI-driven workflows in GeneXus Enterprise AI.

## API Tokens
API Tokens play a crucial role in executing GeneXus Enterprise AI APIs. These tokens are required to access and use the functionality provided by the APIs. 

There are two types of API Tokens: Organization API Tokens and Project API Tokens.

### Organization API Tokens
Certain operations require API Tokens with a higher scope, such as access to Project creation, updating and deletion.

Users with the necessary privileges can manage this type of API Tokens in order to work only with OrganizationAPI endpoints. These API Tokens are not intended to work at the project level and cannot be used to reference assistants or AI models.

### Project API Tokens
For each project, you can define multiple Project API Tokens. This allows for granular control and tracking of usage.

It is important to note that API Tokens have project-wide reach.

Moreover, the ability to assign API Tokens to specific projects allows for fine-grained access control, ensuring that only authorized individuals or systems can execute requests on specific projects.

With this level of granularity, organizations can effectively manage access permissions, track usage patterns, and maintain control over their projects defined within the GeneXus Enterprise AI API.

## Members
The Members option, available in the GeneXus Enterprise AI backoffice left menu, enables you to add new members to the selected project. 

Adding members allows you to give new users access to and involvement in the project's activities.

To invite a new member, follow the steps below:

1. Access the GeneXus Enterprise AI backoffice interface and log in with your organization administrator credentials.
2. Once logged in, select the project to which you want to add members.
4. Select the "Members" option offered in the left menu.
5. Press the "NEW INVITATION" button.
6. Enter the email address of the user you want to invite as a new member.
7. Click on "Invite Member" to send the mail with the invitation. 

Once the invited user enters the backoffice, they will gain access to the project. 

Keep in mind that the invitation is sent by email, and it is valid for 72 hours; after that, it expires and a new one must be generated.

**Note:** Make sure to provide the correct email address of the user you want to invite.

