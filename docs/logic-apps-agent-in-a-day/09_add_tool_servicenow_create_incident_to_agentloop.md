# Module 9 - Add ServiceNow Create Incident Tool to Agent Loop
In this module we will take the ServiceNow Create Incident workflow and add it as a tool in our Agent Loop workflow.


1. Return to the list of workflows in the Logic Apps by clicking on the Workflows link at the top of the page

    ![Return to workflows](./images/09_01_return_to_workflows.png "return to workflows")

1. Open the `demo-conversational-agent` workflow

    ![Open Agent Workflow](./images/09_02_open_agent_workflow.png "open agent workflow")

1. Add a new tool.
    - Click on the `+`

        ![Add a tool](./images/09_04_add_a_tool.png "add a tool")

1. Search and select the `Call workflow in this logic app`

    ![Add Call workflow](./images/09_05_add_action_call_workflow_in_this_logic_app.png "add call workflow")

1. Configure the **Call workflow in this logic app** action
    - **Workflow Name** - `tool-ServiceNow-CreateIncident`

    ![Configure Call Workflow](./images/09_06_configure_call_workflow.png "configure call workflow")

1. Configure the Tool activity.
    - **NOTE** The way you name your tool and provide a description has a significant impact on your Agent Loop (and LLM) will discover and call your particular tool. Provide unique and descriptive terms to improve accuracy during runtime. 
    - Rename the tool to `Create Service Now Incident`
    - **Description:** `This tool will log a ServiceNow Incident`
    - **Agent Parameters**
        
        for each agent parameter click `+ Create Parameter` 
        - **Name:** `Assignment Group`

          **Type:** `String`

          **Description:** `The Assignment Group in ServiceNow that the ticket should be assigned to`

        - **Name:** `Description`

          **Type:** `String`

          **Description:** `The user provided description of the issue being logged`

        - **Name:** `FailureDateTime`

          **Type:** `String`

          **Description:** `The date and time the error occurred`       

        - **Name:** `ResolutionSteps`

          **Type:** `String`

          **Description:** `The steps recommended to resolve the incident`                       

        - **Name:** `Severity`

          **Type:** `Integer`

          **Description:** `The Severity of the inicdent`      

          ![Configure Tool](./images/09_07_configure_tool.png "configure tool")

1. Configure the inputs paremeters for the call to the logic app.

    **NOTE** when you select a parameter to configure you will now see a third source for mapping your inputs. The agent indicates a parameter that the agent can provide.

    ![Agent Parameters](./images/09_08_configure_assignment_group.png "agent parameters")

    - **Assignment Group:** (agent parameter) `Agent Parameters`-> `AssignmentGroup`

        ![Assignment Group Agent Parameter](./images/09_08_configure_assignment_group_selected.png "assignment gorup agent parameter")
    
    - **Description:** (agent parameter) `Agent Parameters`-> `AssignmentGroup`
    - **FailureDateTime:** `utcNow()`
    - **ResolutionSteps:** (agent parameter) `Agent Parameters`-> `ResolutionSteps`
    - **Severity:** (agent parameter) `Agent Parameters`-> `Severity`

    ![Configure Call Workflow Parameters](./images/09_10_configure_call_workflow_parameters.png "configure call workflow parameters")

1. Save you workflow

    ![Save Workflow](./images/09_11_save_workflow.png "save workflow")

## Test your agent

1. Click `Run` to start your workflow and initiate the agent session.

    ![Run Workflow](./images/09_12_run_workflow.png "run workflow")

    You will receive a notification when the workflow as successfully started  
    ![Workflow Started](./images/09_13_workflow_started.png "workflow started")

1. Navigate to you **Run History** and located you **Running** instance. Click on the identifier to view the executed steps and agent chat session.

    ![Workflow Run History](./images/09_14_workflow_run_history.png "workflow run history")

1. Enter the your issue for the agent to action:

    Similar to the last execution, the agent has first presented it's system intrustions and the initial user intruction has been provided resulting in the agent providing more detail instructions on what it requires and what steps it will take. In the following steps we will provide a similar prompt to the one we tested in [Module 7 - Create Agent Loop Workflow](07_create_agent_loop_workflow.md) but now the agent will have a tool enabling it to log the ServiceNow Incident on behalf of the user.

    - Enter the following prompt:
      ```
      my database mysqldev001 appears to be offline  
      ```
        ![Prompt Issue Description](./images/09_15_workflow_run_user_prompt_1.png "prompt issue description")

    As per the system instructions, the Assignment Group is required to proceed.
    - Enter the `Database` in the chat

    Let's review the response....
    - First, the agent details the step by step plan of what it will do

      ![Agent Response - Part 1 - The Plan](./images/09_15_workflow_run_user_prompt_2_plan.png "agent response part1 the plan")

    - Next, the agent retrieves the operational run book, locates the corresponding issue and presents back a more detailed plan with step by step resolution instructions, estimated resolution time and severity 

      ![Agent Response - Part 2 - Operational Runbook Details](./images/09_15_workflow_run_user_prompt_2_runbook_details.png "agent response part 2 operational runbook details")

    - Finally, the agent will call ServiceNow and create the incident ticket and return the Incident Ticket Number, details that will be included in the ticket and finishes off by providing a list of next steps.

      ![Agent Response - Part 3 - ServiceNow Incident](./images/09_15_workflow_run_user_prompt_2_servicenow_incident_creation.png "agent response part 3 servicenow incident")

1. Vaidate your Incident in ServiceNow.
  - Navigate to your ServiceNow developer portal
  - Use the Incident Ticket Number from the Agent's response to search for the incident in ServiceNow

    ![ServiceNow Incident Details](./images/09_17_servnicenow_incident_detailspng.png "servicenow incident details")


<div align="right" valign="middle">

**Previous:** <a href="08_create_tool_servicenow_create_incident.md" title= "Pevious: Module 8">Module 8: Create Tool - ServiceNow Create Incident</a>  
**Next:** <a href="10_create_tool_servicenow_update_incident.md" title= "Next: Module 10">Module 10: Create Tool - ServiceNow Update Incident</a>      

</div>

   
