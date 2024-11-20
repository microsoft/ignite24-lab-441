Lab requirements


# Part 1: Create a declarative agent for Microsoft 365

## Requirements

- Microsoft 365 tenant with Microsoft 365 Copilot enabled and custom app upload enabled
- Visual Studio Code
- Teams Toolkit extension

## Overview

In this lab you create a declarative agent for Microsoft 365 which can answer product support questions using information stored in documents in Microsoft 365. You will learn how to:

- **Create** a declarative agent using Teams Toolkit in Visual Studio Code.
- **Add custom instructions** to shape responses.
- **Add knowledge** by configuring the agent's grounding data to include SharePoint documents.
- **Add conversation starters** so Copilot can suggest prompts to help users get started.
- **Provision** your declarative agent to Microsoft 365 Copilot and validate the results.

## Example scenario

Suppose you work in a customer support team. You and your team handle queries about products that your organization makes from customers. You want to improve response times and provide a better experience. You store documents in a document library on a SharePoint Online site that contains product specifications, frequently asked questions, and policies for handling repairs, returns, and warranties. You want to be able to use natural language to query information in these documents and get answers quickly to customer queries.

## Exercise 1: Create a simple declarative agent

### Step 1: Scaffold project

Start by creating a declarative agent project using Teams Toolkit for Visual Studio Code.

1. Open **Visual Studio Code** (the blue one)
1. On the **Activity Bar**, select the **Teams Toolkit** icon 1️⃣. 
1. In the **Teams Toolkit** view, select **Create a New App** 2️⃣ to start the project creation wizard and complete the following prompts 3️⃣:
    1. **New Project**: Copilot Agent
    1. **App Features using Copilot Agent**: Declarative agent
    1. **Create Declarative Agent**: No plugin
    1. **Workspace folder**: Default folder
    1. **Application Name**: da-product-support

Teams Toolkit opens the project in a new window.

1. In the workspace trust dialog, select **Yes, I trust the authors**.
1. Examine the contents of the **README.md** file to learn more about the project structure and key files.

### Step 2: Examine declarative agent manifest

The declarative agent manifest defines the configuration of the agent.

- Open the **appPackage/declarativeAgent.json** file and examine the contents:

```json
{
    "$schema": "https://aka.ms/json-schemas/agent/declarative-agent/v1.0/schema.json",
    "version": "v1.0",
    "name": "da-product-support",
    "description": "Declarative agent created with Teams Toolkit",
    "instructions": "$[file('instruction.txt')]"
}
```

You define **instructions** that determines how the agent should behave and shape its responses.

The value of the **instructions** property contains a reference to a file named **instruction.txt**. The **$[file(path)]** function is provided by Teams Toolkit. The contents of the **instruction.txt** are included in the declarative agent manifest file when provisioned to Microsoft 365.

- In the **appPackage** folder, open the **instruction.txt** file and review the contents:

```md
You are a declarative agent and were created with Team Toolkit. You should start every response and answer to the user with "Thanks for using Teams Toolkit to create your declarative agent!\n\n" and then answer the questions and help the user.
```

### Step 3: Update the declarative agent manifest

Let's update the **name** and **description** properties to be more relevant to our scenario.

1. In the **appPackage** folder, open **declarativeAgent.json** file.
1. Update the **name** property value to +++Product support+++.
1. Update the **description** property value to +++Product support agent that can help answer customer queries about Contoso Electronics products+++.
1. Save your changes.

The updated file should have the following contents:

```json
{
    "$schema": "https://aka.ms/json-schemas/agent/declarative-agent/v1.0/schema.json",
    "version": "v1.0",
    "name": "Product support",
    "description": "Product support agent that can help answer customer queries about Contoso Electronics products",
    "instructions": "$[file('instruction.txt')]"
}
```

### Step 4: Package and upload the declarative agent to Microsoft 365

Next, upload your declarative agent to your Microsoft 365 tenant.

In Visual Studio Code:

