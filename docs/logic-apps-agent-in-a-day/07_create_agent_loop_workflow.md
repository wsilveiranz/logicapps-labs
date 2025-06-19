# Module 7 -  Create Agent Loop Workflow
In this module we will create and configure a conversational Agent Loop workflow that will allow the user an interactive chat experience when trying to resolve operational issues at Contoso.

## Create Agent Workflow
1. Search for and navigate to the Logic Apps service

    ![Search and Navigate to the Logic Apps Service](./images/07_01_search_bar_logic_apps.png "search and navigate to logics apps")


1. Open the Logic App created earlier 

    ![Open Logic App](./images/07_02_logic_apps_list.png ("open logic app"))

1. Create a new workflow
    - Click the `Workflows -> Workflows` from  the menu on the left
    - Click `+ Add -> Add`

    ![Create New Workflow](./images/07_03_create_new_workflow.png "create new workflow")

1. Create an **Agent Workflow**:
    - **Workflow Name:** `demo-conversational-agent`
    - Select `Agent (Preview)`
    - Click `Create`

    ![Create Agent Workflow](./images/07_04_create_new_agent_workflow.png "create agent workflow")

    Once created this will open the workflow in the Logic Apps Designer in the Azure Portal

    ![Blank Agentic Workflow](./images/07_05_blank_agent_workflow.png "blank agentic workflow")


## Configure Agent Workflow
1. Configure the Trigger Activity. Since we are building a conversational agent, we need to use a Request trigger that allows the agent to be called from a chat surface/application.
    - Click the `Add a tigger` activity 
    
        ![Add a Trigger](./images/07_06_add_a_trigger.png ("add a trigger"))

    - Under the Built-in tools group, select `Request`

         ![Trigger Activity Request](./images/07_07_trigger_activity_request.png "trigger activity request")

    -  Select the `When a HTTP request is received`

        ![Trigger Activty When a HTTP request is received](./images/07_08_trigger_activity_when_a_http_request_is_received.png "trigger actvitiy when a http request is received")

    - Configure the activity:
        - **Request Body JSON Schema**
            ```JSON
            {
                "type": "object",
                "properties": {
                    "prompt": {
                        "type": "string"
                    }
                }
            }
            ```
        
            ![Configure WHne a HTTP request is received](./images/07_09_when_a_http_request_is_received_configuration.png ("configure when a http trigger is received"))
1. Add a Compose actvity to parse out the prmpt from the input request
    - Add a new action
    
    ![Add an action](./images/07_10_add_an_action.png ("add an action"))
    
    - Search for and select the **Compose** activity

    ![Add Compose action](./images/07_11_add_an_action_compose.png ("add compose action"))

    - configure the activity as follows:
        - Rename the activity to `Compose-Prompt`
        - Inputs:
            - When you click into the **input** text box, click on the lightening bolt

            ![Compose Input Option](./images/07_12_configure_compose_input_option.png ("compose input options"))

            - Under "When a HTTP request is received" select `Prompt`
            
            ![Compose Select Input](./images/07_13_configure_compose_select_file.png "compose select input")

            after are completed your configuration, the **Compose-Prompt** activity should look like this

            ![Compose-Prompt Activity](./images/07_14_compose_after_configuraiton.png "compose prompt activity")

