# Module 11 - Add ServiceNow Update Incident Tool to Agent Loop
In this module we will take the ServiceNow Update Incident workflow and add it as a tool in our Agent Loop workflow.


1. Return to the list of workflows in the Logic Apps by clicking on the Workflows link at the top of the page

    ![Return to workflows](./images/11_01_return_to_workflows.png "return to workflows")

1. Open the `demo-conversational-agent` workflow

    ![Open Agent Workflow](./images/11_02_open_agent_workflow.png "open agent workflow")

1. Add a new tool.
    - Click on the `+`

        ![Add a tool](./images/11_04_add_a_tool.png "add a tool")

1. Search and select the `Call workflow in this logic app`

    ![Add Call workflow](./images/11_05_add_action_call_workflow_in_this_logic_app.png "add call workflow")

1. Configure the **Call workflow in this logic app** action
    - Rename activity to `tool-update-servicenow-incident`
    - **Workflow Name** - `tool-ServiceNow-UpdateIncident`

    ![Configure Call Workflow](./images/11_06_configure_call_workflow.png "configure call workflow")

1. Configure the Tool activity
    - Rename the tool to `Update Service Now Incident`
    - **Description:** `This tool will update a ServiceNow Incident`
    - **Agent Parameters**
        
        for each agent parameter click `+ Create Parameter` 
        - **Name:** `Notes`

          **Type:** `String`

          **Description:** `Notes to be added to the incident`

        - **Name:** `Ticket Number`

          **Type:** `String`

          **Description:** `The ServiceNow TIcket Number`


          ![Configure Tool](./images/11_07_configure_tool.png "configure tool")

1. Configure the inputs paremeters for the call to the logic app.
    - **TicketNumber:** `@{agentParameters('Ticket Number')}`
    - **Notes:** `@{agentParameters('Notes')}`

    ![Configure Call Workflow Parameters](./images/11_10_configure_call_workflow_parameters.png "configure call workflow parameters")

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

    In this session we will:
    - Ask the agent to help coordinate the resolution for their issue
    - The agent will 
        - retireve the Operation Run and find the corresponding entry
        - will log the incident in ServiceNow
    - We provide updates on our progress of the Incident
    - The agent will
        - Update work notes in ServiceNow

    1.  Enter the following prompt to have the agent log the incident
        ```
        my database mysqldev001 appears to be offline  
        ```
      
        ![Prompt 1 Response - Incident Created](./images/11_12_prompt_response_incident_created.png "prompt 1 response incident created")

        (**note** the agent may prompt you for the assignment group or the date the incident occured.)
   
    1. Enter the following prompt to have the agent update the ticket with the work notes.
        ```
        Database team has provided an update that the database has been rebooted and functionality should be restored in 15 mins
        ```
        (**note** the  agent has leveraged the Update ServiceNow Incident Tool to update our Incident with the work notes) 
        ![Prompt 2 Resonse - Incident Updated](./images/11_13_prompt_response_incident_updated.png "prompt 2 incident updated")

1. Vaidate your Incident in ServiceNow.
  - Navigate to your ServiceNow developer portal
  - Use the Incident Ticket Number from the Agent's response to search for the incident in ServiceNow
   - Note the update has been captured in the work notes for the incident.
    ![ServiceNow Updated Incident](./images/11_14_servicenow_updated_incident.png "servicenow updated incident")


   