1. In the **Activity Bar**, open the **Teams Toolkit** extension.
1. In the **Lifecycle** section, select **Provision**.
1. In the prompt, select **Sign in** to authenticate your Microsoft 365 account with Teams Toolkit. A web browser is launched.

Continuing in the web browser:

- Sign in using the Microsoft 365 account credentials.

Once signed in, Teams Toolkit automatically starts the provisioning process.

Continuing in Visual Studio Code:

- Wait for the upload to complete before continuing.

Next, review the output of the provisioning process.

1. In the **Activity bar**, select the files (first) icon to open the **Explorer** view and view the files list.
1. In the **appPackage/build** folder, open **declarativeAgent.dev.json** file.

Notice that the **instructions** property value contains the contents of the **instruction.txt** file. The **declarativeAgent.dev.json** file is included in the **appPackage.dev.zip** file along with the **manifest.dev.json**, **color.png**, and **outline.png** files. The **appPackage.dev.zip** file is uploaded to Microsoft 365.

### Step 5: Test the "In context" experience

Next, let's run the declarative agent in Microsoft 365 Copilot and validate its functionality in both the **in-context** and **immersive** experiences.

In Visual Studio Code:

1. In the **Activity Bar**, switch to the **Run and Debug** view.
1. Select the **Start Debugging** button next to the configuration's dropdown, or press <kbd>F5</kbd>. A new browser window is launched and navigates to Microsoft 365 Copilot.

Let's test the **in-context** experience.

Continuing in the browser:

1. In **Microsoft 365 Copilot** message box, enter the <kbd>@</kbd> symbol using your keyboard. A flyout appears with a list of available agents.
1. In the flyout, select **Product support**. Notice the status message above the message box. It displays **Chatting with Product support** above the message box and **Product support is responding** in the chat, which signifies that you're using the in-context experience of the agent.
1. In the message box, enter **Hello** and submit your message.
1. Wait for the response. Notice how the response starts with the text "Thanks for using Teams Toolkit to create your declarative agent!" as defined in the instructions you reviewed earlier.
1. To exit the in-context experience, select the cross (X) in the status message. Notice the status message is removed and a message is displayed in the chat window that indicates that you're no longer chatting with the agent.

### Step 6: Test the "immersive" experience

Finally, let's test the **immersive** experience.

Continuing in the browser:

1. In **Microsoft 365 Copilot**, select the icon in the top right to expand the Copilot side panel. Notice that the panel displays recent chats and available agents.
1. In the side panel, select **Product support** to enter the immersive experience and chat directly with the agent.
1. In the message box, enter +++Hello+++ and submit your message.
1. Send the message and wait for the response. Notice how the response starts with the text "Thanks for using Teams Toolkit to create your declarative agent!" as defined in the instructions you reviewed earlier.

Close the browser to stop the debug session in Visual Studio Code.

## Exercise 2: Configure custom knowledge

Declarative agents use custom knowledge to provide extra data and context to Microsoft 365 Copilot that is scoped to a specific scenario or task.

Custom knowledge consists of two parts:

- **Custom instructions**: defines how the agent should behave and how it should shape its responses.
- **Custom grounding**: defines the data sources that the agent can use in its responses.

### Step 1: Update custom instructions

Instructions are specific directives or guidelines that are passed to the foundation model to shape its responses. These instructions can include:

- **Task definitions**: outlining what the model should do, such as answering questions, summarizing text, or generating creative content.
- **Behavioral guidelines**: setting the tone, style, and level of detail for responses to ensure they align with user expectations.
- **Content restrictions**: specifying what the model should avoid, such as sensitive subjects, or copyrighted material.
- **Formatting rules**: showing how the output should be structured, like using bullet points or specific formatting styles.

Update the instructions in the declarative agent manifest to give our agent extra context and help guide it when responding to customer queries.

In Visual Studio Code:

1. Open the **appPackage/instruction.txt** file and update the contents with:

    ```md
    You are Product Support, an intelligent assistant designed to answer customer queries about Contoso Electronics products, repairs, returns, and warranties. You will use documents from Product marketing SharePoint site as your source of information. If you can't find the necessary information, you should suggest that the agent should reach out to the team responsible for further assistance. Your responses should be concise and always include a cited source.
    ```