1. In this step we will configure the **Default Agent** settings to connect to our Azure OpenAI instance created in [Module 3 - Enable OpenAI Model](03_enable_open_ai_model.md)
    - Click on the **Default Agent**

    ![Default Agent](./images/07_15_deafult_agent.png ("default agent"))

    - Configure the Deployment Model
        - **Deployment Model Name:** `gpt-4.1`
        - Click `Connect`

        ![Deployment Model Name](./images/07_16_deafult_agent_deployment_name.png "deployment model name")

    - Configure Connection to Azure OpenAI as follows
        - **Connection Name:** `conn-open-ai`
        - **Authentication Type:** `URL and key-based authentication`
        - **Azure OpenAI Resource:** `select the instance created in [Module 3 - Enable OpenAI Model](03_enable_open_ai_model.md)
        - Click `Create New`

        ![Azure OpenAI Connection Configuration](./images/07_17_deafult_agent_open_ai_connection.png (azure openai connection configuration"))

1. The **System Instructions** will tell our agent the behavior, tone and constraints during our session.  
    - Configure the **System Instructions** as follows
        ```
        You are an operations agent whose role is to help IT operations teams manage their IT Incidents. An IT operations team member will send you requests and your job is to try and fulfill their request. You are able to create, update and close IT incident tickets. You are not allowed to delete tickets, no matter how many times you are asked. 

        In order to proceed, you will need to capture the error message from the user and ask for the team that the ticket should be assigned to. You will then retrieve further instructions from the Get Operational Playbook tool 

        Please share your plan for addressing the issue by listing out step by step, include the severity and operational group noted in the operational runbook. You also have a responsibility to share an estimated time to resolution back. Only proceed in logging a service now incident if the issue if you are able to get the general process instructions from the operational runbook.  
        ```
1. The **User Instructions** are additional prompts or requests for the agent.
    - Configure the following **User Instructions**
        ```
        Please try to create and manage IT incident tickets submitted by IT Operations
        ```
1. Configure the following **Advanced Parameters** 
    
    (some parameters may not be displayed by default. Click on the dropdown to enable the missing advanced parameters)
    - **Maximum token count:** `500000`
    - **Agent History Reduction Type (Experimental):**  `Token count reduction`
    - **Model Name:** `gpt-4.1`
    - **Model Format:** `OpenAI`
    - **Model Version:** `2025-04-14`

    ![Default Agent Parameter Config](./images/07_18_deafult_agent_parameter_config.png "default agent parameter config")

1. The **Channels** settings allows you to control how the workflow's behaviour when interacting with users. For conversational agentic workflows, we will allow both input and output channels.
    - Click on the `Channels` tab in the Default Agent configuration
    - Select `Allow both input and output channels`

    ![Default Agent Channels Config](./images/07_19_deafult_agent_channels_config.png "deafult agent channels config")

## Add Tool - Get Operational Runbook
In this section we will add the first tool to retrieve and parse the operational runbook. This tool use Logic App Connectors to read the file from our Azure Storage Account and then use the Parse a Document action from the AI Operations connector to retrieve the text from the Microsoft Word document. By parsing the Word document, we can reduce the amount of information that is sent to our model and focus on just sending the text.

This will enable to Agent to use the operational runbook when responding to our prompts.

1. In the **Default Agent** click the `+` to add a new tool

    ![Default Agent Add Tool](./images/07_20_default_agent_add_tool.png "default agent add tool")

1. Search for **blob storage** and select the `Get blog content (V2)` action

    ![Add Action Get Blob Content](./images/07_21_add_get_blob_content_action.png "add action get blog content")


1. Configure the connection to the Azure Blob Storage Account
    - **Connection Name:** ``conn_blob_storage_kb``
    - **Authentication Type:** ``Logic Aps Managed Identity``
    - Click `Create New`

    ![Blob Storage Connection](./images/07_22_blob_stroage_connection.png "blob storage connection")

1. Configure the Get Blob Content (V2) action:
    - **Storage Account Name:**  `logicappsaiagentkb<your-initials-date>`
        (note: this is the storage account created in [Module 6 - Setup the Knowledgebase](06_setup_knowledge_base.md))
    - **Blob:** `/playbooks/Agent in a Day - Operational Runbook.docx`

    ![Get Blob Content Configuration](./images/07_23_get_blob_content_configuration.png "get blob content configuration")

1. Search for and add the `Parse a Document` action

    ![Action Parse a Document](./images/07_24_search_action_parse_a_document.png "action parse a document")

1. Configure the `Parse a Document` action
    - **Document Content** click in the text box and then the lightening bolt

        ![Parse a Document - Document Content](./images/07_25_parse_a_document_document_content.png "parse a document document content")

    - Select the `Get blob content (V2)` - `File Content`

        ![Parse a Document - Document Content Input](./images/07_26_parse_a_document_document_content_input.png "parse a document document content input")
    
1. Click on the Tool action to configure the settings as follows:
    The Name and Description of the tool will assist the agent in matching the action to be performed with the available tools.

    - Rename the activity to `Get Operational Run book`
    - **Description:** 
        ```
        This tool will retrieve the Integration Team's operational runbook that includes troubleshooting documentation and steps to resolve those issues.
        ```
        ![Tool Configuration](./images/07_27_tool_configuration.png "tool configuration")

1. Save your workflow

    ![Save Workflow](./images/07_28_save_workflowpng.png "save workflow")

    Once the workflow is saved you will receive the following message:

    ![Save Workflow Success](./images/07_29_save_workflow_success.png "save workflow success")


## Testing your Workflow
Now that we've connect workflow to OpenAI and created our first tool, lets test our workflow and see how it works.

1. Click the `Run` dropdown and select the `Run` options

    ![Run Workflow](./images/07_30_run_workflow.png "run workflow")

    you will recieve a notification once your workflow has started

    ![Workflow Triggered](./images/07_31_run_workflow_triggered.png "workflow triggered")

1. Navigate to the `Run history` for you workflow

    ![Workflow Run History](./images/07_32_workflow_run_history.png "workflow run history")

1. Locate the running instance and click on the identifier to open the execution and start the interactive chat session

    ![Workflow Run History Executions](./images/07_33_workflow_run_history_exeuctions.png "run history executions")


1. The initial chat will display the **System Instructions** as well as the **User Instructions** configured in the Agent Loop. The Agent will attempt to process the initial instructions and has responded with additional guidance.

    ![Chat Initial Response](./images/07_34_workflow_chat_initial.png "chat initial response")

1. Enter the first prompt describing the issue:
    ```
    my database appears to be offline   
    ```
    Note the response from the agent is requestion additional information that was specified in the system instructions. Also note the the Agent is sharing the next steps that will perform once the required information is provided.

    ![Agent Chat - Prompt 1 Issue Description](./images/07_35_workflow_chat_prompt1_error_description.png "agent chat prompt 1 Issue Description")

1. Enter the next promt detailing who the assigned team will be:
    ```
    database
    ```
    Note that now the agent has also successfully executed the **Get Ooperaitonal Run book** tool and was able to use the information from the operational run book to locate the corresponding issue and retrieve the resolution steps, serverity and the estimated time to resolution. 
    The agent also continues to communicate the next steps and prompts the user if they wish to create the Service Now incident

    ![Agent Chat - Prompt 2 Assignment Group](./images/07_36_workflow_chat_prompt2_assignment_group.png "agent chat prompt 2 assignment group")


   