1. Save your changes.

### Step 2: Configure grounding data

Grounding is the process of connecting large language models (LLM) to real-world information, enabling more accurate and relevant responses. Grounding data is used to provide context and support to the LLM when generating responses. It reduces the need for the LLM to rely solely on its training data and improves the quality of the responses.

By default, a declarative agent isn't connected to any data sources. You configure a declarative agent with one or more Microsoft 365 data sources:

- Documents stored in OneDrive
- Documents stored in SharePoint Online
- Content ingested into Microsoft 365 by a Graph connector

In a web browser:

1. In the address bar, type +++https://lodsprodmca.sharepoint.com/sites/productmarketing+++ and navigate to the Product marketing SharePoint site.

> NOTE: The site is called "productmarketing" even though it's a product support agent. Please suspend disbelief; this is only a lab!

1. In the left hand menu, select **Documents** to view the documents
1. Examine the documents contents

Configure the Documents document library in the Product marketing SharePoint site as a source of grounding data in the declarative agent manifest.

In Visual Studio Code:

1. In the **appPackage** folder, open **declarativeAgent.json** file.
1. After the **instructions** property, add a comma, then paste the following code snippet before the closing bracket:

    ```json
    "capabilities": [
      {
        "name": "OneDriveAndSharePoint",
        "items_by_url": [
          {
            "url": "https://lodsprodmca.sharepoint.com/sites/productmarketing/Shared%20Documents"
          }
        ]
      }
    ]
    ```

1. Save your changes.

The **declarativeAgent.json** file should look like this:

```json
{
    "$schema": "https://developer.microsoft.com/json-schemas/copilot/declarative-agent/v1.0/schema.json",
    "version": "v1.0",
    "name": "Product support",
    "description": "Product support agent that can help answer customer queries about Contoso Electronics products",
    "instructions": "$[file('instruction.txt')]",
    "capabilities": [
        {
            "name": "OneDriveAndSharePoint",
            "items_by_url": [
                {
                    "url": "https://lodsprodmca.sharepoint.com/sites/productmarketing/Shared%20Documents"
                }
            ]
        }
    ]
}
```

### Step 3: Workaround for issue updating DA's with SharePoint URL's

By the time you do this lab, this might be unnecessary, but at the time of this writing there is a platform bug where updates to Declarative Agents containing SharePoint capabilities do not take effect in a timely manner. To work around this, you can simply create a new application instead of updating the old one.

Continuing in Visual Studio Code:

1. In the **env** folder, open the **.env.dev** file.
1. Remove all the environment variables after the comment, **Generated during provision, you can also add your own variables.**
1. Save your changes.

The contents of the **.env.dev** file should look like:

    ```text
    # This file includes environment variables that will be committed to git by default.

    # Built-in environment variables
    TEAMSFX_ENV=dev
    APP_NAME_SUFFIX=dev

    # Generated during provision, you can also add your own variables.
    ```

Removing the environment variables forces Teams Toolkit to register a new application in Teams Developer Portal during the provisioning process.

1. In the **appPackage** folder, open the **declarativeAgent.json** file.
1. Update the name property to +++Product Support 2+++.
1. Save your changes.

Updating the name of the agent enables us to easily identify the agent that has the changes we want to test. You will therefore see two agents listed in Microsoft 365 Copilot.

### Step 4: Package and upload the declarative agent to Microsoft 365

Upload your changes to Microsoft 365 and start a debug session.

In Visual Studio Code:

1. In the **Activity Bar**, open the **Teams Toolkit** extension.
1. In the **Lifecycle** section, select **Provision**.
1. Wait for the upload to complete.
1. In the **Activity Bar**, switch to the **Run and Debug** view.
1. Select the **Start Debugging** button next to the configuration's dropdown, or press <kbd>F5</kbd>. A new browser window is launched and navigates to Microsoft 365 Copilot.

### Step 5: Test the declarative agent in Microsoft 365 Copilot

In Visual Studio Code:

1. In the **Activity Bar**, switch to the **Run and Debug** view.
1. Select the **Start Debugging** button next to the configuration's dropdown, or press <kbd>F5</kbd>. A new browser window is launched and navigates to Microsoft 365 Copilot.
1. Now test your declarative agent in Microsoft 365 and validate the results. First, let's test the instructions:

    * In **Microsoft 365 Copilot**, select the icon in the top right to **expand the Copilot side panel**.
    * Find **Product support** in the list of agents and select it to enter the immersive experience to chat directly with the agent.
    * Select the sample prompt with the title **Learn more** and send the message.
    * Wait for the response. Notice how the response is different from the previous instructions and reflects the new instructions.

Next, let's test the grounding data.

1. In the message box, enter +++Tell me about Eagle Air+++ and send the message.
1. Wait for the response. Notice that the response contains information about the Eagle Air drone. The response contains citations and references to the Eagle Air document stored on the Product marketing SharePoint Online site.

Let's try a few more prompts:

1. In the message box, enter +++Recommend a product suitable for a farmer+++ and send the message.
1. Wait for the response. Notice that the response contains information about the Eagle Air and some extra context as to why the Eagle Air is recommended. The response contains citations and references to the Eagle Air document stored on the Product marketing SharePoint Online site.
1. In the message box, enter +++Explain why the Eagle Air is more suitable than Contoso Quad+++ and send the message.
1. Wait for the response. Notice that the response explains in more detail why the Eagle Air is more suitable than the Contoso Quad for use by farmers.

Finally, let's test the fallback response by asking a question that the agent can't answer:

1. In the message box, enter +++When was Mark8 released?+++ and send the message.
1. Wait for the response. Notice that the response suggests that the agent should reach out to the team responsible for further assistance as defined in the instructions.

Close the browser to stop the debug session in Visual Studio Code.

## Exercise 3: Add conversation starters

Conversation starters are sample prompts that are displayed in the immersive experience. These sample prompts provide a quick way for users to understand the types of questions that can be asked.

### Step 1: Update the declarative agent

Update the declarative agent to include conversation starters that provide users with sample prompts to help them understand the types of questions they can ask.

In Visual Studio Code:

1. In the **appPackage** folder, open the **declarativeAgent.json** file.
1. Update the **name** property value to +++Product support 3+++ so you can see that the agent has been updated.
1. After the **capabilities** property, add a comma, then paste the following code snippet before the closing bracket:

   ```json
   "conversation_starters": [
       {
           "title": "Product information",
           "text": "Tell me about Eagle Air"
       },
       {
           "title": "Returns policy",
           "text": "What is the returns policy?"
       },
       {
           "title": "Product information",
           "text": "Can you provide information on a specific product?"
       },
       {
           "title": "Product troubleshooting",
           "text": "I'm having trouble with a product. Can you help me troubleshoot the issue?"
       },
       {
           "title": "Repair information",
           "text": "Can you provide information on how to get a product repaired?"
       },
       {
           "title": "Contact support",
           "text": "How can I contact support for help?"
       }
   ]
   ```

1. Save your changes.

The **declarativeAgent.json** file should look like this:

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/copilot/declarative-agent/v1.0/schema.json",
  "version": "v1.0",
  "name": "Product support 3",
  "description": "Product support agent that can help answer customer queries about Contoso Electronics products",
  "instructions": "$[file('instruction.txt')]",
  "capabilities": [
    {
      "name": "OneDriveAndSharePoint",
      "items_by_url": [
        {
          "url": "https://lodsprodmca.sharepoint.com/sites/productmarketing/Shared%20Documents"
        }
      ]
    }
  ],
  "conversation_starters": [
    {
      "title": "Product information",
      "text": "Tell me about Eagle Air"
    },
    {
      "title": "Returns policy",
      "text": "What is the returns policy?"
    },
    {
      "title": "Product information",
      "text": "Can you provide information on a specific product?"
    },
    {
      "title": "Product troubleshooting",
      "text": "I'm having trouble with a product. Can you help me troubleshoot the issue?"
    },
    {
      "title": "Repair information",
      "text": "Can you provide information on how to get a product repaired?"
    },
    {
      "title": "Contact support",
      "text": "How can I contact support for help?"
    }
  ]
}
```

### Step 2: Upload and run your agent

Next, upload your changes and start a debug session.

In Visual Studio Code:

1. In the **Activity Bar**, open the **Teams Toolkit** extension.
1. In the **Lifecycle** section, select **Provision**.
1. Wait for the upload to complete.
1. In the **Activity Bar**, switch to the **Run and Debug** view.
1. Select the **Start Debugging** button next to the configuration's dropdown, or press <kbd>F5</kbd>. A new browser window is launched and navigates to Microsoft 365 Copilot.

If you are prompted to log in, choose "Work and School" account and use your lab assigned username **Username: +++@lab.CloudPortalCredential(User1).Username+++** and password **Password: +++@lab.CloudPortalCredential(User1).Password+++** .

Next, test your declarative agent in Microsoft 365 and validate the results.

Continuing in the web browser:

1. In **Microsoft 365 Copilot**, select the icon in the top right to expand the **Copilot side panel**.
1. Find **Product support** in the list of agents and select it to enter the immersive experience to chat directly with the agent. Notice that the conversation starters you defined in the manifest display in the user interface.

Close the browser to stop the debug session in Visual Studio Code.

# Part 2: Create a custom engine agent

## Requirements


- Visual Studio 2022
- Teams Toolkit for Visual Studio (Microsoft Teams development workload)
- Azure subscription
- Azure OpenAI service model deployment

## Overview

Custom engine agents are chatbots for Microsoft Teams powered by generative AI, designed to provide sophisticated conversational experiences. Custom engine agents are built using the Teams AI library, which provides comprehensive AI functionalities, including managing prompts, actions, and model integration as well as extensive options for UI customization. This ensures that your chatbots leverage the full range of AI capabilities while delivering a seamless and engaging experience aligned with Microsoft platforms.

## What will we be doing?

Here, you create a custom engine agent that uses a language model hosted in Azure to answer questions using natural language:

- **Create**: Create a custom agent agent project and use Teams Toolkit in Visual Studio.
- **Provision**: Upload your custom engine agent to Microsoft Teams and validate the results.
- **Prompt template**: Determine the agent behaviour.
- **Suggested prompts**: Define prompts for starting new conversations.

## Exercise 1: Set up the project in Visual Studio 2022

### Step 1: Open starter project

Start with opening the starter project in Visual Studio 2022.

1. Open **Visual Studio 2022**.
1. In the Visual Studio 2022 welcome dialog, select **Continue without code**.
1. Open the **File** menu, expand the **Open** menu and select **Project/solution...**.
1. In the **Open Project/Solution** file picker, on the left hand menu, select **This PC**.
1. Double click **Local Disk (C:)**.
1. Navigate to **C:\Users\LabUser\TeamsApps\cea-career-genie** folder.
1. In the **cea-career-genie** folder, select **Custom.Engine.Agent.sln**, then select **Open**.

### Step 2: Examine the solution

The solution contains two projects:

- **Custom.Engine.Agent**: This is an ASP.NET Core Web API project which contains your agent code. The agent logic and generative AI capatbilies are implemented using Teams AI library. 
- **TeamsApp**: This is a Teams Toolkit project which contains the app package files, environment, workflow and infrastructure files. You will use this project to provision the required resources for your agent.

### Step 3: Create dev tunnel

Dev tunnels allow developers to securely share local web services across the internet. When users interact with the agent in Microsoft Teams, the Teams platform will send and recieve messages (called Activities) from your agent code via the Bot Framework. As the code is running on your local machine, the Dev Tunnel exposes the localhost domain which your web api runs on as a publicly accessible URL.

Continue in Visual Studio:

1. Open the **View** menu, expand **Other windows**, and select **Dev Tunnels**.
1. In the **Dev Tunnels** pane, select the **plus (+)** icon.
1. In the dialog window, create the tunnel using the following settings:
    1. **Account**: Expand the dropdown and select **Sign in**, then select **Work or school account**, then again and select **OK**. Use your Microsoft 365 account details to sign in.
    1. **Name**: custom-engine-agent
    1. **Tunnel type**: Temporary
    1. **Access**: Public
1. To create the tunnel, select **OK**.
1. In the confirmation prompt, select **OK**.
1. Close the Dev Tunnels window.

### Step 4: Configure Azure OpenAI key

Continue in Visual Studio:

1. In the **TeamsApp** project, expand the **env** folder.
1. Rename **.env.local.user.sample** to **.env.local.user**.
1. Open **.env.local.user** file.

1. Update the contents of the file, replacing [INSERT KEY] with the Azure Open AI service deployment API key.

    ```
    SECRET_AZURE_OPENAI_API_KEY=[INSERT KEY]
    ```

1. Save the changes.

> [!NOTE]
> When Teams Toolkit uses an environment variable with that is prefixed with **SECRET**, it will ensure that the value does not appear in any logs. 

1. In the **TeamsApp** project, expand the **env** folder.
1. Open the **.env.local** file.
1. Replace the values of the **AZURE_OPENAI_ENDPOINT** and **AZURE_OPENAI_DEPLOYMENT_NAME** variables with your own deployment values.
1. Save your changes.

## Exercise 2: Provision resources

Teams Toolkit help developers automate tasks using workflow files. The workflow files are YML files which are stored in the root of the TeamsApp project. 

### Step 1: Review Teams Toolkit provisioning tasks

Continue in Visual Studio:

1. In **TeamsApp** project, open **teamsapp.local.yml**.
1. Examine the contents of the file.

The file contains a single stage called **Provision** which contains several tasks.

1. **teamsApp/create**: Registers an app in Teams Developer Portal and writes the app ID to **env\.env.local**.
1. **aadApp/create**: Registers an app in Microsoft Entra and writes several values to **env\.env.local**.
1. **aadApp/update**: Applies an app manifest to the Microsoft Entra app registration.
1. **arm/deploy**: Provisions the Azure Bot Service using Bicep. It writes several values back to **env\.env.local**.
1. **file/createOrUpdateJsonFile**: Updates **appsettings.development.json** file with environment variables which can be used by code at runtime.
1. **teamsApp/validateManifest**: Validates the app manifest file.
1. **teamsApp/zipAppPackage**: Creates the Teams app package.
1. **teamsApp/validateAppPackage**: Validates the app package.
1. **teamsApp/update**: Updates the app registration in the Teams Developer Portal.

### Step 2: Use Teams Toolkit to execute the tasks in the workflow file

1. Right-click **TeamsApp** project.
1. Expand the **Teams Toolkit** menu and select **Prepare Teams App Dependencies**.
1. In the **Microsoft 365 account** dialog, select the account you used to create the Dev Tunnel earlier and select **Continue**. This will start the Dev Tunnel and write the tunnel endpoint and domain to the **env\env.local** file.
1. In the **Provision** dialog, configure the resource group to be used to host the Azure Bot Service:
    1. **Subscription**: Expand the dropdown and select the subscription in the list
    1. **Resource group**: Select **New...**, enter rg-custom-engine-agent-local the text field and then select **OK**.
    1. **Region**: Expand the dropdown and select a region from the list
    1. Select **Provision**
1. In the warning prompt, select **Provision**.
1. Wait for the process to complete, this can take a 1-3 minutes. Teams Toolkit will output its progress in the Output pane.
1. In the **Info** prompt, select **View provisioned resources** to open a browser.

Take a minute to examine the Azure Bot Service resource in the Azure Portal.

### Step 3: Run and debug

With everything in place, we are now ready to test our custom engine agent in Microsoft Teams for the first time.

First, we need to start a debug session to start our local web API that contains the agent logic.

Continue in Visual Studio:

1. To start a debug session, press <kbd>F5</kbd> on your keyboard, or select the **Start** button in the toolbar. A browser window is launched and navigates to Microsoft Teams.

Continuing in the web browser:

1. Wait for Microsoft Teams to load and for the App install dialog to appear.

Previously, Teams Toolkit registered the app in the Teams Developer Portal. To use the app we need to install it for the current user. Teams Toolkit launches the browser using a special URL which enables developers to install the app before they test it.

> [!NOTE]
> If any changes are made to the app manifest file. Developers will need to run the Prepare Teams App dependencies process again and install the app for the changes to be reflected in Microsoft Teams.

Continuing in the web browser:

1. In the App install dialog, select **Add**.
1. In the App install confirmation dialog, select **Open**. The custom engine agent is displayed in Microsoft Teams.

Now let's test that everything is working as expected.

Continuing in the web browser:

1. Enter **Hello, world!** in the message box and press <kbd>Enter</kbd> to send the message to the agent. A typing indicator appears whilst waiting for the agent to respond.
1. Notice the natural language response from the agent and a label **AI generated** is shown in the agent response.
1. Continue a conversation with the agent.
1. Go back to Visual Studio. Notice that in the Debug pane, Teams AI library is tracking the full conversation and displays appended conversation history in the output.
1. Close the browser to stop the debug session.

### Step 4: Examine agent configuration

The functionality of our agent is implemented using Teams AI library. Let's take a look at how our agent is configured.

In Visual Studio:

1. In the **Custom.Engine.Agent** project, open **Program.cs** file.
1. Examine the contents of the file.

The file sets up the web application and integrates it with Microsoft Bot Framework and services.

- **WebApplicationBuilder**: Initializes web application with controllers and HTTP client services.
- **Configuration**: Retrieve configuration options from the apps configration and sets up Bot Framework authentication.
- **Dependency injection**: Registers BotFrameworkAuthentication and TeamsAdapter services. Configures Azure Blob Storage for persisting agent state and sets up an Azure OpenAI model service.
- **Agent setup**: Registers the agent as a transient service. The agent logic is implemented using Teams AI library.

Let's take a look at the agent setup.

```
builder.Services.AddTransient<IBot>(sp =>
{
    // Create loggers
    ILoggerFactory loggerFactory = sp.GetService<ILoggerFactory>();

    // Create Prompt Manager
    PromptManager prompts = new(new()
    {
        PromptFolder = "./Prompts"
    });

    // Create ActionPlanner
    ActionPlanner<TurnState> planner = new(
        options: new(
            model: sp.GetService<OpenAIModel>(),
            prompts: prompts,
            defaultPrompt: async (context, state, planner) =>
            {
                PromptTemplate template = prompts.GetPrompt("Chat");
                return await Task.FromResult(template);
            }
        )
        { LogRepairs = true },
        loggerFactory: loggerFactory
    );

    Application<TurnState> app = new ApplicationBuilder<TurnState>()
        .WithAIOptions(new(planner))
        .WithStorage(sp.GetService<IStorage>())
        .Build();

    return app;
});
```

The key elements of the agent setup are:

- **ILoggerFactory**: Used for logging messages to the output pane for debugging.
- **PromptManager**: Determines the location of prompt templates.
- **ActionPlanner**: Determines which model and prompt should be used when handling a user message. By default, the planner uses a prompt template named **Chat**.
- **ApplicationBuilder**: Creates an object which represents a Bot that can handle incoming activities.

The agent is added as a transient service which means that everytime a message is recieved from the Bot Framework, our agent code will be executed.

## Exercise 3: Prompt templates

Prompts play a crucial role in communicating and directing the behavior of language models.

Prompts are stored in the Prompts folder. A prompt is defined as a subfolder that contains two files:

 - **config.json**: The prompt configuration. This enables you to control parameters such as temperature, max tokens etc. that are passed to the language model.
 - **skprompt.txt**: The prompt text template. This text determines the behaviour of the agent.

Here, you'll update the default prompt to change the agents behaviour.

### Step 1: Update prompt template

Continuing in Visual Studio:

1. In the **Custom.Engine.Agent** project, expand the **Prompt** folder.
1. In the **Chat** folder, open the **skprompt.txt** file. 
1. Update the contents of the file:

    ```
    You are a career specialist named "Career Genie" that helps Human Resources team for writing job posts.
    You are friendly and professional.
    You always greet users with excitement and introduce yourself first.
    You like using emojis where appropriate.
    ```

1. Save changes.

### Step 2: Test the new prompt

Now let's test our change.

1. To start a debug session, press <kbd>F5</kbd> on your keyboard, or select the **Start** button in the toolbar.

Continuing in the web browser:

1. In the app dialog, select **Open** to open the agent in Microsoft Teams.
1. In the message box, enter **Hi** and send the message. Wait for the response. Notice the change in the response.
1. In the message box, enter **Can you help me write a job post for a Senior Developer role?** and send the message. Wait for the response.

Continue the conversation by sending more messages.

- **What would be the list of required skills for a Project Manager role?**
- **Can you share a job template?**

Close the browser to stop the debug session.

## Exercise 3: Suggested prompts

Suggested prompts are shown in the user interface and a good way for users to discover how the agent can help them through examples.

Here, you'll define two suggested prompts.

### Step 1: Update app manifest

Continuing in Visual Studio:

1. In the **TeamsApp** project, expand the **appPackage** folder.
1. In the **appPackage** folder, open the **manifest.json** file.
1. In the **bots** array property, expand the first object with a **commandLists** array property.

    ```
    "commandLists": [
        {
            "scopes": [
                "personal"
            ],
            "commands": [
                {
                    "title": "Write a job post for <role>",
                    "description": "Generate a job posting for a specific role"
                },
                {
                    "title": "Skill required for <role>",
                    "description": "Identify skills required for a specific role"
                }
            ]
        }
    ]
    ```
1. Save your changes.

The **bots** array property should look like:

```
"bots": [
  {
    "botId": "${{BOT_ID}}",
    "scopes": [
      "personal"
    ],
    "supportsFiles": false,
    "isNotificationOnly": false,
    "commandLists": [
      {
        "scopes": [
          "personal"
        ],
        "commands": [
          {
            "title": "Write a job post for <role>",
            "description": "Generate a job posting for a specific role"
          },
          {
            "title": "Skill required for <role>",
            "description": "Identify skills required for a specific role"
          }
        ]
      }
    ]
  }
],
```

### Step 2: Test suggested prompts

As you've made a change to the app manifest file, we need to Run the Prepare Teams App Dependencies process to update the app registration in the Teams Developer Portal before starting a debug session to test it.

Continuing in Visual Studio:

1. Right-click **TeamsApp** project, expand the **Teams Toolkit** menu and select **Prepare Teams App Dependencies**.
1. Confirm the prompts and wait till the process completes.

Now let's test the change.

1. Start a debug session, press <kbd>F5</kbd> on your keyboard, or select the **Start** button in the toolbar.

Continuing in the web browser:

1. In the app dialog, select **Open** to open the agent in Microsoft Teams.
1. Above the message box, select **View prompts** to open the prompt suggestions flyout.
1. In the **Prompts** dialog, select one of the prompts. The text is added into the message box.
1. In the message box, replace <b>&lt;role&gt;</b> with a job title, for example, **Senior Software Engineer**, and send the message.

The prompt suggestions can also be seen when the user opens the agent for the first time.

Continuing in the web browser:

1. In the Microsoft Teams side bar, go to **Chat**.
1. Find the chat with the name **Custom engine agent** in the list and select the **...** menu.
1. Select **Delete** and confirm the action.
1. In the Microsoft Teams side bar, select **...** to open the apps flyout.
1. Select **Custom engine agent** to start a new chat. The two suggested prompts are shown in the user interface.

Close the browser to stop the debug session.

# CONGRATULATIONS you have completed Lab 441!
